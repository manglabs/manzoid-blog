---
layout: page
title: "Javascript: null, undefined, undeclared"
comments: true
sharing: true
footer: true

---

What's the difference the following flavors of "no value" in JavaScript?

* `null`
* `undefined`
* undeclared (i.e., non-existent variable names)?

To examine this, let's start with `undefined` versus `null`, then look at the undeclared case.

# undefined vs null

## undefined

Here is an undefined variable.

	var backpackContents;

If you use `typeof` on this variable, it will return `undefined`.

	> typeof(backpackContents);
	undefined

## null

Here is a null variable.

	var preferredSnack = null;

If you use `typeof` now, you might expect it to return `null`. But that's not what happens.

	> typeof(preferredSnack);
	Object
	
This is because `null` is treated as a special, singleton sub-class of Object.

In general, `null` and `undefined` are used interchangeably to represent nothing-ness. However, it's a bit tidier to use `null` because it expresses intent, whereas `undefined` might just be a mistake.

# How to check for null vs undefined?

You can use the [triple-equals](what-is-double-vs-triple-equals.html) comparison operator in both cases:

	> backpackContents === undefined
	true
	
	> preferredSnack === null
	true
	
However, for the undefined state, it's more common in my experience to check with the `typeof` operator:

	> typeof backpackContents === "undefined"
	true

The rationale for sticking with `typeof` (to the extent that people bother to articulate a rationale) used to be that it's possible to re-define `undefined`, either intentionally or accidentally. However that's kind of a lame argument, because:

1. in JavaScript practically everything can be redefined, so who cares about that risk really -- it comes with the territory
1. you can't arbitrarily redefine `undefined` anymore in modern browsers, that was fixed in ECMAScript v5, which was released in 2009 and is widely implemented now.

So, personally I wouldn't woof if you used either way of checking undefined-ness.
 

# How to check for undeclared vars?

I've never seen the need for this in production code, so this feels like a toy question designed to test your knowledge of Javascript's quirky edges.

Anyway, for what it's worth...

## The "in" trick

Assuming that you are looking for a variable that was supposedly declared in the global scope, you can theoretically use this `in` trick to check for the variable name. This is because the variable name will also be a property name on the [global object](what-are-types-in-javascript.html#global-object) (which if the JavaScript is executing inside a browser, will be named `window`):

	> "myNonexistentShiz" in window
	false
	
	> var myDeclaredVariable;
	> "myDeclaredVariable" in window
	true
	
Note that you can't use the dot-operator to check similarly, because both undeclared and declared-but-as-yet-undefined variables both get reported as "undefined".

	> window.myNonexistentShiz
	undefined
	
	> window.myDeclaredVariable
	undefined

## The "ReferenceError" trick

In theory you could do this, too:

```javascript
	try {
	  foo;
	} catch(ReferenceError) {
	  console.log("wut");
	}
```

Again I've never seen anyone bother with this stuff in reality. Normally you would just see undeclared variables via the console log because that shit would be breaking.

So this section is here just so you can wow people at interview, I guess. :)

