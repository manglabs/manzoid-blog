---
layout: page
title: "What is a doctype?"
date: 2014-02-04 13:19
comments: true
sharing: true
footer: true

---

### tl;dr Always put one at the top of all your HTML pages.

Specifically it should look like this:

	<!DOCTYPE html>

It needs to be the first line. Don't add anything before that line or it triggers quirks mode (see below) in IE versions before v10.

## What is a "doctype"?

A doctype tells the browser's markup parsing engine how to interpret the HTML text that it finds. If you forget to specify one, the processing style is undefined, which means that the browser will drop back into what is known as "Quirks Mode", which has been considered Evil for a long time now and should never be invoked.

Side note: here is [way more detail](https://hsivonen.fi/doctype/) than you probably want on the topic of doctypes. If you just want the gist, carry on...


## What is "Quirks Mode"?

Back in the bad old days of the browser wars in the late 1990's, Microsoft and Netscape implemented their respective browsers such that they interpreted HTML in slightly or very different ways. Entire features would be implemented and available in one browser only.

On top of that, people were writing poorly-formed HTML and copy-pasting those bad snippets all over the web. The browser vendors didn't want pages to "crash" (render poorly or not at all) in the face of unclosed tags and mismatched tags and whatnot (what came to be called "tag soup") -- so they wrote extremely forgiving HTML parsers. So real websites wound up full of junk, but still somehow or another managing to render.

It was basically all a frakking mess.

The W3C (see below) attempted to clean up said mess by issuing standards documents (see below). These were partially implemented by the browser vendors, but could not be fully implemented in one go. Specifically, what were they to do with all these existing sites full of junky, partially-proprietary HTML? 

In response, the vendors introduced two modes to handle newer standards-compliant websites differently from legacy websites: "standards mode" for the new stuff, and "quirks mode" for the old stuff.

Full standards mode follows the W3C specs. Quirks mode emulates what NN4 and IE5 (for Windows, not the Mac IE5) did. 

Then a third was added, "almost standards mode". "Almost standards" mode is basically full standards except that vertical sizing of table cells is done according to quirks style, not according to the CSS2 spec.


## Sidebar: A _very_ brief history of browser wars and standardization

* 1993 - CERN releases source code for their hypertext system into public domain
* 1993 - NCSA releases Mosaic, with installers for Mac and Windows
* 1994 - Marc Andreessen launches Netscape Communications, Inc
* 1994 - Tim Berners-Lee founds W3C at MIT
* 1995 - Microsoft releases Internet Explorer 1.0
* 1995 - W3C releases first HTML spec (called HTML 2)
* 1996 - W3C releases CSS 1 spec
* 1996 - Netscape has ~80% market share, Microsoft begins bundling IE with Windows
* 1996 - IE introduces the `<iframe>` tag
* 1997 - W3C releases HTML 4 spec
* 1998 - IE introduces `XMLHTTP`, which would power Web 2.0 some years later.
* 1998 - Browser market is dominated by IE4 and Netscape Navigator 4
* 1998 - Beta version of IE5 implements "DHTML" (Dynamic HTML)
* 1999 - W3C releases HTML 4.01 spec
* 2000 - CSS2 spec nears completion, spec writers already starting work on a bunch of CSS3 modules.
* 2000 - W3C completes XHTML 1.0 spec
* 2002 - IE has 96% market share (!)
* 2002 - Apple forks KHTML to create WebKit, the foundation for its Safari browser (and later, Google Chrome)
* 2004 - Breakaway group of browser developers forms [WHATWG](http://www.whatwg.org/) to push web specs forward more quickly and pragmatically.
* 2004 - Mozilla Firefox launched (aka Gecko)
* 2004 - Apple releases first version of `<canvas>` tag for WebKit/Safari.
* 2005 - `<canvas>` adopted in Gecko/Firefox.
* 2005 - WHATWG issues [Web Applications 1.0 spec](http://www.whatwg.org/specs/web-apps/2005-09-01/)
* 2007 - W3C forms a working group for the next version of HTML, and adopts WHATWG's "HTML 5" spec as a starting point.
* 2008 - Netscape brand completely shut down in favor of Firefox
* 2008 - Google Chrome launched, based on Webkit
* 2012 - [HTML5 spec](http://www.w3.org/TR/2012/CR-html5-20121217/) and [HTML Canvas 2D spec](http://www.w3.org/TR/2012/CR-2dcontext-20121217/) complete.
* 2013 - Google forks Webkit into "Blink" (controversial move)
* Present - CSS3 has over 40 modules in various stages of completion.


## What doctypes are there?

There are several doctypes, each corresponding to different flavors of HTML specification.

Here are some listed in rough order of their release.

**HTML 4.01 Strict**

	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN"
		"http://www.w3.org/TR/html4/strict.dtd">
   		
**HTML 4.01 Transitional**

	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN"
		"http://www.w3.org/TR/html4/loose.dtd">
	   	
**XHTML 1.0 Strict**

	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
		"http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
   
**XHTML 1.0 Transitional**

	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
		"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

**XHTML 1.1**

	<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" 
		"http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd">

**HTML 5**

	<!DOCTYPE HTML>

## Huh? What's the difference between all these?

There are 3 flavors of HTML represented by all those doctypes above, along with some "transitional" variations (slightly looser-interpreting for backwards-compatibility):

* HTML
* XHTML (see below)
* HTML5 (see below)

## What's "XHTML"?

Remember the "tag soup" problem described above?

One evolutionary branch of HTML emerged to tackle the tag soup / shitty-HTML problem. The gist was: "let's make HTML conform to strict XML requirements for well-formedness".

Thus was "XHTML" born.

The pure XHTML route turned out to be a mess of non-validating pages and overly-strict parsing that broke things, it wasn't supported at all in IE6 (a very popular browser), etc -- kind of impractical. It didn't really get much traction as-is.

However it did have a big impact on getting web developers to think more in terms of validation and cleaner coding, and encouraged the move toward "semantic markup" (see below).

Note: there *is* a 2011 "XHTML5" spec for "polyglot" hybrid of HTML and XML, but it's pretty irrelevant, imo.

## What's "HTML5"?

It's the 5th revision of HTML, the successor to HTML 4, which is what everyone means by default when they say just "HTML".

HTML 5 is supposed to replace HTML 4, XHTML 1, and DOM Level 2 HTML (see the list of specs below).

A brief rundown on some of the things that HTML5 adds to older HTML:

#### Note: don't freak out over the size of this list. Very few people know *all* this shit.

**Just get a feel for what's in the family of specs that people call "HTML 5".**

* New tags
	* `<section>`
	* `<nav>`
	* `<article>`
	* `<aside>`
	* ... and dozens more. Skim the [full list of tags here](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/HTML5_element_list)
* New API's
	* Media API
		* API calls around the `<video>` and `<audio>` elements
	* Text Track API
		* for subtitling and captioning
	* Drag and Drop API
	* History API
	* [User Interaction API](http://www.w3.org/TR/html5/editing.html)
		* lets you provide richtext editing to users
	* [Offline Caching API](https://developer.mozilla.org/en-US/docs/HTML/Using_the_application_cache)
	* [Web Storage](http://dev.w3.org/html5/webstorage/)
	* Canvas 2D Context
		* the `<canvas>` element for 2-D drawing
	* [Web Messaging](http://dev.w3.org/html5/postmsg/)
	* [Web Workers](http://dev.w3.org/html5/workers/)
		* threads, basically
		* by default in JavaScript there's only one thread
	* [Web Sockets](http://dev.w3.org/html5/websockets/)
	* [Server-sent Events](http://dev.w3.org/html5/eventsource/)

And so on, and so on (there are quite a few things not listed here).

Basically, when you couple all this with the huge speed gains made by modern JavaScript engines, the takeaway here is...

#### browsers have evolved into an awesome, feature-rich platform.

In fact, there are a fair number of people who think that the browser is the future of _all_ (consumer) application development. 

While that's a bit of a stretch, you can see big bets being placed in that general vicinity -- Chromebooks being an obvious example. But there's much more activity and investment going on beyond that.

Another practical takeaway is...
**Good web application developers will be in very high demand for the foreseeable future.**


## What's "semantic markup"?

Basically, the philosophy that markup should express the meaning of the content that it is wrapping, as much as possible. 

Originally people would just throw around a shit-ton of generic `<div>` tags and call it a day. However rather than just wrap everything in generic blocks, semantic markup style encourages developers to use more-specific tags to express intent more clearly.

For example, you could replace the `<div>` that used to wrap your header area with a `<header>` tag. 

This makes for cleaner-looking, more-easily-understandable and -maintainable code. Also it's more readily readable by automated screen readers, a big benefit for accessibility.


## What's the W3C?

The W3C (World Wide Web Consortium) is led by Tim Berners-Lee (the guy credited with inventing the Web) and it's a standards-making body, akin to the [IEEE](http://www.ieee.org/publications_standards/index.html), the [ISO](http://www.iso.org/iso/home.html), or the [IETF](http://www.ietf.org/).

Here are some important W3C specifications. But before you look at the list...

#### Again, don't freak out over the size of this list. Very few people have actually read through all these.

*Simply be aware that these standards exist, and are actively used (and contributed to) by the browser developers to implement the browsers we use every day.* Therefore these docs constitute good spelunking material (not to say good spanking material) for understanding some detail about how a given CSS/HTML/DOM feature is supposed to work. But it's reference material, not bathroom reading.

* HTML
	* HTML 4 
		* [HTML 4.01](http://www.w3.org/TR/REC-html40/)
	* "HTML 5"
		* Note: there is no official spec for "HTML 5" per se, just a collection of loosely-related specs that expand (greatly) on HTML 4.
		* [HTML 5](http://www.w3.org/TR/html/)
* CSS
	* [good overview](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS3) 
	* [CSS 1](http://www.w3.org/TR/REC-CSS1/)
	* [CSS 2.1](http://www.w3.org/TR/CSS2/)
	* "CSS 3"
		* Note: there is no official spec for "CSS 3" per se, just a collection of loosely-related specs that expand (greatly) on CSS 2.1
		* [CSS Transitions](http://dev.w3.org/csswg/css-transitions/)
		* [CSS Animations](http://dev.w3.org/csswg/css-animations/)
		* [CSS Transforms Level 1](http://dev.w3.org/csswg/css-transforms/)
		* [CSS Text Level 3](http://dev.w3.org/csswg/css-text/)
		* [CSS Color Module Level 3](http://dev.w3.org/csswg/css-color/)
		* [Selectors Level 3](http://dev.w3.org/csswg/selectors3/)
		* [Media Queries](http://w3c-test.org/csswg/mediaqueries3/)
		* [CSS Style Attributes](http://dev.w3.org/csswg/css-style-attr/)
		* [CSS Backgrounds and Borders Module Level 3](http://dev.w3.org/csswg/css-backgrounds/)
		* [CSS Values and Units Module Level 3](http://dev.w3.org/csswg/css-values/)
		* [CSS Flexible Box Layout Module](http://dev.w3.org/csswg/css-flexbox/)
* DOM (Document Object Model)
	* [good overview](https://developer.mozilla.org/en-US/docs/DOM_Levels) 
	* DOM Level 1 
		* [DOM Level 1 Core](http://www.w3.org/TR/REC-DOM-Level-1/level-one-core.html)
		* [DOM Level 1 HTML](http://www.w3.org/TR/REC-DOM-Level-1/level-one-html.html)
	* DOM Level 2
		* [DOM Level 2 Core](http://www.w3.org/TR/DOM-Level-2-Core/core.html)
		* [DOM Level 2 HTML](http://www.w3.org/TR/DOM-Level-2-HTML/) 
		* [DOM Level 2 Events](http://www.w3.org/TR/DOM-Level-2-Events/)
		* [DOM Level 2 Style](http://www.w3.org/TR/DOM-Level-2-Style/)
		* [DOM Level 2 Traversal and Range](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/)
	* DOM Level 3
		* [DOM Level 3 Core](http://www.w3.org/TR/DOM-Level-3-Core/core.html) (the latest DOM spec)
	* Beyond DOM "levels" (present-day spec work)
		* [DOM Living Spec](http://dom.spec.whatwg.org/)


