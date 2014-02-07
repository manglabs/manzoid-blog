---
layout: page
title: "What's a linked list?"
comments: true
sharing: true
footer: true

---

# tl;dr

The idea of a linked list is simple: you define a node structure that contains a value and pointer to the next node in the list. That's it.

![image](http://i.imgur.com/T00ThFs.png?1)
	
A linked list is useful because it can grow indefinitely in memory, and does so in constant time. It doesn't have to copy its entire contents just to grow, it doesn't have to pause to re-index itself in any way. Insert/delete is cheap (but lookup is expensive).

The usual compare-and-contrast data structure for a linked list is the traditional (fixed) array, where you have to pre-allocate memory, re-allocate memory and copy the buffer in order to resize, and lookup by index is cheap but insert/delete is expensive.

Linked lists are one of the most fundamental data structures. They are a building block for higher-level structures and facilities that are commonly used directly by programmers in their daily work.

# Common variations

If there is a single pointer to the next node, it's called a **singly-linked** list.

If there are 2 pointers, i.e., to both the node before and the node after the current node, then that is called a **doubly-linked** list. It's possible to have even more than 2 pointers (triply-linked, and so on), though that's more rare.

It's also common to have a **circular** linked list, which means that the tail points back to the head rather than to `null`. This is useful for domains that are inherently "circular" or "rotate" (for example, going round-robin around a set of available resource slots).


# How are linked lists used?

The linked list is a venerable old data structure that (depending on the domain) is less often used *directly* these days, but is a hidden building block of other important things that we use all the time.

A few examples:

* *call stacks* can be implemented as linked lists of stack frames.
* *hashtables* that feature "[chaining](http://en.wikipedia.org/wiki/Hashtable#Separate_chaining)" to resolve collisions can implement that with linked lists.
* *file systems* can use them. E.g., Linux keeps track of mount points with linked lists.
* *operating system kernels* (or any app) can use *circular* linked lists (see below) for round-robin resource sharing -- e.g., timesharing of a CPU.

Also, this isn't practical but it's nerdalicious:

* *Lisp* lists (from which Lisp takes its name) are singly-linked lists.


The ubiquity of linked lists is quiet and understated. There are definitely consumer/web application programmers (as opposed, say, to embedded/realtime/game/system programmers) who go their entire careers without ever once seeing a linked list used in production code.

So while linked lists are certainly used and useful, I wouldn't worry too much right now about studying them for your own actual use. It's just helpful to know that they exist and are part of the ecosystem.

# Linked lists in interviews

Where linked lists _really_ crop up for the typical consumer/web application programmer is in... **interviews**! It's one of the most popular categories of interview questions.

Why is that? It's because linked lists offer a quick, self-contained test of some important things:

* Can you work with pointers?
* Can you visualize a set of relationships, and keep that mental model in mind clearly enough to manipulate those relationships?
* Can you solve problems despite not having constant-time lookup?
* Can you visualize a call stack?
* Can you comprehend and implement recursion?

The remainder of this post will walk through some typical linked list stuff that tends to crop up in interviews.

Before we begin, note that the interviewer will almost always be talking about a singly-linked, non-circular list. You can lightly mention, "Just to verify, I assume you're talking about a singly-linked list, right?" if you want to show off a bit, but it's not really necessary :)

# Next up 

Let's move on to [basic implementation of linked lists](linked-list-basic-impl.html).

<br>





