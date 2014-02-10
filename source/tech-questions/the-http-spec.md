---
layout: page
title: "Why should I read the HTTP spec?"
comments: true
sharing: true
footer: true

---

<style>
  li {
    line-height: 1.5em;
  }
</style>

HTTP (v1.1, not 1.0) is formally defined in [RFC 2616](ftp://ftp.rfc-editor.org/in-notes/rfc2616.txt), released in 1999.

Let's crack it open for just a couple minutes, in this post. It's pretty readable.

# Why bother?

Why would you care about the details of the HTTP spec, any more than you would care about the spec used to build, say, your microwave oven?

### Improve troubleshooting

If your microwave oven breaks, you typically just throw it away or maybe get someone else to fix it. However, to extend that analogy, _you_ are the Samsung engineer in the Consumer Appliances Division who has to troubleshoot why all these microwave ovens are being reported as defective. To do that effectively, it helps to know exactly how it was supposedly built -- rather than relying on hearsay, or on busting one open with a circular saw and roaming through the guts... and/or trying to find the original designer (now long gone, of course) to badger with plaintive questions.

Googling articles and Stack Overflow answers when the shit has actually hit the fan only takes you so far. When we don't understand the context of what we're working on, we waste time diving down ratholes and on spray-and-pray experiments.

### Avoid "programming by coincidence"

Solid engineers all try to avoid "[programming by coincidence](http://pragprog.com/the-pragmatic-programmer/extracts/coincidence)", AKA "programming by accident" or "programming by permutation".

The more you know about how exactly the web was designed and therefore how it actually functions in practice, the better your chances of writing reliable code and making sensible architectural decisions.

You will be well-served by slowly and steadily building up a better mental model of how the parts of the technical ecosystem work and how they fit together.

One key way to do that is periodically diving into the source of truth behind those Googled article, and that source of truth is the published specifications. These are the very same docs that the platform, tools, and library engineers are all referring to as they write the foundations that your application is then built upon. That's good company to be in -- just like Ned.

# An excerpt: Last-Modified

Let's take a quick look at a specific example.

Let's say you need to look up how the `Last-Modified` header is supposed to work. In that case, you could refer to section [14.29 "Last-Modified"](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.29), and you would see:

	14.29 Last-Modified
	
		The Last-Modified entity-header field indicates the date and time at which the origin server believes the variant was last modified.
		
		       Last-Modified  = "Last-Modified" ":" HTTP-date

		An example of its use is
		
		       Last-Modified: Tue, 15 Nov 1994 12:45:26 GMT

		The exact meaning of this header field depends on the implementation of the origin server and the nature of the original resource. For files, it may be just the file system last-modified time. For entities with dynamically included parts, it may be the most recent of the set of last-modify times for its component parts. For database gateways, it may be the last-update time stamp of the record. For virtual objects, it may be the last time the internal state changed.
		
		An origin server MUST NOT send a Last-Modified date which is later than the server's time of message origination. In such cases, where the resource's last modification would indicate some time in the future, the server MUST replace that date with the message origination date.
		
		An origin server SHOULD obtain the Last-Modified value of the entity as close as possible to the time that it generates the Date value of its response. This allows a recipient to make an accurate assessment of the entity's modification time, especially if the entity changes near the time that the response is generated.
		
		HTTP/1.1 servers SHOULD send Last-Modified whenever feasible.
	   
# Interpreting the "Last-Modified" excerpt

The language of this entry is a bit stiff of course, but it's not too bad. Here's what this section is saying, with a bit of commentary:

* `Last-Modified` is an **"entity"** header, which means it describes some metadata about a message payload. This header is used in responses, so you could also de facto think of it as a response header, even though it's not formally classified as such.
* The server is **not required** to send it, but should attempt to send it.
* Being a server-side timestamp, the **server's clock is used**.
	* Note: This can be important for debugging client/server clock-skew bugs.
* `Last-Modified` **semantics are ambiguous** depending on the nature of the resource that's being returned.
	* Note: Kind of a non-statement, but otherwise people might assume it means something more precise and consistent than it actually is supposed to...
* Servers also time-stamp responses in the `Date` field
	* Note: if you pop over to section "14.18 Date", you'll see that `Date` is _optional_ (since some servers don't have reliable clocks, etc).
* So *if* there's a `Date` timestamp for the response, then...
	* the server **_cannot_ send a timestamp which is later than `Date`**. In that (broken edge) case, make them the same value instead.
* `Last-Modified` and `Date` should be **time-stamped almost simultaneously**.

So now, when you are looking at a server log or the browser's traffic log in the developer tools area, and you see the `Last-Modified` header listed, you will have a much clearer idea of what it does. There's [more to understand](http://www.w3.org/Protocols/rfc2616/rfc2616-sec13.html#sec13) about how this header interacts with other headers to determine the client's (or proxy's) caching strategy, but at least you now have a solid grasp of what `Last-Modified` is supposed to do, on its own.


