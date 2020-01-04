---
author: Rob
categories:
- Projects
date: "2010-01-05T11:18:58Z"
guid: http://robwilliams.me/?p=98
id: 98
title: International TV Database Website
url: /2010/01/international-tv-database-website/
---
For my Web Programming class, I worked with a partner to create a full website that allows you to keep track of the TV shows you watch. It’s main function is determining when new episodes of shows air and then providing that information to the users in a variety of ways. The basic use of the site is for someone to sign up for an account, input all the TV shows they watch, and then visit the site every few days to see which episodes they may have missed. This is mostly useful for people who don’t watch shows at their allotted times but may instead watch via the Internet or a DVR.

The website also has some advanced features, like e-mail alerts of new episodes, an RSS feed customized to only show new episodes for your shows, as well as an overview of all the episodes of your shows that are going to air in the near future.

The backend data for the site is thetvdb.com, which provides a great API to get at its data. We mirror a lot of the data onto our own databases, but do so in a manner which is optimized for DB space. That is, we store the least amount of data necessary to provide the user with the information they need. This includes pruning of the Episodes DB every night, removing episodes which every user in the system has marked as watched, as well as only storing the episodes that a user has NOT watched. The latter decision complicated a lot of the DB queries, but I think in the long run it should be worth it.

You can view the website [here](http://tv.eatyourexam.com/).

NOTE: This site is no longer live. You can still access it and use it to see how things work, but it no longer pulls updates of episode information, making it pretty much useless. The reason is because I have since started using RSS feeds from the torrent site I belong to, and it accomplishes the same goal. I figured I didn’t want to be sending a few hundred requests to thetvdb’s API every night for no reason.