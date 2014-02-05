---
layout: page
title: "Front-end engineering questions"
date: 2014-02-04 13:19
comments: true
sharing: true
footer: true

---

## <a name='toc'>Table of Contents</a>

1. [CS: Data Structures and Algorithms: Background Knowledge](#cs)
1. [JavaScript: Background Knowledge](#js)

####[[⬆]](#toc) <a name='js'>Data Structures and Algorithms: Background Knowledge</a>
* What's a linked list?
* What's a hashtable?
* What's a tree? vs a _binary_ tree? vs a binary _search_ tree?
* What's a heap?
* What's Base-64 encoding?

[http://www.nczonline.net/blog/tag/computer-science/](http://www.nczonline.net/blog/tag/computer-science/)

####[[⬆]](#toc) <a name='js'>JavaScript: Background Knowledge</a>

##### Variables and type

* What are the types in JavaScript?
* What's the difference between `==` and `===`?
* What's the difference between a variable that is: null, undefined, or undeclared, and how to check for these states?
* What's "hoisting"?

##### Objects

* What's the difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
* What's the difference between an "attribute" and a "property"?
* When is extending built-in JavaScript objects a good idea? When is it *not* a good idea?

##### Functions

* What's the "arity" of a function? Why might you care about that, practically speaking?
* What's the difference between `.call` and `.apply`?

##### Scoping

* How does scoping work in JavaScript?
* How does `this` work in JavaScript?
* What's `Function.prototype.bind`?
* What's a closure, and how/why would you use one?
* What are anonymous functions, and how/why would you use one?
* What's an IIFE (Immediately Invoked Function Expression)? see [http://benalman.com/news/2010/11/immediately-invoked-function-expression/](link)
* Why doesn't the following work as an IIFE: `function foo(){ }();`? How to fix it?

##### Inheritance

* How does inheritance work in JavaScript.
* What are some useful inheritance patterns in JavaScript?

##### Events

* What's event bubbling?
* How do the browsers differ in how they handle events?
* When would you use `stopPropagation`?
* What's event delegation?

##### Execution environment

* What's the difference between host objects and native objects?
* What are some features that the browser provides, not JavaScript itself?
* What are the practical differences between server-side JavaScript and client-side JavaScript?
* What's Node.js?
* Why was Node.js created? What's it good for? What's it not good for?

##### Functional programming

* What's functional programming? Practical pros/cons?
* What functional programming features does JavaScript offer?

##### Server interaction

* What's AJAX? (give some technical detail on how it works).
* What's JSONP? (and how it's not really AJAX).
* 
* What options are there for server-push interaction?

##### Browser behavior / interaction

* When would you use `document.write()`?
  * Most generated ads still utilize `document.write()` although its use is frowned upon
* Difference between document load event and document ready event?
* How would you get a query string parameter from the browser window's URL?
* What's the difference between feature detection, feature inference, and using the UA string?
* Explain the browser's same-origin policy with regards to JavaScript.

##### Optimization

* When do you optimize your JavaScript code?
* Describe a strategy for memoization (avoiding calculation repetition) in JavaScript.
* What's "perceived load time" versus precisely-measured load time, and how can that be minimized?

##### Code organization / tidiness

* How do you organize your code? (module pattern, classical inheritance?)
* Explain the "JavaScript module pattern" and when you'd use it.
  * Bonus points for mentioning clean namespacing.
  * What if your modules are namespace-less?
* AMD vs. CommonJS?
* What's `"use strict";`? Advantages and disadvantages to using it?

##### Libraries

* What are some popular JavaScript libraries? Compare/contrast.
* What are some practical use cases / reasons for using one of these libraries? When might you _not_ use one?
* What do frameworks like "Sencha" do? Why might you want to avoid them?

##### Testing

* How do you test your JavaScript?
* What are the major testing libraries? Which one do you prefer and why?

##### UI/UX

* What's JavaScript templating and why is it used?
* Describe how one of the major templating libraries works. (Mustache.js, Handlebars etc.)
* What's the difference between responsive design and adaptive design?
	* How might JavaScript come into play to make such design styles work?


