---
layout: page
title: "What are the types in JavaScript?"
comments: true
sharing: true
footer: true

---

## tl;dr

At its very simplest, we can think of JavaScript as having just these few types:

* numbers
* strings
* booleans
* objects

Where an object is just a bag of key/value properties. An object property whose value is a function is also known as an object method.

Most people would throw in arrays as well, but personally I think they're so similar (see below) that to get at the bare, minimal essence of JavaScript, you can stick with the list above as your core mental model.

---

## The full (but still short) story

To flesh out the story for practical use, JavaScript types are divided into two categories: "Primitive" and "Object".

* **Primitive types**
	* number
	* string
	* boolean
	* primitive *values* that are effectively their own type
		* null
		* undefined
* **Object types**
	* Object
		* Array
		* Function
		* host-defined objects
		* user-defined objects
		* global object

Let's look at each of these variations:

### null
(Program-level, normal, expected) absence of value.

### undefined
(System-level, abnormal, unexpected) absence of value.

Note that `null` is a language keyword whereas `undefined` is a predefined global variable (see below).

### Array

In JavaScript, Arrays are basically just Objects. Array indexes are little more than property names that happen to be integers. You can access properties on a regular object with square brackets.

	myNonArrayObj['foo']
	
is the same as 

	myNonArrayObj.foo
	
This is why you can access Array indexes with square brackets, it's not special to Arrays.

Conversely, you could also use numeric property names on non-array objects, if you felt like it (which does *not* magically turn them into Arrays):

	myNonArrayObj[1] = 'blah';

So, what's special about an Array? The interesting (externally-facing) thing about an Array versus a plain Object is that it automatically maintains a `length` property for you, depending on the values of any **non-negative integer property names** it has.

For example, in Chrome:

	var x = [];
	x.length; // 0

	x['foo'] = 10; // add a value at a non-numeric property name -- won't do anything
	x.length; // still 0
	
	x.push('a'); // add a value at index 0
	x.length; // 1
	
	x[100] = 'b';
	x.length; // 101

	x.push('c'); // add a value at index 101
	x.length; // 102
	
	x.unshift('d'); // add a value at index 0
	x.length; // 103


Of course, Array objects also ship with plenty of built-in convenience methods for manipulating themselves: `join()`, `slice()`, `indexOf()` and whatnot.

### Function

Since Functions, like Arrays, are just Objects, you can set properties on them, and invoke methods on them. In particular you are likely to be mucking around with the `prototype` property, which every JavaScript function automatically has (except those that are created by `Function.bind()`).

### the global object

The "global object" is a regular JavaScript object that serves a unique, critical purpose -- as the name suggests, this is where all the global stuff lives!

* global properties: `undefined`, `Infinity`, `NaN`
* global functions: `eval`, `parseInt`, `isNaN`
* global constructors: `Date()`, `RegExp()`, `String()`, `Object()`, `Array()`
* (other) global objects: `Math`, `JSON`

### user-defined objects

This is easy-peasy, just plain old objects you define yourself. :)

### host-defined objects

Useful stuff that the host environment injects into the global scope for your implementation pleasure. These generally have official specs, but not always.

Some browser examples:

* console
* Window
* Document
* Element
* Event* XMLHttpRequest* Storage
* Canvas

Some Node.js examples:

* console
* process
* Buffer
* require
* __filename
* __dirname
* module
* exports







