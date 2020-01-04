---
author: Rob
categories:
- Projects
date: "2010-02-06T18:24:17Z"
guid: http://robwilliams.me/?p=107
id: 107
title: Facebook Chat Logger &#8211; Greasemonkey Script
url: /2010/02/facebook-chat-logger-greasemonkey-script/
---
I have recently started using Facebook&#8217;s built-in chat client quite a bit. I&#8217;m very impressed with the way they were able to create it, how it is completely server-oriented, allowing you to skip around their website and log off and on again without losing information. Despite having everything stored on the server, the FB developers have decided to only store the transcripts from the chats for 90 days on their servers, for privacy reasons. However, the amount of time they are actually available to you as the non-admin user of the website is more like 24 hours. In practice, I&#8217;ve found it to be rather unpredictable as to when the log of my last chat with someone vanishes, but they very rarely stick around for more than a day.

Since the chats are not saved for too long, and they certainly aren&#8217;t archived permanently, I decided I needed to find a way to accomplish that. For my AIM chats, I already use Pidgin&#8217;s built-in logging capability to log conversations, and I think having the ability to refer back to old conversations is very helpful.

When I googled to find Facebook chat loggers, I did find two solutions. The first one involved using Pidgin&#8217;s Facebook plug-in in order to access the Facebook chat right from that client, which as I mentioned above already includes a chat logger. The reason why I didn&#8217;t like this solution is because the whole purpose of the Facebook chat, in my opinion, is that it&#8217;s web based and so you can access it anywhere. I wanted a chat logger that would do the same thing, and be portable. The second solution I found did accomplish that goal. There is a Firefox add-on that logs the chats exactly how I wanted. The problem with this is it requires signing up for an account on an external website, and then that website is where all the chat logs are stored. I didn&#8217;t feel comfortable sending my private conversations to a third-party website.

Once I decided there was no existing solution, I set out to design my own. I decided to accomplish it using a Grease Monkey script. The reason I chose Grease Monkey is because it is easy to install those scripts on Firefox or Chrome (4.0+, or earlier beta versions), so it would be pretty portable, and because it would make interfacing with the FB website very easy.

The Grease Monkey script handles the client end of thing, which is what extracts the chat logs from the Facebook website as you are on it. I set it up in a way where it polls the chat window for changes every 1 minute, and it keeps a local copy of the conversations in memory so that it can know whether or not it changed in the past minute. If it does change, that is when it actually decides to record the conversation. The conversations themselves are stored using a PHP script that inserts them into a MySQL database, both of which are hosted on my personal web server (for the privacy reasons I mentioned above).

Because I am sure anyone reading this doesn&#8217;t want their conversations to be sent to my personal web server, nor do I want to offer my database space to others, I decided to release all the components necessary so that an aspiring reader could roll their own setup like I have. I have included a SQL file which will initiate the database for you, the PHP scripts necessary, and finally the Grease Monkey script that you can install into either Firefox or Chrome.

Also included is a web frontend (written in HTML, CSS, Javascript, and PHP) for the database, which allows you to view chats by person, day, most recent, or by performing a textual search of the message bodies.

Download the package [here](http://robwilliams.me/weekly/facebook_chat_logger.zip "Facebook Chat Logger").

Installation Instructions

  1. Import the fb.sql file into your MySQL database server, as a new database. This is probably done the easiest using a frontend such as PhpMyAdmin.
  2. Open the config.php file in your text editor and setup the appropriate settings at the top of the file where it says &#8220;Configure these options&#8221;.
  3. Upload the .php, .js, and .css files to your web server, preferably to its own directory for easy access (and so the index.php doesn&#8217;t overwrite anything).
  4. Install the Grease Monkey script using either the Firefox extension or Chrome 4.0 which supports it natively.
  5. Go to Facebook.com and start chatting. You can then go to your web server&#8217;s address, to the directory where you uploaded the .php files, to view the chat logs as they are saved.

If anyone has any feedback, questions, or comments, let me know.

**Update (5/16/2010):** As you all know, Facebook modifies their page quite often. They obviously don&#8217;t care whether or not their markup change affects third-party programs like the one I created. That being said, a reader informed me that this script no longer works. I have no intention of updating this to continue working every time Facebook modifies their page slightly.

Not only do I not have the time, but I have found a viable alternative to this script that works for my purposes. I previously said that I wanted a web-based solution so it would work across computers, but it turns out that with [Dropbox and Pidgin you can access your logs from any PC.](http://www.maximumpc.com/article/howtos/five_ways_use_dropbox_like_a_pro?page=0,1) This works more than well enough for me.

If you&#8217;d prefer a solution similar to my initial project, feel free to use my code as a starting point and modify it to work with the new Facebook home page markup.