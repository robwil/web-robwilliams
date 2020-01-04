---
author: Rob
categories:
- Self-Referential
date: "2010-08-22T18:19:13Z"
guid: http://robwilliams.me/?p=155
id: 155
title: IE6/7 Windows Compatibility
url: /2010/08/ie67-windows-compatibility/
---
Good news for those stuck on IE6 or 7 at work: I finally decided to make this site cross-browser compatible. Granted, what you see on IE6 or 7 is really stripped down — all of the rounded corners were hidden due to the browsers’ improper handling of PNG alphas, and the background color was forced to white in order to hide glitches caused by the fancy AJAX sliding effect. All in all, though, the site renders fine and is completely usable to those two browsers as of now. As far as I know, all other modern browsers (IE8+, Firefox 2+, Chrome, Safari, Opera, etc.) should display the full page, with rounded corner goodness, perfectly fine. I’d appreciate any feedback if anyone sees weird glitches in the layout.

To make the fixes, I just used the “easy selectors” specified on this [CSS Hacks site](http://www.webdevout.net/css-hacks#in_css-selectors) in order to target the troublesome browsers and override settings. I mostly got rid of the background images, set the background color to white, and in IE6’s case added some position:relative and display:inline to problematic divs.