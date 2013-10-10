---
layout: post
title: "SICP Exercise 1.10"
date: 2013-10-09 23:36
comments: true
categories: SICP

---

I recently began reading through the classic MIT textbook [Structure and Interpretation of Computer Programs](http://mitpress.mit.edu/sicp/full-text/book/book.html). It's pretty cool so far, although I'm come across [serious criticisms of its pedagogy](http://www.ccs.neu.edu/racket/pubs/jfp2004-fffk.pdf), and MIT itself abandoned Scheme/SICP in its CS program a few years ago.

Anyway, while I'm at it, I'll post anything (mildly) interesting or potentially instructive about the material.

On that note, here's…

<!-- more -->

### Exercise 1.10


*The following procedure computes a mathematical function called Ackermann's function...*

	(define (A x y)
	  (cond ((= y 0) 0)
	        ((= x 0) (* 2 y))
	        ((= y 1) 2)
	        (else (A (- x 1)
	                 (A x (- y 1))))))
	                 
…Or at least, it supposedly does. Oddly, when I looked up the [Ackermann function](http://en.wikipedia.org/wiki/Ackermann_function) on Wikipedia, the definition is substantially different:

![image](http://i.imgur.com/1Qqdoax.png)

It seems to me that the implementation of that would look like the following instead:

	(define (A m n)
	  (cond ((= 0 m) (+ 1 n))
	        ((= 0 n) (A (- 1 m) 1))
	        (else (A (- 1 m) (A m (- 1 n))))))

I don't get why there are multiple versions of this mathematical function floating around. It is a little weird, especially because they lead to quite different values.

Annnnyway… so "Ackermann's function" is a classic recursion example in CS. It's interesting because its computed result , as well as the size of its call tree, grow stupendously fast. The values grow far, far faster than normal exponential stuff.

Look at how absurdly huge this gets:

![image](http://i.imgur.com/39QZ9Xp.png)

In fact, everything gets so huge, so fast, that conventional mathematical notation can't even keep with it. This caused Lord-of-CS Donald Knuth to concoct some new "[up-arrow](http://en.wikipedia.org/wiki/Knuth's_up-arrow_notation)" notation to try to represent these mega-numbers.

Back to the exercise, though. The question asks us, basically, to do some experimentation in the interpreter to try to infer the behavior of this function, translated to regular math expressions.

For the function as formulated in the textbook, that looks like this:

	(define (f n) (A 0 n))
#### => 2n

	(define (g n) (A 1 n))
#### => 2^n

	(define (h n) (A 2 n))

#### => 2^(2^(2^(2^(2)))), expanded out `n` times**

I also stepped through these bad boys in Dr. Racket, which was tedious but instructive, as usual. I don't care *too* much about this particular mathematical function per se, but the "instructive" part was that any practice in trying to "be the computer" and keep track of a recursive call stack in your head feels valuable. Kind of like practicing your scales as a musician.

One weird side note on this stuff -- running `(A 4 1)` on the other implementation (i.e., the one based on the Wikipedia version of the function) took almost 5 minutes to compute (!) It did get the correct answer eventually, though.

	cpu time: 289487 real time: 292816 gc time: 21119
	65533
	