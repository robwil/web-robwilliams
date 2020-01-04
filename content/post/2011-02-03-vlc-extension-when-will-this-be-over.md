---
author: Rob
categories:
- Projects
date: "2011-02-03T13:33:53Z"
guid: http://robwilliams.me/?p=194
id: 194
title: 'VLC Extension: When will this be over?'
url: /2011/02/vlc-extension-when-will-this-be-over/
---
I use [the cameo of Ms. Hawking on Brother Campbell’s desk](http://lostpedia.wikia.com/wiki/Catch-22#Trivia). Maybe I’m the only one with this problem, but even then I still had a problem.

So, I decided to make a VLC Extension using the Lua scripting interface. It performs the obvious task of finding the duration of the show, the currently elapsed time, in combination with the current system time, and performing some time arithmetic. All the modulo 3600 stuff (used in seconds -> H:M:S conversion) was not very interesting and I’ve done it many times before. The more interesting, or should I say more difficult (because it was anything but fun), part was figuring out how to use the Lua scripting interface. The [Lua programming language](http://www.lua.org/) itself is somewhat well documented; I say somewhat because it isn’t quite as in-depth as say the Java API, but it still has all the answers there somewhere. But the interface for making extensions in VLC is literally documented in a single README.txt file buried in the source tree. Even so, the text file does a good job of at least explaining the functions, even though it doesn’t show how to use them. But then there are plenty of example extensions which do show how to use the functions. The bigger problem is that while the README gave hints on how to find the duration of the file, it didn’t say anything about how to find the elapsed time or remaining time or anything like that. I started to think it wasn’t possible with the interface and was thinking of adding it then recompiling VLC. However, while poking around in the source code for VLC’s Lua scripting engine, I found some interesting uses of the vlc.var Lua object. It turns out that you can use this object to access even undocumented parts of the program via Lua, you just need to know what the name of it is. Luckily, once I figured this out, I was able to find what I needed by using the “grep” tool to search through the examples extensions.

To make a long story short, once I got that taken care of, coding took about 30 minutes. This just goes to show how much of a negative effect poor documentation can have on a developer’s life.

You can download the script by clicking [here](http://robwilliams.me/weekly/when_will_this_be_over.lua "When will this be over? Lua Script"). Then you need to place it in one of the following folders, depending on your platform:

<div id="_mcePaste">
  <ul>
    <li>
      C:\Program Files\VideoLAN\VLC\lua\extensions\ on Windows;
    </li>
    <li>
      VLC.app/Contents/MacOS/share/lua/extensions/ on Mac OS X;
    </li>
    <li>
      /usr/share/vlc/lua/extensions/ on Linux.
    </li>
  </ul>
</div>

Then you simply use it by going to the “View” menu and selecting “When will this be over?”

Note: This requires VLC version 1.1 or higher.

The github repository for this project is [here](https://github.com/robwil/When-will-this-be-over-).