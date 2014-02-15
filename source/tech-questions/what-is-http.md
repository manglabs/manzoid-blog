---
layout: page
title: "What is HTTP?"
comments: true
sharing: true
footer: true

---

<style>
  table {
    margin-bottom: 1em;
  }
  th {
    font-size: 1.1em;
  }
  td, th {
    text-align: left;
    border:1px solid gray;
    padding:5px;
  }
  li {
    line-height: 1.5em;
  }
</style>

# Summary


* HTTP stands for "Hypertext Transfer Protocol"
* Two versions in use: 1.0 and 1.1
* Almost always transported over TCP/IP
* Communicate via request/response transactions
* Sessions can have 1 transaction, or multiple transactions
* Stateless across requests
* Plaintext format
* Controlled by a lot of HTTP headers
* Actions target specific "resources" via URI's
* Spec defines just a few "verbs" (vs users defining many "nouns")
* Content-agnostic (many types of data can be sent)
* Handles relative and absolute URI's
* Returns well-defined status codes
* Supports redirects
* Supports caching (many variations, as of HTTP/1.1)
* Supports compression (mostly of interest: gzip)
* Supports [cookies](what-is-a-cookie.html)
* Supports persistent connections (in HTTP/1.1)
* Supports "chunked transfer" for incremental page rendering (in HTTP/1.1)


# Details

Here's a quick rundown of a few key items in the above list.

I'll do separate posts to dive a bit deeper into some of the other items, such as how caching works, because otherwise this post would get ginormous.

## TCP pipes

HTTP is usually transported over TCP/IP. It doesn't *have* to be, but it's safe for your working assumption to be that it's always going over TCP/IP.

HTTP connections are really just TCP connections. TCP's job is to give you a reliable pipe to shovel bits through, where "reliable" means: 

1. the bits will arrive (or it will complain) and 
1. they will arrive in the order you sent them.

#### Port 80

