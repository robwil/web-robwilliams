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
This blog is usually used to write about personal software projects that I’ve worked on, but today I take a break from that to write a quick guide about how to use some freely available Python scripts to remove Kindle DRM. I am writing this because the most popular program I have made is the [Skindle GUI](http://robwilliams.me/2010/07/skindle-gui/ "Skindle GUI"), and in providing support for many of the people who use it I have tried to stay current with the DRM removal techniques. I have always known there were a few Python scripts floating around (they’ve been around for quite a while — like since [2008](http://darkreverser.wordpress.com/2008/02/13/new-blog/)), but I never realized how high quality they were. Having tried them out earlier today, I can say they give Skindle a run for its money both in ease of use and functionality.

All of that being said, here’s the guide. I’m assuming Windows here, but the underlying scripts are cross-platform (being written in Python, after all) and the DRM Removal Tools referenced have Mac versions included.

**Setup Steps (do once)**

1) Install Python 2.7 for Windows ([64-bit](http://www.activestate.com/activepython/downloads/thank-you?dl=http://downloads.activestate.com/ActivePython/releases/2.7.2.5/ActivePython-2.7.2.5-win64-x64.msi)) via ActiveState.

2) Install PyCrypto module ([64-bit](http://www.voidspace.org.uk/downloads/pycrypto26/pycrypto-2.6.win-amd64-py2.7.exe)) for Python. The installer should automatically detect the Python 2.7 installed in the previous step.

3) Download and unzip the [here](http://apprenticealf.wordpress.com/2012/09/10/drm-removal-tools-for-ebooks/).

4) Browse to the “DeDRM\_Applications/Windows/DeDRM\_5.4” sub-directory of the DRM Removal Tools folder.

5) Double-click the “DeDRM\_Drop\_Target.bat” file. It will bring up a screen like the following for preferences.

The one we are concerned with is the Kindle Serial Number. Fill that in and press “Set Prefs”. Then you can close the program.

(You can get the serial number by going to Menu->Settings from your Kindle device, then going to Page 2 where it will list the serial number — starting with a “B” — under Device Information)

[<img class="alignnone size-thumbnail wp-image-304" title="screen" src="http://robwilliams.me/wp-content/uploads/2012/11/screen-150x150.jpg" alt="" width="150" height="150" />](http://robwilliams.me/wp-content/uploads/2012/11/screen.jpg)

Now you’re all set!

**Conversion Steps (do for each book)**

1) Plug your Kindle into your PC via the USB cable. It will mount as a disk, accessible from My Computer.

2) Within the “documents” subdirectory of your Kindle, find the book you want to convert. There may be many files for each book title, but the one we want is the “.azw” file.

3) Copy the AZW file to a place on your computer, for example your desktop for easy access.

4) Open up the “DeDRM\_Applications/Windows/DeDRM\_5.4” sub-directory of the DRM Removal Tools folder, so that the “DeDRM\_Drop\_Target.bat” file is visible.

5) Drag your AZW file on top of the “DeDRM\_Drop\_Target.bat” file. This will begin the conversion process. If all went well, you will now have a “.mobi” file in the same directory as the AZW file. This MOBI file will have no DRM protection.

6) You may now convert the book using [Calibre](http://calibre-ebook.com/), or do whatever you want with it.