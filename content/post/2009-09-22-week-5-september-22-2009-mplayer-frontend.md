---
author: Rob
categories:
- Projects
date: "2009-09-22T17:35:06Z"
guid: http://robwilliams.me/?p=77
id: 77
title: Week 5 &#8211; September 22, 2009 &#8211; Mplayer Frontend
url: /2009/09/week-5-september-22-2009-mplayer-frontend/
---
**Purpose:**

Mplayer has been my favorite video player for a long time. I found that it handles audio much better than VLC, while providing the same great support for video. I&#8217;ve never found a file that caused Mplayer to glitch. Okay, that&#8217;s actually not completely true. <a href="http://mplayerosx.sourceforge.net/" title="MplayerOSX" target="_blank">Mplayer for OSX</a> hasn&#8217;t been updated since 2005, and the old version is starting to show its age. That said, Mac users have the option of compiling Mplayer themselves from the source code, and it&#8217;s quite an <a href="http://blog.bloople.net/read/mplayer-on-snow-leopard" title="Mplayer on Snow Leopard" target="_blank">easy process</a>. I recently compiled the latest version and it easily gobbled up the file which gave the old MPlayer for OSX some trouble.

It was mostly from my laziness that this latest program has sprung up. I didn&#8217;t feel like opening the Terminal and pathing out the file to my video file every time I wanted to view a video. But I also wasn&#8217;t willing to settle for the two-month old build of [MPlayer OSX Extended](http://mplayerosx.sttz.ch/ "MPlayer OSX Extended"). It&#8217;s probably my own fault that I&#8217;m not willing to deal with a slightly outdated version of a program which works just as good as the latest thing, and so I figured it was only right that I solved the problem myself. I did just that by developing a quick frontend for Mplayer for this week&#8217;s project.

**Cocoa Results:**

I have been doing iPhone development up until now, but it is pretty much the same exact thing as Cocoa development (that is, the framework for Mac desktop applications provided by Apple). This week&#8217;s program really didn&#8217;t fit iPhone (I think there actually is an Mplayer port for iPhone, but I&#8217;m not trying to drain my battery faster than 3G data access), so I went with Cocoa instead.

I was a little rusty with Objective-C since it&#8217;s been almost three weeks since I did an iPhone App. That said, I was able to code the frontend in little time at all. It didn&#8217;t take more than 90 minutes, thanks mostly to <a href="http://www.cocoadev.com/index.pl?NSTask" title="CocoaDev: NSTask" target="_blank">the great description of NSTask at Cocoadev</a>. It&#8217;s example was pretty much exactly what I needed to launch the Mplayer process from a Cocoa application. After that, I just had to meddle a little bit with NSMutableArrays to get the few options passed to Mplayer, and it was done.

Click [here](/weekly/Week5_Cocoa_MplayerFrontend.zip) to download the source code for this week.  
Note: I would include a binary, but I feel like it&#8217;s pointless considering I am running Snow Leopard and therefore only those with 10.6 or higher would be able to use it. Also, the path to Mplayer is hard-coded into the application, so you might need to change that and recompile anyway.

**Conclusion:**

I&#8217;m not going to lie, Cocoa makes it VERY easy to make a frontend for a command-line application. I won&#8217;t hesitate to use it again for this purpose. On a completely unrelated note, it seems the rule I made for myself that I cannot ever skip a language environment more than twice in a row works well. If I had gone any longer without firing up XCode, I expect I wouldn&#8217;t be able to remember the difference between an outlet and an action. It&#8217;s really sad how poor my memory is with programming languages, but I suppose I can&#8217;t really complain. I just need to work extra hard to keep myself up to snuff.