The TCP default port for HTTP is `80`. (Once again, [there's a spec for that](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml?&page=2)...)

On the client side, port `80` is what your browser will try to use for its HTTP requests, if the port isn't explicitly included in the URL.

On the server side, static-content web servers like Apache will default to listening on port `80`, whereas dynamic web application servers don't want to conflict with static-content web server running on the same machine, so they use numbers like `3000` (Rails), `8000` (Django), and `8080` (Tomcat).

## Requests, and responses thereto

HTTP is a **client/server protocol**. Clients initiate requests and servers respond to those requests. Servers cannot initiate actions, they must passively wait for requests. (There are a variety of "server-push" technologies nowadays, but describing those is out of the scope of this post.)

This transaction style is generally referred to as the **request/response cycle**, or a request/response loop. This bit of terminology is relevant to note because web engineers are often trying to visualize and discuss what happens back and forth along that cycle/loop.

HTTP is **stream-based**. Therefore in HTTP you _have_ to specify either content length or, when using chunked-transfer (see `Transfer-Encoding` below), you send the final chunk with a special marker saying it's the end. Otherwise the app that you're talking to has no way of knowing when you're done!


## HTTP sessions (not those *other* sessions, hmph!)

The word "session" is kind of overloaded in web-related talk. In my experience, people use it most often to refer to the application-maintained session. E.g., the user experiences "session timeout" in your app after, say, 30 minutes of inactivity and has to log in again.

However, here we are *not* talking about that kind of session, rather we are talking about a more specific and limited "HTTP session". An HTTP session is a set of 1 or more HTTP request-response transactions. HTTP/1.0 can only handle 1 transaction per session. HTTP/1.1 supports the option of multiple transactions per session.

#### HTTP session phases

1. The browser establishes a TCP connection
2. The client sends the HTTP request over that connection and starts waiting for the response
3. The client processes the request and sends it.
4. Conclude the session (?)
	* In HTTP/1.0, the TCP connection is always now closed by the server.
	* In HTTP/1.1, persistent connections are enabled by default, so the server waits for a possible follow-on request from the client. After some pre-configured number of seconds has passed without a subsequent request, the server times out and closes the TCP connection.

#### Things to note

1. This HTTP keep-alive/persistent-connections timeout is usually set to be only a few minutes or less (and sometimes just a few seconds).
2. The timeout length is not set by a header value or really negotiated between client and server in practice (as far as I know). Rather, each server configures its own default timeout value, and all the clients of that server will have their own default timeout values. It all somehow works out in the end!
3. Again this is quite different from *application* sessions, where even sensitive apps like bank websites don't time out *that* fast.

## HTTP is (almost) stateless

A fundamental design principle of HTTP is avoiding persistent state.

Each request/response cycle is basically stateless. Nothing is remembered on the server that's specific to the previous request from that client. The main exception is the `Cookie` header, and even that was tacked onto the core protocol for the sake of pragmatism.

A stateless approach avoids messy coupling between a specific client and a specific server.  This in turn helps our systems scale because processing can be more-easily parallelized.

### Why "almost" stateless?

Because of [cookies](what-is-a-cookie.html). But if you say that out loud, everyone's going to frown and insist that well, _really_ it's a stateless protocol, so just go with the flow.

## Content-agnostic
HTTP uses MIME types to enable the transmission of a wide variety of content types. This makes it more flexible and future-proofed.

MIME stands for "Multipurpose Internet Mail Extensions", and it's the standard for identifying resources in email attachments and on the web. The idea is similar to but more reliable than using filename extensions to determine file type (".doc", ".txt" and whatnot).

Here is the official list of [supported MIME types](http://www.iana.org/assignments/media-types/media-types.xhtml).


## Easy to read and write!

HTTP is simply chunks of plain text delimited by carriage returns. That makes it super-easy to construct your own HTTP messages by hand, just to get a feel for it (or to interactively debug, etc). 

Let's fire up `telnet` and look at an example.

### GET example

	> telnet www.manzoid.com 80
	
	Trying 208.113.140.57...
	Connected to manzoid.com.
	Escape character is '^]'.

	> GET / HTTP/1.1
	> Host:www.manzoid.com
	> 

Note the extra carriage return at the end. This signals that the request header part is complete and the request body is about to start. Since we didn't supply a `Content-Length` request header, the web server knows at this point that the entire HTTP request is complete.

The server generates and sends back its response:

	HTTP/1.1 200 OK
	Date: Sat, 08 Feb 2014 20:30:39 GMT
	Server: Apache
	Last-Modified: Sun, 04 Aug 2013 03:50:21 GMT
	ETag: "1be-4e31717451940"
	Accept-Ranges: bytes
	Content-Length: 446
	Vary: Accept-Encoding
	Content-Type: text/html
	
	<html>
	<body>
	<h1>This space intentionally left blank.</h1>
	</body>
	</html>
	
Note that the response headers have a carriage return after them as well, separating them from the response body (where the HTML is).

At the end of all that, telnet reports to us:

	Connection closed by foreign host.

### POST example

Basic posting of form values to the server only requires changing the HTTP method from `GET` to `POST` and adding a couple more headers, `Content-Length` and `Content-Type`:

	> POST /user HTTP/1.1
	> Host: www.mysweetfakeapp.com
	> Content-Length: 65
	> Content-Type: application/x-www-form-urlencoded
	> 
	> name=Ned%20Jones&request=Send%20me%20one%20of%20your%20thingies.

There are of course many variations on this stuff. Don't worry about all those details yet.

For now, just feel confident that you can hit servers at will (your own or third-party), to explore how they behave. It's pretty easy!

Also note that the plaintext nature of HTTP means it's not too hard to capture requests and "replay" them, for troubleshooting or automated testing. As just one example, the ["VCR" gem](https://github.com/vcr/vcr) does that for Ruby apps.


## HTTP headers

HTTP has a bunch of headers where key metadata and configuration lives. Most of the interesting tweaks and gotchas of HTTP reside in its headers.

### Header categories

There are 5 categories of HTTP headers:

<table>
<tr><th>Category</th><th>Description</th></tr>
<tr><td>General</td><td>Generic metadata about both request messages and response messages.</td></tr>
<tr><td>Request</td><td>Metadata specific to request messages.</td></tr>
<tr><td>Response</td><td>Metadata specific to response messages.</td></tr>
<tr><td>Entity</td><td>Metadata specific to the "entity" (payload) of the message.</td></tr>
<tr><td>Extension</td><td>Non-standard headers. May be app-specific or a de facto standard.</td></tr>
</table>

### Examples of widely-used headers

Here's a sampler of a few headers for each of the above categories. This is _not_ a comprehensive list! You should be familiar with at least the headers listed here, as a starting point, and then (at least vaguely aware of) [all of them](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14), over time.

<table>
<tr><th>Category</th><th>Name</th><th>Description</th></tr>
<tr><td class="bg1">General</td><td>Connection</td><td>Set to <code>keep-alive</code> by default in 1.1. You can pass <code>close</code> to end the session immediately.</td></tr>
<tr><td>General</td><td>Date</td><td>Timestamp of message origination.</td></tr>
<tr><td>General</td><td>Transfer-Encoding</td><td>Set this to <code>chunked</code> to start receiving chunks of the response before the entire response is generated.</td></tr>

<tr><td>Request</td><td>Host</td><td>E.g., "manzoid.com". Required by HTTP/1.1. Needed for virtual hosts (multiple domains on one machine).</td></tr>
<tr><td>Request</td><td>User-Agent</td><td>Identifies this client, e.g. by browser type.</td></tr>
<tr><td>Request</td><td>Accept-Language</td><td>User-preferred language. Important for I18N (internationalization).</td></tr>
<tr><td>Request</td><td>Referer</td><td>Which URI was on before I asked for this resource? E.g., I was at foo.com and clicked a link to go to foo.com/blah, the <code>Referer</code> will say "foo.com". Used a lot by tools like Google Analytics.</td></tr>
<tr><td>Request</td><td>Cookie</td><td>How cookies are passed back to the server.</td></tr>

<tr><td>Response</td><td>Expires</td><td>When the client should consider the resource to be "stale".</td></tr>
<tr><td>Response</td><td>Last-Modified</td><td>When the server thinks the resource was last modified.</td></tr>

<tr><td>Entity</td><td>Content-Length</td><td>Size of the body in bytes.</td></tr>
<tr><td>Entity</td><td>Content-Type</td><td>MIME type of the body.</td></tr>

<tr><td>Extension</td><td style="white-space: nowrap;">X-Forwarded-For</td><td>De facto standard for identifying the original IP address of a client connecting through a proxy/load balancer.</td></tr>

</table>

## HTTP status codes (AKA response codes)

When an HTTP response arrives back at the client, it will contain a status code indicating what happened when the server tried to process the request, and sometimes what should happen next (e.g., in the case of a redirect).

### Response code categories

There are 5 categories of response status codes:

<table>
<tr><th>Range</th><th>Category</th></tr>
<tr><td>1xx</td><td>Informational Messages</td></tr>
<tr><td>2xx</td><td>Success</td></tr>
<tr><td>3xx</td><td>Redirect</td></tr>
<tr><td>4xx</td><td>Client Error</td></tr>
<tr><td>5xx</td><td>Server Error</td></tr>
</table>

### Examples of widely-used response codes

Here are some of the more important specific HTTP response codes in various categories.

<table>
<tr><th>Code</th><th>Description</th></tr>
<tr><td>200</td><td>The famous general-purpose A-OK.</td></tr>
<tr><td>201</td><td>The resource was successfully created, yay!</td></tr>
<tr><td>301</td><td>The resource has <em>permanently</em> moved to another URL.</td></tr>
<tr><td>302</td><td>The resource has <em>temporarily</em> moved to another URL.</td></tr>
<tr><td>304</td><td>The resource wasn't modified since the last time you fetched it (based on what you told me in your request), so just load your cached copy.</td></tr>
<tr><td>400</td><td>Your request is malformed.</td></tr>
<tr><td>401</td><td>Some more authorization is needed (e.g., Basic Auth)</td></tr>
<tr><td>403</td><td>Access denied!!! The creds you sent are wrong, or maybe the filesystem permissions are set up wrong on the server.</td></tr>
<tr><td>404</td><td>The famous <em>derp!</em> code.</td></tr>
<tr><td>500</td><td>Generic server badness. Better check the logs!</td></tr>
<tr><td>502</td><td>You connected to me, and I tried to proxy the request upstream to another server, but that <em>other</em> server pooped its pants.</td></tr>
<tr><td>503</td><td>Basically "closed for repair". Often crops up when the server's overloaded.</td></tr>
<tr><td>504</td><td>Just like 502, except the request to the upstream server just timed out, there was no explicit error.</td></tr>
</table>

## HTTP/1.0 vs HTTP/1.1

HTTP/1.1 added some really useful stuff to the standard:

* Persistent connections
* Lots of caching additions
* Proxy support
* Chunked transfer encoding

Nowadays pretty much every HTTP server and client is 1.1-compliant, given that 1.1 was released in 1997 (with some fixes in 1999). There are a few old Unix tools like `wget` that [still do 1.0](http://wget.addictivecode.org/FrequentlyAskedQuestions#Does_Wget_understand_HTTP.2F1.1.3F).

Despite the prevalence of 1.1 support, the distinction is still somewhat relevant, because when you're using simple scripts or using simple/older tools, you don't want to waste time being confused about unexpected behavior because you dropped yourself back to HTTP/1.0 behavior accidentally.


# Next steps

Following are some intermediate topics that deserve more attention in one or more subsequent post(s). I'll add links as I fill it in.

* [Try reading the HTTP spec!](the-http-spec.html) It's not so bad! And it's useful!
* [Cookies](what-is-a-cookie.html). Yum.
* HTTP-specific **performance optimizations** (caching, compression, persistent connections, and chunked transfer)
* HTTP **proxying**
* **ReST API's** and HTTP
	* note to self: *method types - safe/unsafe/idempotent, "representation" - MIME types and content negotiation, clean URI design and routing, proper use of response codes, verbs/nouns*
* **SEO** and HTTP
	* note to self: *URI's, referrers, redirects, single-page apps and URI fragments*





