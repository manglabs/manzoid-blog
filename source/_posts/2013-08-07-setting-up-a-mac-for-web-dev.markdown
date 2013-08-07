---
layout: post
title: "Setting up a Mac for web dev"
date: 2013-08-07 05:21
comments: true
categories: howto productivity
---

<img src="http://i.imgur.com/sVXZv5w.jpg?2" style="float:right;margin:3px 10px">

I recently set up somebody’s brand-new Macbook Pro for Ruby-centric web development work (they are new to it, and will be working through tutorials and stuff first).
I didn’t install absolutely everything she’s going to need, by any means, but I did want to provide a starting point, to save her some time and make it easier to get to the “meat” of ramping up. I find that a fair number of people get bogged down in the tedium of just getting things set up to do anything, and they don’t get to the part where they actually do stuff.

Also I think having a bunch of stuff pre-installed gives an upfront, holistic sense of the kinds of tools that come in handy, which could also help orient a new developer.

Anyway, having done that, I thought it might be useful to post the list of what I wound up installing. (I suppose I could make live links for all of these things, but unless someone asks, I’m just going to be lazy for now and let you paste things into Google to find them. Your results will be up-to-date that way, at any rate.)

I'm interested in hearing other suggestions for what a good web dev set up might look like. Please pile on--

## Development Kits

*   XCode (needed to install ruby, also for iOS development)
*   Xcode Command Line Tools 
    *   iOS6 simulator
*   Eclipse IDE 
    *   Android dev kit

## Package Management

*   Homebrew (http://brew.sh/)
*   Macports (this was actually installed as a side effect of installing ruby via RVM, and they're not usually supposd to be installed together, but oh well)
*   RVM
*   Bundler

## Text Editors

*   Sublime Text 3 (this is currently a beta version, the stable version is v2)
*   Sublime Text enhancements 
    *   as per [Michael Hartl's list][1]
*   MacVim (a graphical vim client)
*   LibreOffice (open-source MS Office clone)

## Other programming tools

*   iTerm2 (a slick replacement for built-in Terminal app)
*   ack (a programmer-friendly replacement for generic `grep`)
*   Charles (proxy sniffer tool)

## Browsers

*   Chrome 
    *   Google URL shortener extension
*   Firefox 
    *   Firebug inspector

## Languages

*   ruby 2.0.0 (the new default version of Ruby)
*   ruby 1.9.3 (probably unnecessary but just to demonstrate RVM's ruby version management)
*   Java SE (Standard Edition) 6

## Frameworks

*   Rails 4
*   Sinatra
*   Node.js

## Database

*   MySQL
*   Sequel Pro (a nice Mac-only GUI for MySQL admin)

## Audio/Video

*   VLC (ugly but functional video player that plays many formats)
*   Audacity (nice sound editor)

## Image Editing

*   Paintbrush (MS Paint clone for Mac)
*   GIMP (GNU Image Manipulation Program -- Photoshop clone)
*   imagemagick (command line tools for image manipulation)
*   graphviz (node-graph generator, useful for generating entity-relationship diagrams etc)

## Utility Apps

*   Alfred (quick search over your local system, launches apps quickly)
*   BetterTouchTool (for window snapping)

## GNU Utilities

*   wget
*   tree
*   watch
*   fortune
*   cowsay (just for fun, try `fortune | cowsay` on the command line)

## Documentation Tools

*   Mou (for markdown documentation)
*   PlantUML for Eclipse (for generating flow diagrams)

## Miscellaneous tweaks

*   Hid "Spotlight" service (in favor of Alfred, described above).
*   Enabled "New Terminal Tab at Folder" in Finder.
*   Turned off ri and rdoc for gem installs by default.
*   Customized the command prompt.
*   Made various aliases and other `.bash_profile` tweaks.

 [1]: https://github.com/mhartl/rails_tutorial_sublime_text