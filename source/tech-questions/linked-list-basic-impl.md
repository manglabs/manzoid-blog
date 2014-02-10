---
layout: page
title: "Linked lists: Basic implementation"
comments: true
sharing: true
footer: true

---

# Setup 

Let's throw together a very simple linked list implementation in Ruby, build it up into something we can play with comfortably, then tackle a few common interview problems using that as a base.

Here's all we need, to start with:

``` ruby
class Node	
  attr_accessor :value, :next
end
```

(We could also use a Struct for something this simple, but let's stick with a Class.)

With this bare-bones start, right away we can create ourselves a bona fide linked list:

``` ruby
	# Create the head
	head = Node.new
	head.value = "julie"
	
	# Create the 2nd node
	nodeB = Node.new
	nodeB.value = "dylan"
	
	# Link head -> nodeB
	head.next = nodeB
	
	# Create the 3rd node
	nodeC = Node.new
	nodeC.value = "nathan"
	
	# Link nodeB -> nodeC
	nodeB.next = nodeC
	
	# Create the 4th node
	nodeD = Node.new
	nodeD.value = "john"
	
	# Link nodeC -> nodeD
	nodeC.next = nodeD
	
	# Link nodeD -> null, it's the tail
	nodeD.next = nil
```

Now in memory we will have the following structure:

![image](http://i.imgur.com/Ifw95F7.png)

# Counting

Let's count how many nodes we have. We just walk the list until we hit null.

``` ruby
def count(head)
  count = 1
  node = head
  while node = node.next
    count += 1
  end
  count
end

puts count(head) # 4
```

Let me pause to point out line 3 above:

	node = head
	
Note that it's customary, in linked list code, to re-use the `head` variable in the body of the function as a placeholder for whatever the current node is, as you are walking the list. That would look like this in our simple case:

``` ruby
def count(head)
  count = 1
  while head = head.next
    count += 1
  end
  count
end
```

That style does make the code shorter. I think that it also makes the code more confusing however, because the name doesn't really describe what it is pointing to at that moment. If practical, I try to throw in another local variable to make things a bit clearer. Anyway, just gird your loins for code that re-uses `head`, at some point.

# Counting, Ruby-style

The counting function above doesn't feel much like Ruby. In Ruby, we'd be looking to pass a block to one of the usual iterator methods defined by the [Enumerable mixin](http://ruby-doc.org/core-2.1.0/Enumerable.html).

Fortunately, it's easy-peasy to get our little Node class to work that way, at least for the purposes of counting. We just declare the mixin then define a simple `#each` implementation:

``` ruby
class Node
  include Enumerable
  #...
  
  def each(&block)
    yield self
    self.next.each(&block) if self.next
  end  
end
```

Now we get a nice `#count` for free!

	puts head.count # 4

*Sidebar #1: we could also have chosen to name the "next-pointer" property `succ` (instead of `next`) as per Ruby's [standard #succ method](http://www.ruby-doc.org/core-2.1.0/Integer.html#method-i-succ). This would avoid the need for scoping `next` to `self.next` to avoid the collision with Ruby's built-in `next` keyword. But, no biggie.*

*Sidebar #2: this implementation of `#each` isn't quite formal recursion, but it's very close. The function isn't calling itself directly, it's calling a clone of itself residing in the next instance. But this creates a chain of calls that is quite similar to a recursive call tree. And when you throw in the fact that the function in question actually lives in the class that every instance shares, so it really is the same function code being executed, in memory, then I think you can reasonably consider this to be recursive style.*

# Printing values

With `#each` established, now we can trivially print the contents of the list:

``` ruby
def print_list(head)
  s = ""
  head.each do |node|
    s << "|#{node.value}| -> "
  end
  s
end
```

Running this looks like:

```ruby
puts print_list(head) 
# |julie| -> |dylan| -> |nathan| -> |john| -> 
```

In Ruby, it'd be more natural for this facility to be implemented as a class method rather than a standalone function. Let's move this to the usual `#to_s` override:

``` ruby
class Node	
  #...
  
  def to_s
    s = ""
    self.each do |node|
      s << "|#{node.value}| -> "
    end
    s
  end
end
```

Note that since this facility will be present in any node, you have to call `#to_s` on the actual head node in order to print the entire list from beginning to end. But that was true in the original standalone function too.

# Enumerable is Array-centric

While we have added Enumerable in order to enjoy some of its automagical benefits for counting, etc, it is not as automagical as it normally would be. This is because Enumerable methods are fundamentally meant to manipulate Arrays, so the standard implementations of methods such as `#map` will give you back an array:

```ruby
foobar = head.map { |node| node = node.value + "foo"; node; }
puts foobar.to_s
# ["juliefoo", "dylanfoo", "nathanfoo", "johnfoo"]
```

Note the return value -- we no longer have a linked list, we have an array. That may be fine for some use cases, of course, but it's not an intuitive result.

Let's fix it. 


## Roll your own #map

It's not that big a deal to create your own version of `#map` that works properly with the linked list:

```ruby
class Node
  #...
  def map
    head = nil
    prev = nil
    each_with_index do |node, index|
      new_node = Node.new
      new_node.value = yield(node.value)
      index == 0 ? head = new_node : prev.next = new_node
      prev = new_node
    end
    head
  end
```

#### Notes

* Line 6 &mdash; `Enumerable` gives us `#each_with_index` for free.
* Line 8 &mdash; Allow the passed-in block to change the node value as desired.
* Line 9 &mdash; In the first iteration, there is no `prev` node yet.

## Roll your own #map!

Regular `#map`, like all the other `Enumerable` methods, is meant to be used in "functional"-flavored programming. That means in this case that it's not supposed to change the original value, and instead return a new one. That's why we had to create a new linked list on the fly in the code above.

Implementing the destructive (aka "non-idempotent") variant of `#map` is trivially easy, since we don't have to make a new list, we just modify (aka "mutate") the existing one in-place.

```ruby
class Node
  #...
  def map!
    each do |node|
      node.value = yield(node.value)
    end
    self
  end
```

# Easy list initialization

While we're talking about arrays, why don't we add a convenience factory method for generating a linked list from an array of values. It's tedious to construct them manually like we did at the beginning of this post.

It probably makes the most sense to implement this as a class method rather than an instance method:

```ruby
class Node
  #...
  def self.from_a(array)
    return nil if array.empty?

    head = Node.new
    head.value = array.first

    prev = head
    array[1..-1].each do |value|
      node = Node.new
      node.value = value
      prev.next = node
      prev = node
    end

    head
  end
```

Then it's easy-peasy to construct a new list:

	head = Node.from_a ['fee', 'fi', 'fo', 'fum']
	puts head
	> |fee| -> |fi| -> |fo| -> |fum| -> 


