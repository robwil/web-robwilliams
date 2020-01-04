---
author: Rob
categories:
- Projects
date: "2009-09-14T14:46:54Z"
guid: http://robwilliams.me/?p=66
id: 66
title: Week 4 &#8211; September 14, 2009 &#8211; Episode Renamer
url: /2009/09/week-4-september-14-2009-episode-renamer/
---
**Purpose:**

I have a lot of TV episodes that are ripped from DVD or Blu-ray, and often the filenames of such files are in strange formats that lack any useful information. The one piece of information that I really love to have is the episode name because some shows have really clever names (for example, Gossip Girl uses a variation on a movie name for every episode; The L Word episode names all start with L; Lost episode names are related to what happens in the episode), and most of the time this info isn&#8217;t included. About the only thing that is always there is the season number and episode number.

Like the Lyrics Viewer, there are a bunch of programs that probably do it way better than the program I&#8217;m going to build. However, many of the ones that I found on the first page of Google results were Windows-only. The one I&#8217;ve been using forever on Mac is command-line only and dependent on a plugin which makes it difficult to use and install for those not well-versed in command line techniques. Plus, I really felt like learning how it all works.

It turns out that extracting the season number and episode number is relatively easy with Regular Expressions (conveniently supported in both Flex and Ruby natively). Then to get the actual episode information, I am using the excellent community-driven database at <a title="TheTVDB.com" href="http://thetvdb.com" target="_blank">TheTVDB.com</a>. Not only is the information updated often, but they have a well-documented API which makes getting info pretty easy. With those two things taken care of, I figured it wouldn&#8217;t be too hard.

This week, I decided to implement the solution in both Ruby and Flex. I decided to omit iPhone for the obvious reason that there aren&#8217;t many people storing seasons of TV shows on their iPhone &#8212; and even if they were, those files wouldn&#8217;t be accessible because the iPhone API only lets you access files created in your own little sandbox.

**Flex Results:**

I decided to write the program first in Flex because I had in my mind the whole plan of how it was going to work, and I expected it to be easy. I was right about that. It took 2:02 total development time. There were some early issues with getting the API figured out, but once those were ironed out the development went smoothly. That is, until I ran into a rather complex problem.

You see, Flex dispatches URL requests asynchronously, meaning that after you send a request the program keeps going. I first implemented the program by looping through all the episodes that needed to be renamed and sending a request each time &#8212; the problem is that the loop executed in a split-second while the actual request didn&#8217;t finish until a second or so later. Once the request sent out its &#8220;Complete&#8221; event and the event handler kicked in, the only one that actually got processed was the very last request, since all the rest were lost in the mix. I had to rethink how I was doing it, and solved it by getting rid of the loop altogether and doing a implicitly recursive solution. It will probably make more sense if you check out the source code.

<img class="alignnone" title="Episode Renamer Screen" src="/images/screens/EpisodeRenamer.jpg" alt="" width="500" height="328" /> 

You can download the source code/executable [here](/weekly/Week4_Flex_EpisodeRenamer.zip "Week 4 Flex Application").

The github repository for the Flex source code is <a title="Github - robwil - Episode Renamer" href="https://github.com/robwil/Episode-Renamer" target="_blank">here</a>.

**Ruby Results:**

Porting it to Ruby should have taken ten minutes. It wound up taking 43 minutes because of HPricot issues. The first issue was because I recently upgraded my computer to Snow Leopeard, which killed my gems install that was built against the Leopard libraries. I had to do a quick &#8220;gem update&#8221; followed by &#8220;gem uninstall hpricot&#8221;, &#8220;gem install hpricot&#8221; to fix that. Not a huge deal but I wasted 5 minutes with that. Then, after hpricot was working, I failed to understand how to properly use the plugin. The home page with the documentation I used last time is now down (which is why I&#8217;m not linking to it in this post), so I had very little to go on. Thank God for the blogosphere, though, because after some googling I found some <a title="Ruby Bikini - How to Process XML in Ruby" href="http://tips.webdesign10.com/rubybikini" target="_blank">nice</a> <a title="How to Parse Twitter XML in Ruby on Rails" href="http://ruby.about.com/od/networking/qt/twitterparse.htm" target="_blank">tutorials</a> about XML parsing with hpricot.

