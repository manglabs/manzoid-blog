## Verbs and Nouns
HTTP has a handful of request methods, which you are already familiar with -- GET, POST, etc.

These can be best understood as "verbs" corresponding to the good old CRUD actions:

<table>
<tr><td>Create</td><td>POST</td></tr>
<tr><td>Read</td><td>GET</td></tr>
<tr><td>Update</td><td>PUT</td></tr>
<tr><td>Delete</td><td>DELETE</td></tr>
</table>

"Resources" are the entities that URI's point to. A specific web page, a specific image, or a specific "User" instance in a database, these are all thought of as "resources" in HTTP, and each one is supposed to have a unique URI addressing it.

You can also think of these "resources" as being "nouns".

So the idea is to have just a few verbs, but many objects (nouns) manipulated by those verbs. The verbs are defined by HTTP spec, the nouns are defined by you. So you get the best of both worlds, hopefully -- the flexibility of specifying any nouns you need to model your particular domain, coupled with a small, easily-understood set of shared actions that clients of your system can then take against those nouns. I.e., it should be easy to consume your API.

This is in direct counterpoint to RPC (Remote Procedure Call)-based API's. RPC-based API's have many verbs _and_ many nouns. That style leads to an endless proliferation of an endless variations on similar-sounding API functions, such as `fetchUserRecord()` or `getUser()` or `getUserRecord()` or `readUser()` or `loadUser()` or whatever. The resulting lack of standardization makes it harder to inter-operate.

This "few verbs, many nouns" philosophy is the basis of the popular API style known as ReST. The full discussion of ReST belongs in a post of its own, but suffice it to say that HTTP's philosophy is powerful and a non-trivial reason why the Internet has scaled to the size that it has.