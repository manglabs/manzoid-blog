---
layout: post
title: "What we can learn from FizzBuzz"
date: 2013-08-15 23:15
comments: true
categories: programming learning interviews entry-level

---

In programming circles, the "FizzBuzz" interview problem has become a famous chestnut, a "[can you believe how many people can't get this](http://www.codinghorror.com/blog/2007/02/why-cant-programmers-program.html)??!" kind of thing.

Here's the FizzBuzz problem statement:

	# Write a program that prints the numbers from 1 to 100. 
	
	# But for multiples of 3, print "Fizz" instead of the number.
	# For the multiples of 5, print "Buzz". 
	# For numbers which are multiples of both 3 and 5, print "FizzBuzz".


Despite being a bit of an in-joke, this question does actually get used. I was interviewing a guy recently who said that when he was interviewing at his previous job, FizzBuzz was the only question that he had been asked (!)

Like [many](http://www.codeproject.com/Articles/27742/How-To-Reverse-a-Linked-List-3-Different-Ways) [other](http://www.geeksforgeeks.org/reverse-a-string-using-recursion/) [standard](http://www.ardendertat.com/2011/10/10/programming-interview-questions-7-binary-search-tree-check/) [questions](http://en.wikipedia.org/wiki/Subset_sum_problem) that you should become familiar with if you are prepping for a programming interview, it's not a bad idea to run through it just to make sure you can do it in your sleep, on the off chance that it comes up. It's such a simple-looking problem, but it actually does trip up a lot of people, even [professional programmers with CS degrees](http://www.skorks.com/2010/04/a-fizzbuzz-faux-pas/).

Beyond that, though, it can also be useful to study FizzBuzz as a barebones driver for exploring "Interviewer Psychology 101".  What are interviewers looking for?

Let's look at what even a trivial "weeder" problem like FizzBuzz can reveal about that. 

<!-- more -->

### Can you follow instructions?

The instructions for FizzBuzz say explicitly that you are to output `Fizz` instead of the number. However, some people will "spruce up" the output so it doesn't just output `Fizz`.

To take a recent example, I saw someone choose to output the number as a prefix, e.g., `33: Fizz` That's not the end of the world or anything, but it is a bit of a yellow flag. The instructions are to output exactly the string `Fizz`, no more, no less.

Similarly, some people will **only** output `Fizz`, `Buzz`, or `FizzBuzz`, and nothing else. But the instructions clearly state that you are to print the numbers **as well**.

Misunderstanding or ignoring such simple instructions won't speak well of your ability to correctly interpret the approximately one-gajillion-times-more-complex scenarios that professional programming will present you.

I'm not saying you can't be creative or stamp your work with personal [flair](http://upload.wikimedia.org/wikipedia/commons/thumb/5/50/Ric_Flair_-_Wooooo.jpg/220px-Ric_Flair_-_Wooooo.jpg), far from it, but when doing so in an interview context you need to first demonstrate that you grokked the instructions to begin with, before you begin riffing.

Even great, iconoclastic artists like Picasso first demonstrated that they could do regular stuff first before going hog-wild:

<div style="text-align:center;font-size:0.8em">
  <img src="http://i.imgur.com/xgrNZHd.jpg?1" />
  <img src="http://i.imgur.com/mLLYQXC.jpg?1" />
  <div><em>(some straight-laced stuff Picasso did when he was a teenager)</em></div>
</div>
<br/><!-- Note: these came from: http://www.nga.gov/images/noncol/torsofs.htm -->

### Do you probe the spec for assumptions?

Miscommunication is the forlorn heart of the human condition. Overcoming the solitude of our own skulls is our lifelong lot.

Which is to say, pretty much every feature specification you'll ever get in your professional development career will have some ambiguity in it. Ambiguity is built into human languages, into our inevitable laziness about fully explaining things, into the fuzzy and intuitive way humans tend to think. Ambiguity is built into how we're not all that good at thinking things through fully, not thinking ahead to all the edge cases, not predicting how other people will receive what we're trying to say, and so on.

That given, blithely charging ahead with code without pausing to check for assumptions is a bad sign.  Take a moment to reflect on where the spec might be "soft".

Even in the case of tiny little FizzBuzz, we can concoct a few such assumption-probes. 

For example:

* Is the interviewer expecting this to be implemented in a function or is just a floating chunk of code OK?
* Is the interviewer looking for a TDD (test-driven development) approach, or just to pump out something that works? If it's a TDD approach, then more parameterization will need to take place to be able to test it.
* Assuming it's a function, shall we parameterize it to handle any range of (positive) numbers, with 1 to 100 being just the base example?
* Does each item go on its own line, or is the output separated by some delimiter, such as a space, on a single line?


<div style="text-align:center;font-size:0.8em">
  <img src="http://i.imgur.com/eXalWKu.gif" />
  <div><em>probing</em></div>
</div>
<br />

Asking stuff like this might seem overly nitpicky and bloody-minded, but it speaks to the kind of programmer who is used to flushing out and validating hidden assumptions. Those hidden assumptions can often bite you in the butt down the line, leading to wasted work and unhappy users, clients, or selves.

You don't want to spend a ton of time on this stage and be seen as a procrastinating hair-splitter, but asking at least one or two assumption-probing questions is suggestive of a thoughtful approach to coding in general. 

### Do you think things through or just dive in and make a mess?

![image](http://stream1.gifsoup.com/view1/1163476/forklift-accident-o.gif)

The following points will flesh out what "thinking things through" means in practice...


### Can you translate the problem into standard programming concepts?

Naturally, you should know about basic programming concepts like:

* variables
* booleans
* conditionals
* loops
* functions
* arrays
* hashes
* etc, etc

With the caveat that by "know about them", we mean: do you really understand how and why they're used in various situations, i.e., how to use each of them as a **building block**? 

Programming is mostly about breaking down larger real-world problems into successively smaller and smaller problems until eventually we get to tiny challenges that even a dumb, rigid computer can wrap its binary brain around. Which means, in effect, that we need to be able to express the solutions to those tiny challenges in terms of the standard handful of "primitives" and core facilities that our chosen programming language affords us.

Note that this point is *somewhat* independent of whether you can remember the exact syntax for expressing those things in the given programming language. If you are conceptually solid on the building blocks for your solution, and on the way you intend for them to snap together, then a certain (small) amount of fuzziness on the syntactical implementation details is generally accepted and waved off as "you could Google that easily".

You can't rely *too* much on this however. It's not a Get Out of Jail Free card for just plain not knowing how to do stuff, which leads to the question…

### Can you actually cut code?

Are you actually familiar with your chosen programming language? You will often be given your choice of several languages in which to do the implementation, [within reason](http://www.muppetlabs.com/~breadbox/bf/). If you don't seem to have a ready command of basic syntax in the language that you yourself chose, that doesn't augur well for your abilities. 

<div style="float:right;padding-bottom:5px;margin-left:10px">
  <img src="http://i.imgur.com/2ffwgLd.jpg"/>
  <div style="text-align:center;font-size:0.8em"><em>i'm as smart as a sponge!</em></div>
</div>

Now, most of us will readily claim that: "*I may not know this thingymabob right here, but I pick up things **fast**! I'm a sponge for learnings!*" 

The interviewer will typically think, in response: "*If that's true, shouldn't you already have picked up how to write simple things in your preferred language?*" It behooves you to know all the basic aspects of your language stone-cold and blindfolded (look again at the list of building blocks in the previous section).

Beyond language syntax, the upshot is -- can you write something that actually works? People often tie themselves into knots trying to write actual working code to do the smallest thing -- even smaller than FizzBuzz -- such as:

	# Print out the numbers 1 through 10. 

I saw someone struggle somewhat with this tiny sub-problem in an interview just yesterday (as part of a slightly larger problem), taking two minutes to assemble one line of awkward, incorrect code. This person, an entry-level hopeful, had spent something like 100 hours preparing to take their first coding interview, learning a wide variety of fairly hard things. But he didn't polish/practice/review enough to be able to cleanly execute something trivial.

<div style="float:left;margin-right: 10px;font-size:0.8em">
<img src="http://i.imgur.com/Z8tBaXy.jpg?1" />
<div style="text-align:center"><em>flop sweat</em></div>
</div>

Part of that can be attributed to interview pressure, of course. I'm keenly aware of the IQ-suppressing effects of interview pressure, and try to accommodate for that when I conduct interviews.

Interviewing is a species of public speaking, after all, which we all know is one of the scariest things for most people. And programming interviews are especially nitpicky, therefore extra-prone to make us look and feel dumb. 

But, to be fair, it's not *just* that. At least for some candidates, sub-par execution of routine tasks can also be attributed to a **copy/paste/pray** approach to slapping code together. The excellent book "[The Pragmatic Programmer](http://pragprog.com/the-pragmatic-programmer)" highlights a phenomenon which the authors call "[programming by coincidence](http://pragprog.com/the-pragmatic-programmer/extracts/coincidence)".

As you can imagine, that style of "programming by coincidence" is not something an interviewer wants to smell **any** whiff of, hence… 

<div style="clear:both"></div>

### Can you think on your feet, without running your program every 15 seconds?

You may wonder why interviewers don't simply let you crank away on an actual computer and pound out code the way you normally do, with normal tools.

Instead, in a typical interview scenario, you are generally forced to deal with the awkwardness of scrawling out code on a whiteboard, without the affordances of a modern text editor or an interactive shell to try things out in. More recently, there are ways of writing your interview code in a shared online document. But those feel clunky too.

One ostensible reason for this unnatural setting is interviewers are primarily interested in the quality of your thinking, and strictly speaking, you shouldn't need to type in order to demonstrate that. It's an artificial environment, to be sure, but they want to see if you can handle *not* being able to just jam out bits of code and run them "to see what happens", over and over and over. People who have to program that way tend to just try whatever until they get something that sort-of-works, and then commit that to the code repository. This is very related to the unappealing "programming by coincidence" style mentioned above.

A secondary reason is that it is simply more cognitively taxing to multi-task by thinking out loud while you are writing. Even though you would be kind of weird if you were to constantly mumble while you were working at a real task next to your teammates, it's not just encouraged but usually mandatory to do so when you are in an interview setting. It takes mental horsepower to do it, and if you can perform well nonetheless, it speaks well to your ability to produce even better under normal conditions. 

Whether you agree that these reasons hold any water or not, nonetheless working on a whiteboard or the online equivalent is the current coin of the realm, so you'll have to deal with it.

Even with humble little FizzBuzz, you can demonstrate being able to think through the problem and execute a relatively clean solution on the (virtual) whiteboard, talking about your approach while you do so.


### Can you spot problems?

People usually create little bugs or omissions in their first pass at a given problem.

To catch these, can you step through your code line-by-line and “be the compiler/interpreter” (so to speak) understanding what the state of the program is, as of each step?

In so doing, do you demonstrate that you're familiar with common classes of error, such as off-by-one or not handling null?

In the FizzBuzz case, the main gotcha is going to be whether you really handle all 4 cases properly. It's surprisingly easy to screw up the case where you're supposed to output `FizzBuzz`.

Blithely announcing, "welp, that's that!" when there are clearly some problems with the code remaining, or basic cases that haven't been checked… that's not confidence-inducing.

<div style="text-align:center;font-size:0.8em">
  <img src="http://i.imgur.com/PHEsTXd.jpg?1" />
  <div><em>Yep -- I'm all clean!</em></div>
</div>
<br/>


### Do you think about tests?

Most modern software development includes a lot of unit testing. Not necessarily "test-driven development", where you write the tests before you write the code, but any seasoned developer should probably be writing a bunch of tests at some point in the development cycle, or something's awry.


<div style="float:right;padding:5px;padding-left:10px;border:1px solid gray;margin-left:10px;text-align:center">
  <img src="http://i.imgur.com/WxWWSZr.gif?1"/>
  <div style="text-align:center;font-size:0.8em"><em>helpful coding assistant</em></div>
</div>

So even in doing the rinky-dink FizzBuzz problem, if you don't instinctively announce an intention and strategy for testing your code, then that's a bit of a yellow flag.

Worse, if even after being prompted to do so, you can't demonstrate that you are able to set up sensible tests, then that's even more of a yellow flag.

You should be able to show that you can sensibly bucket the large total space of possible tests into a manageably small number of distinct, meaningful categories. Trying to test every possible outcome is not as bad as testing nothing, but it's pretty bad too, as it wastes a lot of time and effort for no additional benefit.

In the trivial case of FizzBuzz, there are naturally at least 4 test categories you should concoct:

* the 3 case
* the 5 case
* the 3 and 5 case
* the number-only case (none of the above)

And if you wanted to be extra-thorough, you could also check whether it can handle invalid input (in the case where you created a parameterized function for flexible ranges).
<br style="clear:both"/>

### The upshot

Ok, let's face it, no one really cares that much about FizzBuzz per se.  

But.…if we can extract this many "*what will the interviewer be thinking?*" kind of observations just from a teensy problem like FizzBuzz, you can imagine how rich the pickings are on a juicier, typical interview problem.

And everything we've examined here (and much more) is still relevant on those larger problems.

On that scary-fun note, I'll sign off for now, but meanwhile good luck on those interviews, and feel free to drop a note if you'd like further tips on interview preparation. Happy to help if I can.

<br/>

<div style="text-align:center">
  <img src="http://i.imgur.com/dBG4G7m.jpg"/>
</div>



