---
layout: page
title: "What is a cookie?"
comments: true
sharing: true
footer: true
---

## Overview

Cookies were invented by Netscape to work around the stateless nature of HTTP.

Cookies are...

* data in key-value format (`foo=bar`)
* stored on the "user-agent" (aka the "client") (aka the browser).
* up to 4 kilobytes in size.
* scoped by the website's (sub-) domain.
* usually "set" by the server (but stored on the client).
* often used to create user "sessions" -- to track interactions with a specific client across requests.

## Cookies are part of the HTTP specification

Namely, [RFC 6265](http://tools.ietf.org/html/rfc2965) and [RFC 6265](http://tools.ietf.org/html/rfc6265). Do not be afraid of these formal-looking documents! They are quite readable, if you dive in, albeit dry. And they are the definitive source for how things work, since these are docs used by the core teams building browser like Chrome and servers like Apache.

But for now, the ultra-short-story: 

#### Setting the cookie

Cookies are usually set by the server, via the HTTP response.

Here's a real response example, right after logging into [LinkedIn](http://www.linkedin.com):

    Set-Cookie:lidc="b=LB40:g=51:u=5:i=1390971520:t=1391057920:s=1456935344"; Expires=Thu, 30 Jan 2014 04:58:40 GMT; domain=.linkedin.com; Path=/

#### Sending the cookie

Once a cookie is set, the client then sends that cookie along in any future HTTP requests to that same (sub-) domain. Every HTTP request has a `Cookie` request header. 

Here's a real request example, for [nasdaq.com](http://nasdaq.com):

    Cookie:c_enabled$=true; clientPrefs=||||black; s_pers=%20s_nr%3D1390973618041-New%7C1398749618041%3B%20bc%3D1%7C1391060018044%3B; s_sess=%20s_cc%3Dtrue%3B%20s_sq%3D%3B; NSC_W.TJUFEFGFOEFS.OBTEBR.80=ffffffffc3a08e3445525d5f4f58455e445a4a423660


## Sidebar: HTTP micro-refresher

Note: remember that HTTP requests themselves are pretty simple. Just a bunch of lines of plaintext. Try piping this simple HTTP string into the awesome `netcat` tool on the command line:

    printf 'GET / HTTP/1.1\r\nHost: www.manzoid.com\r\nConnection: close\r\n\r\n' | nc www.manzoid.com 80

And you will get back some plaintext that represents the HTTP response. If your user-agent were a browser instead of a command-line tool, then that browser would start parsing and rendering that response. 

So to add the Cookie header you would just do:

    printf 'GET / HTTP/1.1\r\nHost: www.manzoid.com\r\nCookie: foo=bar\r\nConnection: close\r\n\r\n' | nc www.manzoid.com 80


## Structure of a cookie

Mandatory part:

    {key}={value}

Optional parts:

    ;path=path (e.g., '/', '/foo')
    
    ;domain=domain (e.g., 'foo.com' If not specified, defaults to current host)
    
    ;max-age={#-of-seconds}
    
    ;expires={date-in-GMT-format}
    
    ;secure (only transmit this cookie via https)

Here's a real example of a cookie I have set in my Chrome browser now, for an [Amazon Web Services site](http://forums.aws.amazon.com):

    Name:   seance
    Content:  %7B%22accountId%22%3A%22457366798257%22%2C%22iam%22%3Afalse%2C%22services%22%3A%5B%5D%2C%22status%22%3A%22ACTIVE%22%2C%22exp%22%3A1389939482000%7D
    Domain:   forums.aws.amazon.com
    Path:   /
    Send for: Secure connections only
    Accessible to script: Yes
    Created:  Thursday, January 16, 2014 10:41:47 AM
    Expires:  Wednesday, January 11, 2034 10:41:45 AM


## How do you perform CRUD on cookies?

#### 1) Cookies are usually set by the server, via the HTTP response:

The creation step via `Set-Cookie` response header is described above.

To update a cookie, the server would simply send back `Set-Cookie` for that cookie name and domain/path scope, but with different data.

To "delete" a cookie, the server would update a given cookie with an expiry in the past, or immediately expiring. Usually servers don't do this, instead when the cookie has naturally expired, this triggers some action on the server's part as of the next request, such as rendering a session-expired view or regenerating some resource.

#### 2) Cookies can also be manipulated on the client, using JavaScript:

    document.cookie = "someCookieName=true; expires=Fri, 31 Dec 2014 23:59:59 GMT; path=/";

Note that this `document.cookie` object that is exposed by the browser's `Document` object is pretty weird. If you read it, you get all the cookies:

    var allCookies = document.cookie;
    console.log(allCookies); // you will see 5 cookies in this example...

    > "fff=run; PHPSESSID=22420d53b5922e8f699e8c5fdc4d63a5; __utma=134076296.930312141.1390682149.1390682149.1390729463.2; __utmc=134076296; __utmz=134076296.1390682149.1.1.utmcsr=(direct)|utmccn=(direct)|utmcmd=(none)"
  
But you can only *write* one cookie at a time. Unlike how most JavaScript properties work, setting `document.cookie` does *not* overwrite the whole thing, it only sets _one_ cookie.

To further confuse things, there's no interface exposed for deleting a cookie outright. Instead you have to re-write it (update it) with an expiration date in the past. This may seem a bit bloody-minded, but I believe it's meant to mirror how servers manage a client's cookies.

For these reasons, as well as the fact that splitting apart and joining raw cookie strings is messy and error-prone, most people write or use a JavaScript library for manipulating cookies. E.g., [this jQuery plugin](https://github.com/carhartl/jquery-cookie).


## How does cookie security work?

Cookies are partitioned primarily by domain. In other words, if the current page is served by `foo.com`, then it cannot see cookies that were set by `bar.com`.

Similarly cookies specifically set for `www.foo.com` will not be visible on `forum.foo.com`. Only if the domain was set to the more-relaxed `foo.com` will that cookie be visible across sub-domains.

Secondarily, some sites will use the path value for finer-grained scoping. E.g., if you added `Path=/accounts;`, then that cookie would only be sent from and visible on pages under the /accounts sub-tree.

Besides scoping, another security measure is that servers can specify that their cookies are `HttpOnly`, which means that those cookies are not visible or settable via JavaScript on the client. This helps prevent malicious tampering.

Servers can specify the `secure` attribute, which means that that cookie will only be sent via HTTPS, thus preventing "man in the middle" attacks.

## How does cookie expiration work?

Cookie expiration is specified via `max-age` in seconds and/or an explicit `Expires` value in GMT format.

It's generally pretty important to expire your cookies after a judicious period of time. Too short and your users will be logged out frequently, which is annoying. Too long, and sessions will never expire, which could well be a security risk, e.g., on shared computers. Perhaps even more importantly, without timely session expiration, your site usage metrics will not be as accurate (because you won't know as well if people are really there or not).

A cookie that has no expiration set will expire when the browser window closes. This is known as a **session cookie**, as opposed to the more-common **persistent cookie**.

## What are the performance implications of using cookies?

One thing that you have to keep in mind is that **every** HTTP request to a given (sub-)domain will send your `Cookie` request header along. Since there can be multiple HTTP requests to assemble a single page, this can add up to non-trivial wasted bandwidth and wasted processing time, thus slower page loads. A big cookie sent a couple dozen times can add tens of kilobytes to a page load.

This suggests two straightforward performance improvements:

#### Don't store large data in cookies.

Instead store client-side "pointers" into a server-side cookie store.

#### Don't use the same domain or sub-domain to serve static assets.

Move those off to a CDN (content distribution network) such as Akamai or one of its many competitors. Static assets are the reason there are so many extra requests to assemble a page. If images, scripts, stylesheets, and whatnot are served from a different location, then the cookies of the main, dynamic site will not need to be sent to them and the request/response cycles will be more lightweight. 


# Extra info

Feel free to skip the rest of this for now. But just FYI...

## What are some alternatives to cookies?

**tl;dr there aren't great alternatives to cookies right now, afaik.**

But, just so you're aware of the options:

* IP address
  * obviously not reliable, but it's another data point that's used for the tracking-type of cookie
* The query string
  * If cookies aren't enabled, you can store session identifiers in the URL itself. This is messy though for obvious reasons (the URL's aren't clean and it exposes your sausage-making to even casual users, in the browser location bar).
* Flash
* eTag/If-None-Match HTTP headers
  * `eTag` is a caching mechanism meant for tagging specific resources to be cached vs regenerated, but because it involves a unique identifier, it can be "abused" for tracking as well. My impression is that this is frowned upon, because unlike cookies, there's no user opt-out mechanism for eTags.
* HTML5 (now W3C) Web Storage API
  * Lets you store a lot more data (5MB rather than 4KB, iirc)
  * But it's client-specific, so if you switch browsers or machines, whatever was stored there is lost...
  * [link to spec](http://dev.w3.org/html5/webstorage/)
  

## What other interesting/useful things are there to know about cookies?

Beyond this bare-bones outline, there are interesting follow-on topics like:

* Everyone knows that cookies have something to do with tracking. But how specifically does that work? How exactly do advertisers track you with cookies? (Who are "you" to an ad company?) What data do they collect and how do they use it?
    * Similarly, what do retailers do? (There can be some differences, at least in emphasis, because retailers own their own sites, so it's not a 3rd-party situation, and are concerned with brand building and overt customer relationships)
* What's the new policy on cookies in the EU? 
* What are the best practices around cookie expiration?
    * e.g., see [this link](http://www.truste.com/blog/2011/12/02/best-practices-for-using-cookies/)
* How are cookies involved in common security attacks?
* How can those attacks be mitigated?
    * examples: encrypting cookie contents, building in entropy, frequently regenerating session ID's, using `secure` and `HttpOnly`, proper domain scoping, obfuscating cookie names to obscure their purpose, etc

For now just be aware of these follow-on questions and you can fill in your knowledge as time goes on.
