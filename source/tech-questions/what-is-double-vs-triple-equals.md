---
layout: page
title: "What is double- vs triple-equals in JavaScript?"
comments: true
sharing: true
footer: true

---

# tl;dr

Double-equals type-casts (or "coerces") values before comparing them, whereas triple-equals compares values as-is.

**Takeaway**: Double-equals can lead to surprising, non-intuitive results, which leads to gnarly bugs. Nowadays there is strong sentiment in the JavaScript community to always use triple-equals.

<br>

# The details

The two comparison styles are defined in the ECMAScript (see below) specification: [strict equality algorithm](http://ecma-international.org/ecma-262/5.1/#sec-11.9.6) and the ["abstract" (loose) equality algorithm](http://ecma-international.org/ecma-262/5.1/#sec-11.9.3).

## Triple-equals

Let's look at the strict equality algorithm in detail, since it is short and it's the recommended one to use. I've rewritten it here for increased clarity:

Given two values, x and y...

<style>
  th {
    font-size: 1.1em;
  }
  td, th {
    text-align: left;
    border:1px solid gray;
    padding:3px;
  }
  h4 {
    margin-top: 20px;
  }
</style>
<table style="margin-bottom:30px">
<tr><th style="width:99%">Case</th><th>Outcome</th></tr>
<tr><td>If the types of x and y are different</td><td><strong>unequal</strong></td></tr>
<tr><td>If x and y are both <code>undefined</code></td><td><strong>equal</strong></td></tr>
<tr><td>If x and y are both <code>null</code></td><td><strong>equal</strong></td></tr>
<tr>
	<td colspan="2"><strong>If x is a number, then...</strong>
		<table style="width:98%;margin-left:20px;margin-top:20px">
			<tr><td style="width:99%">If x or y are <code>NaN</code></td><td><strong>unequal</strong></td></tr>
			<tr><td>Note: that includes <em>both</em> being <code>NaN</code></td><td><strong>unequal</strong></td></tr>
			<tr><td>If x and y have the same number value</td><td><strong>equal</strong></td></tr>
			<tr><td>If x and y are both zeroes and one is <em>negative</em>*</td><td><strong>equal</strong></td></tr>
			<tr><td>Otherwise, if you get to this point...</td><td><strong>unequal</strong></td></tr>
		</table>
	</td>
</tr>
</table>

### The signed-zero business 

\* - The "[signed-zero](http://en.wikipedia.org/wiki/Signed_zero)" business is not *JavaScript* being annoying, it is *computer arithmetic* being annoying. Signed zeroes are defined in the venerable old floating-point arithmetic standard -- first published in 1985 and recently updated in 2008: [IEEE 754](http://ieeexplore.ieee.org/xpl/articleDetails.jsp?arnumber=4610935&sortType%3Dasc_p_Sequence%26filter%3DAND%28p_IS_Number%3A4610934%29). Here's an [authoritative explanation](http://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html#924). The basic idea is that dividing by a signed zero changes the sign of the resulting infinity value, and that is apparently useful when doing some math with complex numbers. But all it does for you, in normal usage, is create the opportunity for a bug.

### Shallow, not deep... "We must go deeper"

And one other thing to note is that JavaScript will only ever compare "shallow", scalar values. It doesn't look at object structure.

E.g.,


	var x = { 'a': 123 };
	var y = { 'a': 123 };
	console.log(x == y); // false
	console.log(x === y); // false

There are certainly times when you want a "deep" object comparison (e.g., for sorting an array of objects). In that case you have to roll your own comparator function, using your context-specific knowledge of structure.

---

## Double-equals

So anyway, that's triple-equals comparison. What's double-equals comparison?

### Short answer: avoid it. 

To quote Douglas Crockford's excellent [JavaScript: The Good Parts][1]:

> JavaScript has two sets of equality operators: `===` and `!==`, and their evil twins `==` and `!=`.  The good ones work the way you would expect.  If the two operands are of the same type and have the same value, then `===` produces `true` and `!==` produces `false`.  The evil twins do the right thing when the operands are of the same type, but if they are of different types, they attempt to coerce the values.  the rules by which they do that are complicated and unmemorable.  These are some of the interesting cases:

>     '' == '0'           // false
>     0 == ''             // true
>     0 == '0'            // true
    
>     false == 'false'    // false
>     false == '0'        // true
    
>     false == undefined  // false
>     false == null       // false
>     null == undefined   // true
    
>     ' \t\r\n ' == 0     // true

> The lack of transitivity is alarming.  My advice is to never use the evil twins.  Instead, always use `===` and `!==`.  All of the comparisons just shown produce `false` with the `===` operator.

Basically, it's just asking for weird bugs.

### Also, stuff you will use will be using it

Nowadays, the internal comparisons made by useful things like `Array#indexOf` will be done via strict comparison. This can trip you up if you aren't expecting it... so expect it!

See [Mozilla doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf):

> `indexOf` compares `searchElement` to elements of the `Array` using strict equality (the same method used by the ===, or triple-equals, operator).

### And it's faster

Another practical consideration is that triple-equals is faster than double-equals (naturally, it's doing less work). For typical light processing, that won't matter, but sometimes when dealing with lots of data etc that can matter.

	


## Sidebar: What's "ECMAScript"?

For practical purposes, given the kind of development you intend to do, you can think of it as being synonymous with "JavaScript".

ECMA is a standards body. Its name used to mean "European Computer Manufacturers Association", but now they hide that origin to be more global, and just call themselves "Ecma".

ECMA maintains the standard for ECMAScript, since Netscape asked them to take it on, back in the day.

ECMAScript is the basis for 3 popular languages: JavaScript, JScript (Microsoft's flavor, used in [.NET](http://www.microsoft.com/net) stuff), and [ActionScript](http://www.adobe.com/devnet/actionscript.html), used in Adobe Flash and Adobe Air (the desktop version of their stuff).

More detail here: [link](http://en.wikipedia.org/wiki/ECMAScript).



[1]: http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742
