---
author: Rob
categories:
- Projects
date: "2015-07-05T17:42:08Z"
guid: http://robwilliams.me/?p=347
id: 347
title: 'Pack: Android Packing List App'
url: /2015/07/pack-android-packing-list-app/
---
A few years ago I made a <a href="/packing-list-web-app/" target="_blank">Packing List Web app</a>. I&#8217;ve used this app ever since on my personal travel, both domestically and internationally. The app is not rocket science or any revolutionary idea, but it works because checklists are powerful. Over time you will develop very solid packing lists that help you remember even the most obscure items. After a few trips to Asia and Africa, malaria nets are now on my list.

The issue is that when traveling, Internet access is not a guarantee. One of the key components of the app design is that it also creates a &#8220;repack&#8221; list for the way home, but that&#8217;s a lot less useful when you can&#8217;t access it due to spotty or nonexistent wifi. I also noticed a few other nitpicks, like that I would often want to pack the same things for certain kinds of trips, but there was no way to carry often my &#8220;Should Pack&#8221; selections to a new trip.

I decided to solve both of these problems by porting the app over to Android. In reality, it wasn&#8217;t really a port at all but a complete redesign. The schema changed quite a bit, and there have been a few tweaks to the UX and functionality. Again, it&#8217;s nothing special and probably can&#8217;t compete with the feature loaded packing apps already on the Play Store, but it was a fun exercise to build. At the very least, I was pleasantly surprised to see Android dropping the Eclipse IDE in favor of an IntelliJ-based IDE. The new Android Studio was a breeze to work with.

If you are interested, the full source code is available <a href="https://github.com/robwil/pack" target="_blank">on Github</a>, or you can download the built APK <a href="/upload/android-pack-app.apk" target="_blank">here</a>.

&nbsp;

<div id='gallery-2' class='gallery galleryid-347 gallery-columns-3 gallery-size-thumbnail'>
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/2015/07/pack-android-packing-list-app/device-2015-07-05-203654-2/'><img width="84" height="150" src="http://robwilliams.me/wp-content/uploads/2015/07/device-2015-07-05-2036541.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-2-350" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-2-350'>
      Editing a List
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/2015/07/pack-android-packing-list-app/device-2015-07-05-203734/'><img width="84" height="150" src="http://robwilliams.me/wp-content/uploads/2015/07/device-2015-07-05-203734.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-2-351" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-2-351'>
      Editing a List Set
    </dd>
  </dl>
  
  <dl class='gallery-item'>
    <dt class='gallery-icon portrait'>
      <a href='http://robwilliams.me/2015/07/pack-android-packing-list-app/device-2015-07-05-203818/'><img width="84" height="150" src="http://robwilliams.me/wp-content/uploads/2015/07/device-2015-07-05-203818.png" class="attachment-thumbnail size-thumbnail" alt="" aria-describedby="gallery-2-349" /></a>
    </dt>
    
    <dd class='wp-caption-text gallery-caption' id='gallery-2-349'>
      Managing a Trip
    </dd>
  </dl>
  
  <br style="clear: both" />
</div>