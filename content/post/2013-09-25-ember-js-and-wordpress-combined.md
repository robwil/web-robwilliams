---
author: Rob
categories:
- Projects
date: "2013-09-25T20:35:52Z"
guid: http://robwilliams.me/?p=314
id: 314
title: Ember.js and WordPress Combined
url: /2013/09/ember-js-and-wordpress-combined/
---
UPDATE (11/1/2013): Time got away from me on this project. Interestingly enough, I started working on a web application project in October 2013 that heavily used AngularJS, so by necessity I was forced to learn that. I prefer Angular to Ember in a lot of ways (and it obviously helped when you’re more familiar with something), so I think I will be using that in the future. In either case, this idea of bolting a JS frontend to a WordPress blog was kind of silly and I’m not planning on going further with it.

As mentioned in the last post, I’ve been playing a lot with [via Yehuda Katz](http://www.justin.tv/confreaks/c/2246947)). “Convention over configuration” is something that I mostly love about Rails, so I figured it might be worth the relentless learning curve which EmberJS has a reputation for.

After following along with a couple screencasts, I felt naively confident in my skills and trudged deep into a weird project idea I had: create a WordPress theme which fetches the blog posts dynamically using Ember. Perhaps I got the idea because the [JSON API provided by this plugin](http://wordpress.org/plugins/json-api/).

My intention is to use this theme as the new layout for this blog, but before I do that I need to figure out a way to maintain the current URLs. I’ve written my Ember routes to keep the WordPress style permalinks which I’ve got configured on my site, but this doesn’t escape the “#” which appears in all Ember URLs. There seems to be ways around this with .htaccess, but I’m not sure how I can simultaneously keep images and other links working while rewriting all of WordPress’s URLs to include the ‘#’. I’m thinking it may be best to implement this redirection on the WP side of things, rather than meddling with .htaccess and potentially breaking many things. We’ll see what I can come up with…

Even in its working state, this EmberWP theme has almost zero features of WordPress aside from pagination and viewing posts. I still need to add the following features:

  * Category filtering
  * Date archives (more filtering)
  * Actually using Ember properly (maybe this means using Ember Data 1.0 Beta?)
  * Better handling of mobile (remove that padding!)
  * Code cleanup as I learn Ember better (since I bet I’m [doing it wrong](http://www.pandemiclabs.com/blog/wp-content/uploads/2012/04/facebook_-_doing_it_wrong.jpg "Doing it wrong"))

The current code base can be accessed on [Github](https://github.com/robwil/emberwp).