---
author: Rob
categories:
- Projects
date: "2010-07-11T02:24:47Z"
guid: http://robwilliams.me/?p=126
id: 126
title: Skindle GUI
url: /2010/07/skindle-gui/
---
**Notice 11/11/2012:  **With all the hassle it takes to convert using Skindle GUI nowadays, I personally don’t even use the tool any more. I instead use the Python scripts method, described [here](/2012/11/guide-removing-kindle-drm-python-scripts/ "Guide: Removing Kindle DRM with Python Scripts"), and I recommend you check it out if you aren’t afraid to install a few extra things on your computer.

<span style="color: #008000;"><strong>Notice 8/28/2012:</strong> Received a note from “cworkman” today with a very simple fix to the Kindle 1.4.1 launch issues. Simply set your system clock to a date before July 21, 2012 (I have specifically tested June 1, 2012) and Kindle for PC 1.4.1 will launch. You can then proceed as always. The quick guide below has been updated to include this and all previous workarounds needed.</span>

<span style="color: #800000;"><strong>Notice 8/2/2012: </strong>It has come to my attention that the previous workaround (see notice from 5/6/2011) of using Kindle for PC v. 1.4.1 no longer works as of approximately July 25. The program simply no longer launches, preventing the downloading of books from that version, and therefore crippling the Skindle process. It appears all versions prior to and including 1.4.1 do not launch on any of the users systems who have e-mailed. Most likely they were crippled somehow by Amazon to prevent this very process of removing DRM. As of this moment, Skindle (nor the GUI here) seem to be usable for DRM removal. I am not aware of any alternatives to Skindle that still work (all that I’ve seen depended on the older versions of Kindle for PC which are now defunct). If you have any ideas or solutions to this problem, please e-mail me.</span>

**Notice 5/6/2011:** It has come to my attention that the latest version of Kindle for PC (1.5.0) completely breaks the functionality of skindle, the utility which this GUI uses to operate. All current DRM removal tools require the “kindle.info” file, but 1.5.0 removes this file. There is currently no way to remove DRM on books downloaded using 1.5.0. However, there is a simple workaround that involves downgrading to 1.4.1. Uninstall the new version of Kindle for PC, download the old version from [here](/upload/KindleForPC-installer-1.4.1.exe "Kindle for PC v 1.4.1 Installer - hosted here on robwilliams.me"), and install it. Note that you will probably have to manually delete and re-download all your books after downgrading, but then skindle and Skindle GUI should work well again. One annoyance is that Kindle tends to update itself automatically, so you may have to go through this uninstall-reinstall procedure every time.

I have recently been reading a lot of Kindle books, and while I love the Kindle device and it’s native iPhone app for reading on-the-go without the Kindle itself, the DRM of the Kindle books prevents some freedom that you might want. For example, if you want to read the eBooks you’ve purchased from Amazon.com on a different device like the Sony Reader or Barnes and Noble Nook, it is not possible with the DRM currently embedded in the files. It seems that the current DRM embedded in eBooks today is hurting consumers – what we have is many disjoint marketplaces for eBooks, and most are tied to a single device.

While you should realize the following information could be used to break the law, you can make your own choices. There is currently an open-source program available on the Internet called [Kindle for PC](http://www.amazon.com/gp/feature.html/ref=kcp_pc_mkt_lnd?docId=1000426311) and make sure to download whichever books you intend to free from DRM.

That being said, the linked program is command-line only. While it doesn’t bother me, some people just don’t like using the command-line. And let’s face it, Windows command line is sorely weak for those well-versed in Unix power tools. I decided to make a quick wrapper GUI around the Skindle program using C#.Net that uses the very simple-to-use System.Diagnostics.Process class to call the CLI with the appropriate arguments. It didn’t take very long to make and it makes using Skindle much easier. Enjoy.

Edit (1/28/2011): This is by far the most popular tool I posted on this site. I have received a lot of e-mails asking how to use the old program. I decided to give the program a complete overhaul, since the initial version was just a wrapper around a command-line tool and didn’t offer any additional help. This new iteration I think is much easier to use even if you have no idea about the Kindle For PC directory structure or skindle command-line parameters. I’ve also hidden away the Optional Settings in a different tab, since in the past some people were confused by their presence — you don’t have to touch that tab at all in 99% of cases.

Here is a quick guide on how to use the program:

  1. Buy the book you want from Amazon.
  2. Download and install [Kindle for PC 1.4.1](/upload/KindleForPC-installer-1.4.1.exe "Kindle For PC v. 1.4.1"). (newer versions will not work)
  3. Set your system clock to a date before July 21, 2012. I have tested June 1, 2012 with success.
  4. Launch Kindle for PC.
  5. Open the book in Kindle for PC. (in other words, make sure it is downloaded to your computer, not in “Archived Items”)
  6. Close Kindle for PC.
  7. Open SkindleGUI (download link below).
  8. The program will list all the Kindle books on your computer. The filenames don’t have anything to do with the book title, so you probably won’t know which is your book. However, when you select a book it will display its cover image, and this should help you find the book you want.
  9. Browse for output anywhere (e.g. your desktop) and give it a file name.
 10. Press Convert.
 11. You can now use the outputted file as input for [Calibre ](http://calibre-ebook.com/)or another tool to convert it to the format you want, or you can add to your Kindle via USB.

![SkindleGUI Screenshot](/images/screens/SkindleGUI.jpg) 

Download the program ZIP [here](/weekly/SkindleGUI_exec.zip "Download SkindleGUI Executables"). (You will need the .NET 3.5 Framework to run this program)

Download the source code [here](/weekly/SkindleGUI_src.zip "Download SkindleGUI Source Code"). (source code for Skindle itself can be found at the above link)

The github repository for this project is [here](https://github.com/robwil/SkindleGUI).