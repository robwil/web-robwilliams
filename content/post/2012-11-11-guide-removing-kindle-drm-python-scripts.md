---
author: Rob
categories:
- Guides
date: "2012-11-11T18:49:04Z"
guid: http://robwilliams.me/?p=300
id: 300
title: 'Guide: Removing Kindle DRM with Python Scripts'
url: /2012/11/guide-removing-kindle-drm-python-scripts/
---
This blog is usually used to write about personal software projects that I&#8217;ve worked on, but today I take a break from that to write a quick guide about how to use some freely available Python scripts to remove Kindle DRM. I am writing this because the most popular program I have made is the [Skindle GUI](http://robwilliams.me/2010/07/skindle-gui/ "Skindle GUI"), and in providing support for many of the people who use it I have tried to stay current with the DRM removal techniques. I have always known there were a few Python scripts floating around (they&#8217;ve been around for quite a while &#8212; like since <a title="DarkReverser's Blog" href="http://darkreverser.wordpress.com/2008/02/13/new-blog/" target="_blank">2008</a>), but I never realized how high quality they were. Having tried them out earlier today, I can say they give Skindle a run for its money both in ease of use and functionality.

All of that being said, here&#8217;s the guide. I&#8217;m assuming Windows here, but the underlying scripts are cross-platform (being written in Python, after all) and the DRM Removal Tools referenced have Mac versions included.

**Setup Steps (do once)**

1) Install Python 2.7 for Windows (<a title="ActiveState Python 2.7 Windows Installer 32-bit" href="http://www.activestate.com/activepython/downloads/thank-you?dl=http://downloads.activestate.com/ActivePython/releases/2.7.2.5/ActivePython-2.7.2.5-win32-x86.msi" target="_blank">32-bit</a>, <a title="ActiveState Python 2.7 Windows Installer 64-bit" href="http://www.activestate.com/activepython/downloads/thank-you?dl=http://downloads.activestate.com/ActivePython/releases/2.7.2.5/ActivePython-2.7.2.5-win64-x64.msi" target="_blank">64-bit</a>) via ActiveState.

2) Install PyCrypto module (<a title="PyCrypto Module 32-bit on Voidspace" href="http://www.voidspace.org.uk/downloads/pycrypto26/pycrypto-2.6.win32-py2.7.exe" target="_blank">32-bit</a>, <a title="PyCrypto Module 64-bit on Voidspace" href="http://www.voidspace.org.uk/downloads/pycrypto26/pycrypto-2.6.win-amd64-py2.7.exe" target="_blank">64-bit</a>) for Python. The installer should automatically detect the Python 2.7 installed in the previous step.

3) Download and unzip the <a title="DRM Removal Tools: Direct Download Link" href="http://www.datafilehost.com/download-a9204932.html" target="_blank">DRM Removal Tools</a> (I recommend you uncheck the box for downloading using DataFileHost&#8217;s download manager; no need to install a program just to download one thing). If you are interested in reading more about DRM Removal Tools, check out the web site <a title="ApprenticeAlf: DRM Removal Tools" href="http://apprenticealf.wordpress.com/2012/09/10/drm-removal-tools-for-ebooks/" target="_blank">here</a>.

4) Browse to the &#8220;DeDRM\_Applications/Windows/DeDRM\_5.4&#8221; sub-directory of the DRM Removal Tools folder.

5) Double-click the &#8220;DeDRM\_Drop\_Target.bat&#8221; file. It will bring up a screen like the following for preferences.

The one we are concerned with is the Kindle Serial Number. Fill that in and press &#8220;Set Prefs&#8221;. Then you can close the program.

(You can get the serial number by going to Menu->Settings from your Kindle device, then going to Page 2 where it will list the serial number &#8212; starting with a &#8220;B&#8221; &#8212; under Device Information)

[<img class="alignnone size-thumbnail wp-image-304" title="screen" src="http://robwilliams.me/wp-content/uploads/2012/11/screen-150x150.jpg" alt="" width="150" height="150" />](http://robwilliams.me/wp-content/uploads/2012/11/screen.jpg)

Now you&#8217;re all set!

**Conversion Steps (do for each book)**

1) Plug your Kindle into your PC via the USB cable. It will mount as a disk, accessible from My Computer.

2) Within the &#8220;documents&#8221; subdirectory of your Kindle, find the book you want to convert. There may be many files for each book title, but the one we want is the &#8220;.azw&#8221; file.

3) Copy the AZW file to a place on your computer, for example your desktop for easy access.

4) Open up the &#8220;DeDRM\_Applications/Windows/DeDRM\_5.4&#8221; sub-directory of the DRM Removal Tools folder, so that the &#8220;DeDRM\_Drop\_Target.bat&#8221; file is visible.

5) Drag your AZW file on top of the &#8220;DeDRM\_Drop\_Target.bat&#8221; file. This will begin the conversion process. If all went well, you will now have a &#8220;.mobi&#8221; file in the same directory as the AZW file. This MOBI file will have no DRM protection.

6) You may now convert the book using <a title="Calibre" href="http://calibre-ebook.com/" target="_blank">Calibre</a>, or do whatever you want with it.