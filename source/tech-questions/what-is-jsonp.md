---
layout: page
title: "What's JSONP?"
comments: true
sharing: true
footer: true

---
<style>
td {
	border:1px solid black;
	padding: 10px;
}
</style>

JSONP stands for "JSON with Padding".

You know that **JSON** is a serialization of a JavaScript object's data properties for transport over the wire. 

Well, **JSONP** is a variation of JSON with two major features:

1. use script-injection to get around the browser's same-origin policy
1. use wrapper code (the "padding") to pass the payload to its destination within your code

# Same-origin policy

An early version of Netscape introduced "same-origin policy" to sandbox resources from different sites so they could not see each other. Since JavaScript code could run in the browser, they needed a security measure that a website could rely on to wall off potentially malicious code from other parties.

## How do you define "origin"?

An "origin" in this context is defined as a URL to which you can send HTTP requests having the following identical characteristics:

<table>
<tr><td>The same protocol</td><td>(can't switch http and https)</td></tr>
<tr><td>The same domain</td><td>(can't switch www.manzoid.com and blog.manzoid.com)</td></tr>
<tr><td>The same port</td><td>(can't switch port 80 and port 8080)</td></tr>
</table>

## No spec

Note that there is no spec for how same-origin policy works, although it has become ubiquitous across all browsers.  For example, unlike other browsers, Firefox has (or at least had, I haven't checked recently) this strange "feature" where _fonts_ couldn't be shared across domains. Other browsers would allow that.

Anyway just be aware that this is almost-but-not-quite standardized.

# Script injection

One interesting and giant hole in same-origin policy is that script tags can point anywhere.

I can run this, no problem, on manzoid.com:

	<script src="http://foo.com/foo.js"></script>

... even though it's loaded from foo.com.

We can use this to get data from a 3rd-party API -- i.e., on a page that was served from our domain, not theirs!

However, you have to have a bit more infrastructure in order to effectively get data from a 3rd-party API that way.

# Padding

This is where "padding" comes in.

Let's say you issue a GET request to `manzoid.com/stuff/1` -- via XHR on a page served by manzoid.com -- and get back this JSON:

	{"name":"tim","iq":90}

Now you try using script injection to get that same payload from a page served by juliekang.net.

	<script src="http://manzoid.com/stuff/1"></script>
	
Well that will work, up to a point. It will just give you a JavaScript error upon arrival, because that snippet by itself is not a valid JavaScript expression.
 
*But* if you pass a parameter that indicates the target function within your code, then the 3rd-party server (manzoid.com) can generate some "padding" script around the data payload. When that padding script executes, it will deliver the data payload to the function of your choice.

E.g., 

	<script src="manzoid.com/stuff/1?callback=myTargetFunc"></script>
	
And if manzoid.com were JSONP-enabled (i.e., the app developers had written code to support this parameter), then you'd get back:

	foo({"name":"tim","iq":90});
	
**Now** you can get your data!



 
 
 
 
 