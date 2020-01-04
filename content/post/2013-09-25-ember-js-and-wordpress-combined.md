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
UPDATE (11/1/2013): Time got away from me on this project. Interestingly enough, I started working on a web application project in October 2013 that heavily used AngularJS, so by necessity I was forced to learn that. I prefer Angular to Ember in a lot of ways (and it obviously helped when you&#8217;re more familiar with something), so I think I will be using that in the future. In either case, this idea of bolting a JS frontend to a WordPress blog was kind of silly and I&#8217;m not planning on going further with it.

As mentioned in the last post, I&#8217;ve been playing a lot with <a title="TodoMVC" href="http://todomvc.com/" target="_blank">Javascript MVC frameworks</a> lately. <a title="EmberJS" href="http://emberjs.com/" target="_blank">EmberJS</a> caught my attention because of its close-knit connections with the Rails community (<a title="Embrace Javascript Talk at RailsConf" href="http://www.justin.tv/confreaks/c/2246947" target="_blank">via Yehuda Katz</a>). &#8220;Convention over configuration&#8221; is something that I mostly love about Rails, so I figured it might be worth the relentless learning curve which EmberJS has a reputation for.

After following along with a couple screencasts, I felt naively confident in my skills and trudged deep into a weird project idea I had: create a WordPress theme which fetches the blog posts dynamically using Ember. Perhaps I got the idea because the <a title="Getting Started with Ember" href="https://www.youtube.com/watch?v=1QHrlFlaXdI" target="_blank">Getting Started screencast</a> is creating a blogging platform, or maybe it&#8217;s because I am obsessed with WordPress theme development. The problem is that it turns out I had no idea what I was doing. I hit about fifteen and a half brick walls, but after much hacking together of code I managed to get something sort of working. My initial version used a really hacky &#8220;bridge&#8221; layer between the Ember app and <a title="Wordpress XML-RPC API" href="http://codex.wordpress.org/XML-RPC_WordPress_API" target="_blank">WordPress&#8217;s XML-RPC API</a>, but I quickly ditched it for the <a title="Wordpress Plugin: JSON API" href="http://wordpress.org/plugins/json-api/" target="_blank">JSON API provided by this plugin</a>.

My intention is to use this theme as the new layout for this blog, but before I do that I need to figure out a way to maintain the current URLs. I&#8217;ve written my Ember routes to keep the WordPress style permalinks which I&#8217;ve got configured on my site, but this doesn&#8217;t escape the &#8220;#&#8221; which appears in all Ember URLs. There seems to be ways around this with .htaccess, but I&#8217;m not sure how I can simultaneously keep images and other links working while rewriting all of WordPress&#8217;s URLs to include the &#8216;#&#8217;. I&#8217;m thinking it may be best to implement this redirection on the WP side of things, rather than meddling with .htaccess and potentially breaking many things. We&#8217;ll see what I can come up with&#8230;

Even in its working state, this EmberWP theme has almost zero features of WordPress aside from pagination and viewing posts. I still need to add the following features:

  * Category filtering
  * Date archives (more filtering)
  * Actually using Ember properly (maybe this means using Ember Data 1.0 Beta?)
  * Better handling of mobile (remove that padding!)
  * Code cleanup as I learn Ember better (since I bet I&#8217;m [doing it wrong](http://www.pandemiclabs.com/blog/wp-content/uploads/2012/04/facebook_-_doing_it_wrong.jpg "Doing it wrong"))

The current code base can be accessed on <a title="Github: EmberWP" href="https://github.com/robwil/emberwp" target="_blank">Github</a>.