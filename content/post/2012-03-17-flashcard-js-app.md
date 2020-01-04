---
author: Rob
categories:
- Projects
date: "2012-03-17T22:06:48Z"
guid: http://robwilliams.me/?p=253
id: 253
title: Flashcard JS App
url: /2012/03/flashcard-js-app/
---
A few months ago I created the .NET Flashcard application. It’s been working great for me. The only trouble is I’m not always near my computer when I have time to look through my flash cards. I wanted to be able to access the cards from anywhere – my iPhone, a public computer, etc. This screamed web-based application, and Javascript seemed like the clear choice.

I was able to port the .NET logic almost line-for-line to Javascript with minimal effort. In order to minimize the changes necessary to the code, I had to add an Array.remove() function, which is easy enough to do in JS thanks to the .prototype feature. Aside from that, the biggest amount of work was UI-related. Instead of a dialog-based design for adding and editing slides, I decided to use a form which appeared and disappeared via jQuery effects. To be iPhone useable, I also was forced to make buttons for all the actions, a departure from the keyboard-only design of the original.

The data storage in the original .NET program was JSON, so it was a breeze to integrate with that in JS. I used a jQuery [plug-in](http://code.google.com/p/jquery-json/) that allows easy de-serialization and serialization of JSON text. The only question that remained in where and how and I going to persist the JSON object that represents the deck of flash cards? Lately I’ve become obsessed with WordPress, and since I was feeling lazy I just leveraged WP to do all the heavy lifting for me. I stuck the entire JSON object into the value field of some user metadata, so the basic idea is you log in to the WP site, go to my JS application, and it loads your deck straight from the SQL database powering WordPress. I don’t have to worry about security or authentication or storage — it’s all just handled for me. The server-side work took only about 10 minutes.

The source code is available [here](http://robwilliams.me/weekly/JSMemory.zip).

To install, just place it all in a sub-directory of your favorite WordPress installation and modify the line at the bottom of index.php to be the appropriate URL for your domain and subdirectory (this setups an auto-redirect for logging in). Then access the sub-directory you uploaded to and you will be prompted to log into WordPress (if you aren’t already); log in as any WordPress user that can edit posts (feel free to change the permission requirement in ajax.php) and start using the app.

I’m not sure what the behavior is when you start with a fresh database, as I manually imported the one I was using with my old .NET application. You’ll likely get some horrible error, but the solution is to simply create a blank database in your user metadata table. The schema is as follows:  
`<br />
meta_key = memoryDecks<br />
meta_value = {"1": "name of your first deck", "2": "name of your second deck", ..., "active": "1"}<br />
meta_key = memoryDeck1<br />
meta_value = {"finishedSlides":[], "notMemorizedSlides":[], "remainingSlides":[]}<br />
...`

So, create a ‘memoryDecks’ entry with a sequential list of your decks and their names. Set “active” equal to the number of the deck you want selected by default. Then create a ‘memoryDeck#’ entry with a blank deck — with three empty arrays for each category of slides. From here, the app’s UI should work without an issue and you won’t need to revisit the database again.

**Update July 23, 2012:**

I actually use this app every day, so I’ve made a few modifications from the original to make it nicer:

  * Ability to view a tabular version of your current deck at any time (the “Show Deck” button)
  * Fixed a bug where saving would fail if the deck did not change
  * Ability to switch between multiple decks

[<img class="alignnone size-full wp-image-256" title="Screen shot 2012-03-18 at 1.07.17 AM" src="http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.17-AM.png" alt="" width="510" height="400" srcset="http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.17-AM.png 510w, http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.17-AM-300x235.png 300w" sizes="(max-width: 510px) 100vw, 510px" />](http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.17-AM.png)

[<img class="alignnone size-full wp-image-257" title="Screen shot 2012-03-18 at 1.07.37 AM" src="http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.37-AM.png" alt="" width="570" height="325" srcset="http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.37-AM.png 570w, http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.37-AM-300x171.png 300w" sizes="(max-width: 570px) 100vw, 570px" />](http://robwilliams.me/wp-content/uploads/2012/03/Screen-shot-2012-03-18-at-1.07.37-AM.png)