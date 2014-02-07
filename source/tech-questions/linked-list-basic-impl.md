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

*Note that you could also choose to name the "next-pointer" property `succ` (instead of `next`) as per Ruby's [standard #succ method](http://www.ruby-doc.org/core-2.1.0/Integer.html#method-i-succ).*

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

That style does make the code shorter. I think that it also makes the code more confusing however, because the name doesn't really describe what it is pointing to at that moment. If practical, I throw in another local variable to try to make it clearer. Anyway, just gird your loins for code that re-uses `head`, at some point.

# Counting, Ruby-style

The counting function above doesn't feel much like Ruby. In Ruby, we'd be looking to pass a block to one of the usual iterator methods defined by the [Enumerable mixin](http://ruby-doc.org/core-2.1.0/Enumerable.html).

It's easy-peasy to get our little Node class to work that way, at least for the purposes of counting.

As usual, we just need to declare the mixin:

asdfasdf

``` ruby foo start:1 mark:2
class Node	
  include Enumerable
  attr_accessor :value, :next
end
```

Then define `#each`:

