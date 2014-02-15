---
layout: page
title: "What is hoisting?"
comments: true
sharing: true
footer: true

---

# An odd snippet

Have a glance at this trivial script:

```javascript
  var petName = 'Maggie'; 
  function printPetName() {
    console.log("Pet name is: " + petName);
  }
  printPetName();
```
This, of course, prints out the following:

	Pet name is: Maggie 

Now check out this small modification (note line 4):

```javascript
  var petName = 'Maggie'; 
  function printPetName() {
    console.log("Pet name is: " + petName);
    var petName = 'Maggie'; // added
  }
  printPetName();
```

This version, oddly, now prints out:

	Pet name is: undefined

This happens even though the variable `petName` seems to be properly declared and defined in both the global scope and the function-local scope.

What gives?

# Hoisting

Here's the backstory to the strange behavior above. Check this out:

```javascript
  console.log("before definition: " + someVar);
  var someVar = "I was used before I was declared. I feel violated.";
  console.log("after definition: " + someVar);
```

In many other languages, that would just buy you an error on line 1, when you try to use `someVar` before it was declared.

However, in JavaScript, you get this output instead:

	before definition: undefined
	after definition: I was used before I was declared. I feel violated. 

If you just try to run line 1 all by itself, you'd get this:

	Uncaught ReferenceError: someVar is not defined 
	
What's going on here is a JavaScript feature called "hoisting".

# Why is it called "hoisting"?

It's called "hoisting" because the variable declarations are "hoisted" up to the top of the scope.

Let's put the code inside a function to make that a bit clearer.

```javascript
function test() {
  console.log("before definition: " + someVar);
  var someVar = "I was used before I was declared. I feel violated.";
  console.log("after definition: " + someVar);
}
```

Here's what that looks like under the hood, after the "hoisting" pass is done:

```javascript
function test() {
  var someVar;
  console.log("before definition: " + someVar);
  someVar = "I was used before I was declared. I feel violated.";
  console.log("after definition: " + someVar);
}
```

# Revisiting the odd snippet

Let's rewrite the original odd snippet in the hoisting-under-the-hood style:

```javascript
  var petName = 'Maggie'; 
  function printPetName() {
    var petName;
    console.log("Pet name is: " + petName);
    petName = 'Maggie';
  }
  printPetName();
```

Now you can see why it gave us the weird output:

	Pet name is: undefined

THE END.
