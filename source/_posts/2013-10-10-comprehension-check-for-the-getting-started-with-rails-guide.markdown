---
layout: post
title: 'An idea for checking your understanding of tutorial content'
date: 2013-10-10 16:08
comments: true
categories: rails, learning

---

There are a lot of coding tutorials online -- it's very cool and inspiring, how much great material is available!

But -- I have the impression that a lot of that material doesn't really "stick", for beginners.

So here's one small idea: What if you converted the content of a given tutorial into a simple recipe format, and went through that recipe, trying to recall *how* they did each thing in the tutorial? Seems like it would be a quick and reliable comprehension and retention check. (Of course you could refer back to the tutorial as often as necessary.)

For example, check out the "[Getting Started](http://manzoid.com/static/rails_guide/getting_started.html)" Rails Guide. If you were to just read through that Rails Guide, or even actively read through while following along with each step on your own computer, you will definitely learn stuff. But I suspect you'd still have the nagging worry that you really hadn't quite learned it to the point of being able to do it yourself.

However, by going through a recipe-ized version of that Rails Guide, you could check your understanding and reassure yourself that, yes, you DID really get it, well enough to very likely be able to apply it elsewhere.

Below I have cobbled together a working example of what I mean. Pretty much everything there is solveable just using that tutorial (otherwise I'll provide an external link to the answer).

Give it a try, and let me know what you think. Naturally, this isn't a very "deep" insight or technique, but it might be helpful...

<br>

---

### Exercise: Let's make yet-another-Rails-powered-mini-blog!

#### Basic Setup

* Create a new Rails project called "blog"
	* (using Rails 4, *not* 3.2)
* Start up the web server and visit the home page.
* Stop the web server.
* Add a page at [localhost:3000/welcome](localhost:3000/welcome) that says "Hello Rails!"
	* *Question: When generating a controller, and supplying actions on that controller, are the action names upper-case or lower-case?*
* Make this new page the root page for the site.
	* I.e., so you can see it from [localhost:3000](localhost:3000)
	
#### Creating Posts ("C" in CRUD)

* Configure the ReSTful routes for a new "post" resource.
* On the command-line, check that the various routes got added.
* Make a controller to handle post-related actions.
	* *Question: When generating a controller, is the controller name plural or singular?
* Enable a view for creating a new post.
* Add a create action for the new-post form. 
	* For now, the action should just dump the HTTP request parameters that are sent to it.
* Create a Post model, with a `title` and `text`.
	* *Question: When generating a model, is the name singular or plural?*
	* *Question: When generating a model, does the field name come before the type, or vice-versa?*
	* *Question: What exactly are "models"? [answer: see the short section "Understanding The Model-View-Controller Pattern"](http://betterexplained.com/articles/starting-ruby-on-rails-what-i-wish-i-knew/)*
* Run the corresponding db migration.
	* *Question: are model names singular or plural? How about the name of the underlying database table for each model, singular or plural?*
* Make the create action actually save to the DB.
	* Use "strong parameters".
		* *Question: What is the problem that "strong parameters" are meant to solve? [answer](http://railscasts.com/episodes/26-hackers-love-mass-assignment?view=asciicast)* 
* After saving, redirect to a show action, which displays the newly-created post.


#### Reading Posts ("R" in CRUD)

* Add a view for the show action.
* Add an index action to list all posts.
* Add a link to the blog index page, from the home page.
* Add a "New Post" link on the blog index page.
* Add a "Back" link (back to the blog index page) on the blog-create page.

#### Validation

* Add validation to Posts, such that:
	* the title cannot empty (although the body can)
	* the length must be at least 5 characters
* If validation fails,
	* show the form again.	
	* show an error message
	
#### Updating Posts ("U" in CRUD)

* Add an edit action for Posts.
	* Make sure to use the HTTP `PATCH` method.
	* *Question: What is an HTTP method anyway? How do they compare to Ruby class methods?*
	* *Question: Why use the PATCH method in this case?*
* Add an Edit link to the posts shown on the index page.

#### Refactoring

* Remove duplication (aka "DRY up" the views) by factoring out a partial for the form.
	* *Question: Why can the same `form_for` declaration render both of the original forms, even though one was for creating Posts and the other was for updating Posts?*
	* *Question: What does "DRY" refer to? ([answer](http://programmer.97things.oreilly.com/wiki/index.php/Don't_Repeat_Yourself))*

#### Deleting Posts ("D" in CRUD)

* Add a destroy action for Posts.
* Add a "Destroy" link to the posts listed on the index page.
	* When clicking the link, it should pop up a confirmation dialog.
	
#### Adding comments

* Add a 2nd model, for comments, with a `commenter` and a `body`, associated with posts.
	* *Question: Which model is decorated in order to express "Each comment is associated with one post": the Comment model or the Post model?*
	* *Question: Which model is decorated in order to express "One post can be associated with many comments": the Comment model or the Post model?*
	* *Question: Which table has a foreign key column, `comments` or `posts`?*
* Update the db to reflect this new model.
* Add both associations connecting Comment and Post, if you haven't done so already.
* Create routes for the Comment resource, nesting it within the Post resource.
* Add UI for users to make a new comment when viewing a Post.
* Edit the Comments controller to handle posting a comment.
	* Leverage the model associations you set up.
		* *Question: What does #build do, here? ([answer](http://stackoverflow.com/questions/783584/ruby-on-rails-how-do-i-use-the-active-record-build-method-in-a-belongs-to-rel))*
	* Don't forget the "strong parameters".
	* Redirect the user back to the original post.
* Adjust the UI so the newly-posted comment renders under the original post.

#### Refactoring

* Factor out a comment partial to show all the comments for the post.
	* *Question: Why don't you need to explicitly iterate over the comments like this: `@post.comments.each`…?*
	* *Question: Is the auto-generated `comment` instance variable's name based on the model class name or on the partial-template file name?*
* Factor out the new-comment section to its own partial.

#### Deleting comments

* Add a delete link under each comment.
	* *Question: What does `method` mean, here?*
* Handle the destroy action for comments.
* Extend the destroy action for posts to delete associated comments too.

#### (Crude) authentication

* Apply basic auth to all Post-related actions except listing all posts or displaying an individual post.
	* *Question: What is "basic auth"? What's "basic" about it?*
* Apply basic auth to comment deletion.


<br>

---
<small>*Small note (mostly to self): The above exercise/recipe keys off of [my own copy of the "Getting Started" Rails Guide](http://manzoid.com/static/rails_guide/getting_started.html).  The official version that was up on the actual Rails site at the time of this writing had a bug in it, so I re-generated the docs from source (where the bug was fixed). That also keeps the doc version stable so this "recipe" doesn't go out of date too quickly.*</small>

	
