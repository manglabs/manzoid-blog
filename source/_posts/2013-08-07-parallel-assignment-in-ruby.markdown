---
layout: post
title: "Parallel assignment in Ruby"
date: 2013-08-04 04:00
comments: true
categories: ruby programming
---

**Parallel assignment** is a handy feature of the Ruby language (a feature it shares with other languages, such as Python).

Parallel assignment is the way that Ruby handles multiple lvalues and/or multiple rvalues. *(Definitions: an "lvalue" is a value on the left-hand side of the equals sign, and an "rvalue" is a value on the right-hand side of the equals sign.)*

Let's look at some concrete examples.

### Case #1: One value on the left, multiple values on the right

The first, simplest case is when there is 1 lvalue and multiple rvalues. For example:

    > a = 1, 2
    
    > a
     => [1, 2] 
    
<!--more-->

All that's happening is the right-hand comma-separated list of values is converted into an array and assigned to the left-hand variable.

### Case #2: Multiple values on the left, the same number of values on the right

The second case is when there are the same number of lvalues and rvalues. For example:

    > a,b = 'foo', 'bar'
    
    > a
     => "foo" 
    > b
     => "bar" 
    

This is also pretty straightforward -- you simply evaluate each item on the right hand, then assign it to its corresponding variable on the left-hand side, in order.

### Case #3: Multiple values on the left, one value on the right -- but it's an array

The third case is when the rvalue is an array, and that array's elements are distributed (or "expanded") among multiple lvalues. For example:

    > a, b = ['foo', 'bar']
    
    > a
     => "foo" 
    > b
     => "bar" 
    

This is effectively the same as case #2, above, except in this case explicitly employing array syntax.

### Case #4: More values on the left than on the right

The fourth case is when there are multiple values on both sides, and there are more lvalues than rvalues. For example:

    > a, b, c = 'foo', 'bar' # Note only 2 values on the right
    
    > a
     => 'foo'
    > b
     => 'bar'
    > c
     => nil 
    

As you can see, Ruby did the best it could to distribute the values, but ran out of them and was forced to assign `nil` to the last variable on the left.

### Case #5: More values on the right than on the left

The fifth case is when there are multiple values on both sides, and there are fewer lvalues than rvalues. For example:

    > a, b = 'foo', 'bar', 'baz' # Note only 2 values on the left
    
    > a
     => 'foo'
    > b
     => 'bar'
    

Again, Ruby did the best it could to distribute the values, but had too many to parcel out, and was forced to send the right-most value ("baz") into the ether.

### Case #6+: Splatting

Note in the case above, we lost the last value. However it is actually possible to capture any such excess values and gather them into an array, using a related operator called the "splat" operator (which consists of an asterisk in front of the variable, as in `*my_var`). You can do a number of useful things with this "splat" operator, but so as not to overload this post too much, it's probably better to go elsewhere to look at examples of it in action. E.g., [this other blog post][1] lists several variant uses. <br/>

### Note: Swapping without a temp variable

One nice aspect of this parallel assignment facility is that you can swap values conveniently.

To swap values *without* parallel assignment, you might write something like this:

    > a = 'foo'
    > b = 'bar'
    > temp = a # Introduce a temp variable
    > a = b
    > b = temp
    
    > a
     => "bar" 
    > b
     => "foo" 
    

But with parallel assignment, you can simply rely on Ruby to implicitly handle the swapping for you:

    > a = 'foo'
    > b = 'bar'
    > b, a = a, b
    
    > a
     => "bar" 
    > b
     => "foo"

 [1]: http://endofline.wordpress.com/2011/01/21/the-strange-ruby-splat/