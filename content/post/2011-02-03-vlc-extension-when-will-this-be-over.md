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
I use <a title="VideoLAN - VLC Media Player" href="http://www.videolan.org/" target="_blank">VLC Media Player</a> as my primary program for watching digital media. I don&#8217;t know about you, but sometimes I have places to be and I&#8217;m not sure if my show will be done in time. Of course, you can do some time arithmetic and add 42:38 to the current time, but then what if you have to pause it a bunch of times to answer text messages? Or what if you need to make lunch in between and pause for a few minutes. The point is, constantly recalculating when the show will be finished isn&#8217;t difficult, but it&#8217;s just tedious to have to constantly do. Plus, juggling all those numbers means you&#8217;ll probably miss <a title="Lostpedia: Catch-22 Episode Trivia" href="http://lostpedia.wikia.com/wiki/Catch-22#Trivia" target="_blank">the cameo of Ms. Hawking on Brother Campbell&#8217;s desk</a>. Maybe I&#8217;m the only one with this problem, but even then I still had a problem.

So, I decided to make a VLC Extension using the Lua scripting interface. It performs the obvious task of finding the duration of the show, the currently elapsed time, in combination with the current system time, and performing some time arithmetic. All the modulo 3600 stuff (used in seconds -> H:M:S conversion) was not very interesting and I&#8217;ve done it many times before. The more interesting, or should I say more difficult (because it was anything but fun), part was figuring out how to use the Lua scripting interface. The <a title="Lua" href="http://www.lua.org/" target="_blank">Lua programming language</a> itself is somewhat well documented; I say somewhat because it isn&#8217;t quite as in-depth as say the Java API, but it still has all the answers there somewhere. But the interface for making extensions in VLC is literally documented in a single README.txt file buried in the source tree. Even so, the text file does a good job of at least explaining the functions, even though it doesn&#8217;t show how to use them. But then there are plenty of example extensions which do show how to use the functions. The bigger problem is that while the README gave hints on how to find the duration of the file, it didn&#8217;t say anything about how to find the elapsed time or remaining time or anything like that. I started to think it wasn&#8217;t possible with the interface and was thinking of adding it then recompiling VLC. However, while poking around in the source code for VLC&#8217;s Lua scripting engine, I found some interesting uses of the vlc.var Lua object. It turns out that you can use this object to access even undocumented parts of the program via Lua, you just need to know what the name of it is. Luckily, once I figured this out, I was able to find what I needed by using the &#8220;grep&#8221; tool to search through the examples extensions.

To make a long story short, once I got that taken care of, coding took about 30 minutes. This just goes to show how much of a negative effect poor documentation can have on a developer&#8217;s life.

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

Then you simply use it by going to the &#8220;View&#8221; menu and selecting &#8220;When will this be over?&#8221;

Note: This requires VLC version 1.1 or higher.

The github repository for this project is <a title="Github - robwil - When will this be over?" href="https://github.com/robwil/When-will-this-be-over-" target="_blank">here</a>.