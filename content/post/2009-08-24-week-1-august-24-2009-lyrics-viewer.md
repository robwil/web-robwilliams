---
author: Rob
categories:
- Projects
date: "2009-08-24T15:05:22Z"
guid: http://robwilliams.me/?p=41
id: 41
title: Week 1 – August 24, 2009 – Lyrics Viewer
url: /2009/08/week-1-august-24-2009-lyrics-viewer/
---
**Purpose:**

I love listening to music, and nothing enhances the experience like being able to read along with the lyrics. There are many websites out there which host lyrics, but sometimes internet access isn’t always available, or maybe it’s too much of a hassle to open a browser when you’re doing other things. Either way, it would be nice to have a program that automatically fetches the lyrics from the internet and displays them.

Such programs already exist, but at least the ones on Mac keep getting shut down by the authorities. [GimmeSomeTune](http://www.eternalstorms.at/gimmesometune/) is the one I use now, which is awesome because it automatically copies the found lyrics into iTunes which in turn wind up on iPod or iPhone.

Even though GimmeSomeTune is perfect for my needs, I decided that I would make my own Lyrics Viewer that would automatically fetch data from the Internet. It would provide a nice exercise in HTML parsing (or at least learning the plugins that provide such functionality, as the case may be). I have always been intrigued by these utilities, so now I would have the opportunity to figure out how they really work. The overall idea is just to output the lyrics to the screen in some way, shape or form. I decided not to include automatic iTunes populating for now, but it’s certainly an option for the future.

P.S. I’m pretty sure no one reads this blog, but even so I am half-expecting “them” to find me and force me to remove the program. We’ll see what happens.

**Ruby Results:**

I made the Ruby application first. I decided to make it into a Rails app. It probably defeats the purpose of saving you from going to the Internet for lyrics, but considering I don’t know how to use any of the Ruby graphical toolkits yet, I doubt a command line Ruby app would be useful to too many people. Plus, doing in in Rails let me get some AJAX practice (okay — granted, Rails did all the hard work for me). The total development time was 1:19:29. I wasted most of the time looking up how to make an AJAX form in Rails. The actual lyrics extraction and parsing was super easy thanks to the [Hpricot](http://github.com/defunkt/hpricot/tree/master) gem for Ruby, as well as the built-in regular expression support which made constructing the proper URLs very easy.

You can view/use the Rails app here.  
The source code can be downloaded [here](/weekly/Week1_Ruby_LyricsViewer.tgz "Ruby Source Code for Week 1").

**iPhone Results:**

The next one I tackled was iPhone. I had been doing Flex training all week, so I knew that would be a breeze. I wanted to get the hard one out of the way, and I haven’t touched iPhone development in a solid month or more. It turned out my suspicions were correct. Developing the iPhone app took a really long time (4:35:23 to be exact). I ran into a lot of trouble across the board — my Objective-C knowledge is next to nothing, I had to hit up the Apple reference guide like I was mentally challenged, and the plug-ins I tried to use for HTML parsing didn’t work.

I tried to use the [Hpple](http://github.com/topfunky/hpple/tree/master) plug-in because it was supposed to work like Hpricot which made everything so easy in Ruby. The first hurdle was that it used XPath search paths only, not CSS selectors. It wasn’t a huge deal, except when I didn’t get the right results I went on a wild goose chase for XPath examples because I thought that was the problem. But I eventually realized that Hpple just didn’t work on getting the inner_html of a div containing anything large (for example, it could easily extract the content of an h4 tag containing a few words).

I spent a good amount of time searching around for an alternative, thinking about using NSXMLParser which seemed too complex, and eventually settling on [HyperParser](http://www.dimzzy.com/index.php?page=hyper-parser), another Cocoa/iPhone plug-in for HTML parsing. The only problem with HyperParser is that, after wasting about a half hour, I realized it didn’t provide any easy way to get the contents of an HTML element — it only gave the attributes. Maybe I was just missing something, but it seemed like it didn’t pass the right information to the didEndElement delegate method to allow extracting the inner_html, and there wasn’t any other clear way to get it. The only example for its use provided on the home page dealt with extracting attributes only. However, all was not lost. HyperParser came with a nice class called HyperInput which is essentially a wrapper around NSString which lets you do some nice searching. It made it easy enough for me to implement my own parsing that simply searched for the string “<div class=’lyricbox'” and returned everything from that point until the ending tag for the div. This let me accomplish what I wanted.

The last hour or so was spent dabbling with UIWebView to get any formatting working, as well as correcting a bunch of crashes I was having because I was overzealous with my “release” calls. What this made me realize is that I have no clue how Objective-C memory management works. It’s not as simple as C free()’s. And the fact that I just called free() simple should give an idea of how crazy Obj-C’s model is. It could be that I’m just missing something altogether, but suffice it to say that it makes me miss Java’s garbage collector A LOT. One final comment I would like to make is that I officially hate NSStrings. It treats you like an idiot, and simply removing the first character from the string is a big ordeal. From now on I think I am going to convert NSStrings to C-style strings for easier parsing, then convert them back when I need to display them.

You can download the source code for the app [here](/weekly/Week1_iPhone_LyricsViewer.zip "iPhone Source Code for Week 1").

**Flex Results:**

The Flex program was more painful than I imagined, but only because I have absolutely no clue what I’m doing with E4X (ECMAScript 4 XML, the XML/XHTML parser used by ActionScript 3). The total development time was 2:09:38, and literally 80% of that was me messing around with the XHTML parsing. The main issue is that the way E4X lets you test the contents of an attribute requires the use of an at-sign followed by the attribute name — except the attribute I was testing for was “class”, which is a reserved word for AS3. There were a bunch of workarounds posted online, using the .attributes() method or using syntax like @[“class”], but nothing worked. I went crazy trying to isolate the problem, and I never could figure out why it worked for other people and not me. Either way, I finally figured out all I needed to do was test [“@class”]. Annoying issues, though not quite as bad as my iPhone experience.

Once that was taken care of, the rest was simple. Flex makes it very easy to make a quick GUI and handle all the applicable events. I am in love with the MXML code that Flex uses to define its graphical interface. I would venture to say that it’s the best GUI framework ever made. It just makes so much sense to define GUI components using an XML structure.

You can download the source code for the app [here](/weekly/Week1_Flex_LyricsViewer.zip "Flex Source Code for Week 1").

**Conclusion:**

Making a Lyrics Viewer is fairly easy once you take care of the HTML parsing. I think if I did this in a language like C++ with great string handling, I could have manually did the HTML parsing way faster than it took me to dabble with the various plug-ins. In the case of Ruby’s Hpricot, the plug-in was amazing, but in the other two cases not so much. The clear winner here was Ruby.