You can download the source code [here](/weekly/Week4_Ruby_EpisodeRenamer.zip "Week 4 Ruby Source Code").

**Conclusion:**

This is probably the coolest thing I have made during these weekly exercises. I expect to use it a lot (I already have tested it on my TV collection). The only problem I see with the programs right now is that <del datetime="2009-09-16T02:34:25+00:00">they don&#8217;t allow the user to confirm the series was found appropriately</del> (updated 9/16: I added this functionality to the Flex app. updated 9/21: Fixed a bug with the regex.). They also don&#8217;t handle filenames that give the season and episode name together with no &#8220;e&#8221; or &#8220;x&#8221; delimiter (e.h. Lost.316.avi). That&#8217;s just a simple Regexp fix, while the former problem would require additional UI elements.

**Update 1/22/2011:**

Since I recently taught myself how to do some programming in Scala (more on that in this <a title="What I've Been Up To" href="http://robwilliams.me/2011/01/what-ive-been-up-to/" target="_blank">post</a>), I decided to rewrite this program using Scala. The biggest reason I chose this program in particular is because the way I implemented this program both in Flex and in Ruby was very serial in nature. That is, there was no concurrency. Flex, in fact, can&#8217;t do multi-threading at all. Ruby can, but at the time I had no clue even what multi-threaded programming was, so I left it alone. Now, coming fresh off a semester filled with concurrent programming, and learning Scala which is designed to tackle such issues, I wanted to port this program over and see if the performance would increase.

Here&#8217;s a spoiler: it didn&#8217;t. At least not in any noticeable way. I implemented the Scala code such that a separate actor (and therefore a separate thread) would both perform the regular expression parsing of the old filename and query the episode name from TheTVDB.com&#8217;s database. I figured that I should at least be able to get a large speedup since the slowest part of the operation &#8212; the Internet-based lookup &#8212; would be amortized. However, this didn&#8217;t seem to be the case. I was running this on a quad-core machine, so I know for a fact that more than one actor was running at a time. The speedup, however, just wasn&#8217;t there. My connection here at school isn&#8217;t amazing, so perhaps the network latency or theTVDB&#8217;s own servers were the reason why concurrent requests didn&#8217;t go faster than serial requests. I&#8217;m guessing that on a faster network connection, the speedup may be more noticeable.

Another unfortunate thing is that the program is much buggier than its Flex counterpart. When querying for the series ID, the program looks up all shows that match the users input. (e.g. &#8220;Lost&#8221; matches over 100 shows) If any of that resulting XML document contains non-ASCII characters, the Scala implementation blows up. The XML loader is not well-documented enough for me to get to the bottom of this problem. Googling didn&#8217;t help either.

Another issue is that the development time was so much longer on Scala. To be fair, however, this is the first program I&#8217;ve written inside of Scala on my own, so a lot of the slowness was due to me being unfamiliar with the language. I did run into some serious problems with the GUI development, though. Scala uses Java classes for many things, including GUI &#8212; I used Swing in particular. Scala has a wrapper around Swing which makes development of GUIs much easier, at the cost of flexibility. I had to produce some quite ugly pure-Java code to do some of the things I wanted, specifically with the JTable, the prominent component on my UI for this program.

All of that being said, I do think Scala is a language I will use in the future. I&#8217;m not sure it is a good pairing for GUI programs, but I do see the power in its concurrency model. I didn&#8217;t have to worry about contention or locking or anything like that, due to the immutable philosophy surrounding the language. One thing I believe Scala would be great for is any type of text parsing, due to the impressive amount of built-in pattern matching in the language.