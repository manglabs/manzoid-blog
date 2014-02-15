---
layout: page
title: "Engineering questions"
comments: true
sharing: true
footer: true

---

## <a name='toc'>Table of Contents</a>

### Background knowledge
* [Data Structures and Algorithms](#cs-alg-background)
* Databases
* [Networking](#networking-background)
* Practical Math for non-scientific computing
	* logic, set theory 
	* logarithms, log-log graphs
	* calculus
	* probability, statistics (stochastic modeling), combinatorics
		* Bayes
	* linear algebra
	* game theory
	* math for dynamic programming (e.g., sudoku solver)
	* common problems (see http://community.topcoder.com/tc?module=Static&d1=tutorials&d2=geometry2)
		* line-line intersection
		* finding a circle from 3 points
		* reflection
		* rotation
		* convex hull
		* primes	
* Programming Paradigms
* Regular Expressions
* Software Design
	* What's pub/sub?
		* What's a message bus?
		* What are message queues?
	* What is ReST?
	* What's the Law of Demeter?
	* How to choose between storage options? E.g., SQL vs NoSQL, or various flavors of NoSQL.
	* various abstract software design patterns
	* various specific cases, like how Reactor is implemented in EventMachine
		* TOCHK http://stackoverflow.com/questions/9138294/what-is-the-difference-between-event-driven-model-and-reactor-pattern
* Compilers and Domain-specific Languages
* Scaling for Internet traffic
* Mobile Computing
* HCI/UX/Design -- theory & practice
* Operating systems
	* TOCHK http://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-828-operating-system-engineering-fall-2006/readings/
	* TOCHK: http://studentnet.cs.manchester.ac.uk/ugt/COMP25111/syllabus/
* Computer Hardware
	* computer architecture & organization
	* How does computer arithmetic work? [see link](http://floating-point-gui.de/)
	* Assembly 
* Java
* Python
* Ruby
* C
* Objective-C
* [JavaScript](#js-background)
* HTML
* CSS
* [Web app development](#web-apps-background)
	* How browsers work
* Engineering, product, and project management
* Data mining basics
* Business stuff

### Interview problems
*
*
*

### Programming Practice

#### Exercises
* make a slideshow widget
* "fix the bugs"...
#### Katas / Practice sites
#### Game clones
* Rock-Paper-Scissors
* Connect 4
* [Tic Tac Toe](/games/tictactoe/)
* Hangman
* Pong
* Solitaire
* Snake
* Tetris
* Asteroids
* Breakout
* [Game of Life](/games/game_of_life/)
* Maze creator
* Maze solver
* Tanks
* Pacman
* Zork
* http://inventwithpython.com/blog/2012/02/20/i-need-practice-programming-49-ideas-for-game-clones-to-code/
* http://playfic.com

####[[⬆]](#toc) <a name='cs-alg-background'>Data Structures and Algorithms</a>
* How do you perform Big-O analysis?
* [What's a linked list?](what-is-a-linked-list.html)
* What's a hashtable?
* What's a set? Disjoint-set?
* What's a tree? vs a _binary_ tree? vs a binary _search_ tree?
* What's a graph?
* What's a trie?
* What's a quadtree?
* What's a stack?
* What's an array?
* What's a vector?
* What's a queue? (circular, priority)
* What's a heap?
* Sorting (various)
* Union-find
* Shortest path
	* A*
	* Line-sweep see http://community.topcoder.com/tc?module=Static&d1=tutorials&d2=lineSweep
* What is NP-hard vs NP-complete?
* Min/max flow
* Sequence comparison
* String searching (various)
* Dijkstra’s algorithm
* Traveling salesman
* Compression (TBD various algorithms)
* N-grams http://www.sitepoint.com/natural-language-processing-ruby-n-grams



[http://www.nczonline.net/blog/tag/computer-science/](http://www.nczonline.net/blog/tag/computer-science/)
TOCHK: Sedgwick's algorithms course, part 1 and 2

####[[⬆]](#toc) <a name='ruby-background'>Ruby and Ruby Ecosystem</a>
* What is Rack?
* What is Rails?
* What is ActiveRecord?
* What is Sinatra?
* What are the main differences between Ruby 2.0 and Ruby 1.9.x and Ruby 1.8.x?
* What are the different flavors of Ruby?
* What is Ruby?
* What's cool about Ruby?
* Why do some people dislike Ruby?
* Ruby vs Python?
* Ruby vs Java?


####[[⬆]](#toc) <a name='networking-background'>Networking</a>

TOCHK: http://server.dzone.com/articles/introduction-networks
TOCHK: http://beej.us/guide/bgnet/output/html/multipage/index.html
TOCHK: http://stackoverflow.com/questions/4139379/http-keep-alive-in-the-modern-age

* What is the OSI model?
* What's a port?
* What are sockets?
* What's a packet?
* What's IP?
* How does `ifconfig` work?
* What's the IP routing table?
	* How do you use `route` command?
* What's the difference between IPv4 and IPv6? (and who cares?)
* What's MAC?
* What's DNS?
* How does `nslookup` work?
* What's ARP?
* What's DHCP?
* What's TCP?
	* Why is it usually called TCP/IP?
* What's UDP?
* What's a URI?
	* How's it different from a URL?
* [What's HTTP?](what-is-http.html)
	* comprehensive header list
		* http://code.tutsplus.com/tutorials/http-headers-for-dummies--net-8039 
	* HTTP performance optimizations
	* HTTP proxying
	* SEO and HTTP
	* References
		* https://developer.mozilla.org/en-US/docs/HTTP
		* http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177
		* http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-2--net-31155
* HTTP keep-alive vs TCP keep-alive http://stackoverflow.com/questions/2735883/relation-between-http-keep-alive-duration-and-tcp-timeout-duration
* Practical cryptography for the web
* What's Base-64 encoding?
* What's HTTPS?
* What's SSL?
* What's TLS?
* How does HTTP Authentication work?
* Overview of network authentication strategies.
* What is the same-domain policy? 
* What's CORS?
* What's CGI?
* How does email work?
	* SMTP, POP 
* What happens when you submit a form?
* How does uploading images work?
	* (or generally, big binary files)
* How does background-upload work?
* What's MIME?
* How does a router work?
	* What's the difference between a hub and a router?
* What's a gateway?
* What's a firewall?
* What's a proxy server?
* What's a debugging proxy? (Charles, Fiddler, Wireshark)
* What exactly happens when you type in "google.com" into your browser's location bar?
* How does "ping" work?
* How does "traceroute" work?
* What is SNMP?
* What are popular network monitoring tools that DevOps uses?
	* e.g., Nagios
* What's SSH?
* How does FTP work?
* SFTP? SFTP vs SCP?
* What's rsync?
* How does `netstat` work?
* How does `telnet` work?
* How would you minimally secure a newly-setup box?
* How do sessions work?
* What's the difference between throughput, concurrency, and latency?
* How does streaming audio work?
* How does streaming video work?
* How does a crawler work?
	* TOCHK: http://www.robotstxt.org/robotstxt.html
* What's apache?
* What's nginx?
* What's Phusion Passenger?
* What's SPDY?
	* see https://blogs.akamai.com/2012/10/http20-what-is-it-and-why-should-you-care.html

####[[⬆]](#toc) <a name='web-apps-background'>Web app development</a>
* What is ReST?
* How would you minimally set up a new box to run a web server?
	* http://a-developer-life.blogspot.com/2011/09/setup-rails-application-on-ubuntu.html
* What are Puppet and Chef?
* What's Capistrano?
* What's New Relic?
* _insert a shitload of browser technologies here_

####[[⬆]](#toc) <a name='js-background'>JavaScript</a>

##### Variables and type

* [What are the types in JavaScript?](what-are-types-in-javascript.html)
* [What's the difference between `==` and `===`?](what-is-double-vs-triple-equals.html)
* [What's the difference between a variable that is: null, undefined, or undeclared, and how to check for these states?](what-is-the-difference-between-null-undefined-undeclared.html)
* [What's "hoisting"?](what-is-hoisting.html)

##### Objects

* What's the difference between: `function Person(){}`, `var person = Person()`, and `var person = new Person()`?
* What's the difference between an "attribute" and a "property"?
* When is extending built-in JavaScript objects a good idea? When is it *not* a good idea?
* How do you use getters and setters in JavaScript objects?

##### Functions

* What's the "arity" of a function? Why might you care about that, practically speaking?
* What's the difference between `.call` and `.apply`?

##### Scoping

* [How does scoping work in JavaScript?](scoping-in-js.html)
* How does `this` work in JavaScript?
* What's `Function.prototype.bind`?
* What's a closure, and how/why would you use one?
* What are anonymous functions, and how/why would you use one?
* What's an IIFE (Immediately Invoked Function Expression)? see [http://benalman.com/news/2010/11/immediately-invoked-function-expression/](link)
* Why doesn't the following work as an IIFE: `function foo(){ }();`? How to fix it?

##### Inheritance

* How does inheritance work in JavaScript?
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

* What functional programming features does JavaScript offer?
	* intro: What's functional programming? Practical pros/cons?

##### Server interaction

* What's AJAX? (give some technical detail on how it works).
* [What's JSONP?](what-is-jsonp.html) (and how it's not really AJAX).
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
* If you have a Mac, how are you going to test your JavaScript on IE?

##### UI/UX

* What's JavaScript templating and why is it used?
* Describe how one of the major templating libraries works. (Mustache.js, Handlebars etc.)
* [What's the difference between responsive design and adaptive design?](what-is-diff-btw-responsive-adaptive.html)
	* How might JavaScript come into play to make such design styles work?


