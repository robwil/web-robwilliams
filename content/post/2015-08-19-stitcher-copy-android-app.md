---
author: Rob
categories:
- Projects
date: "2015-08-19T19:16:56Z"
guid: http://robwilliams.me/?p=355
id: 355
title: 'Stitcher Copy: Android App'
url: /2015/08/stitcher-copy-android-app/
---
The purpose of this app is to use root access on Android to automate something I found myself doing quite often:

  * Extracting all the *.audio files from the <a href="https://play.google.com/store/apps/details?id=com.stitcher.app&hl=en" target="_blank">Stitcher</a> download directory (any files downloaded for &#8220;Listen Later&#8221; go there)
  * Copy to external SD card, into the &#8220;audio&#8221; subfolder, renaming into mp3.

Why did I need to do this? To play the audio files in <a href="https://play.google.com/store/apps/details?id=ak.alizandro.smartaudiobookplayer&hl=en" target="_blank">an app I enjoy more</a> than Stitcher&#8217;s playback. I did this search/copy/rename manually one too many times, so decided to automate it. The code is super hard-coded to my use case, but should be easily forkable for others&#8217; similar needs. It&#8217;s basically just a shell one-liner that finds the files and constructs a &#8220;cp&#8221; command with xargs and sed plumbing.

_Room for improvement:_ Get root permission once for the app and then reuse that for future uses (currently it asks for root access for every launch)

View the code on <a href="https://github.com/robwil/stitchercopy" target="_blank">Github here</a>