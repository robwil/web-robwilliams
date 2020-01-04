---
author: Rob
categories:
- Projects
date: "2009-12-02T00:09:02Z"
guid: http://robwilliams.me/?p=94
id: 94
title: MS Word 2007/8 – Word Frequency
url: /2009/12/ms-word-2008-word-frequency/
---
Just a quick note beforehand: I am done with labeling each project I do as Week N, since I have failed miserably at doing one per week. I suppose I underestimated the distractions and my inability to come up with useful projects. Either way, I am still going to make every effort to continue with these side projects; I just don’t want to pretend that I am going to be able to do them once a week, since that is clearly not the case.

**Purpose:**  
As you may or may not know, my biggest hobby is creative writing. In addition to fiction, I also write extensively about my life experiences. I prefer to call it an autobiography, but it could just as easily be called a memoir or a diary. Either way, I am inside Microsoft Word quite a bit, and my platform of choice is Mac OSX, so the specific version I use is Microsoft Word 2008 for Mac. The other day I got the idea of wanting to see which words (specifically which names) showed up the most in my autobiography. However, Word doesn’t have a built-in mechanism for showing a full distribution of word frequencies. You can get the frequency of a single word using the “Find and Highlight” feature or a trick where you do a Replace All and replace a word with itself. But there is no built-in way to show every word and its frequency.

I did some Googling, and I found out that there is indeed a way. Over at Allen Wyatt’s Word Tips website, they [dropped support for VBA](http://www.microsoft.com/mac/developers/default.mspx?CTT=PageView&clr=99-21-0&target=7b1c718f-a611-4612-b3cf-f96d4babbf261033&srcid=ad148fd4-1f3c-4690-8198-9a137b91f09d1033&ep=7). What was I supposed to do? Enter AppleScript and Perl.

UPDATE 01/10/10: I have started using Windows extensively now that I have a new gaming desktop, and so I’m in Word 2007 for Windows a lot. Rather than use the above VBA script for Word frequency, I figured I could port my Perl/Applescript solution to Windows. I made a Word add-in using Visual Studio 2008. I think it came out pretty well, aside from some strange UI choices. See below for the results, as always.

**Perl/AppleScript Results:**  
What Microsoft suggests developers do in order to cope with the lack of VBA in Word 2008 is convert their VBA scripts into AppleScript. I decided this was a valid idea, and I downloaded the multi-hundred-page documentation for Office’s AppleScript tie-ins. I trudged through endless code examples and messed around until I finally figured out it was feasible. I made a quick Applescript to print out each word, and it seemed like this would be easy. But then I realized AppleScript was missing a key data structure, one which is invaluable for word frequency calculations: a hash table. Simply put, without a hash table, keeping track of all these words is too much, especially for the very (very) slow AppleScript. It looked like I was out of luck.

That is, until I stumbled upon [Mac::AppleScript::Glue](http://search.cpan.org/~jlabovitz/Mac-AppleScript-Glue-0.03/Glue.pm). This Perl module is just what it sounds like — a way to “glue” together Perl and AppleScript. What it essentially lets you do is, from within a Perl script, send commands to AppleScript. You can then get the results as standard Perl scalars, and then use Perl’s much faster functions for post-processing. I also knew that Perl’s built-in hash type would do the trick nicely.

The documentation for the Mac::AppleScript::Glue module is quite lacking. They give an example which simply sends commands to an AppleScript-enabled program but doesn’t receive any response. I had to do a bunch of trial and error with the API to figure out exactly how to get an actual string that would represent a word from the Word document. After about two hours of frustrating tinkering, I finally figured it out. The problem is that it was ridiculously slow. It took about a half-second to get a word, and given that my documents can be as large as 100,000 words, I wasn’t about to deal with that. It was with this that I gave up altogether.

But then a day later, I got an idea that should have occurred to me a lot earlier. Why was I trying to get each word from Word, when I could just get the entire contents of the document? I figured that transferring the whole document once would maybe take a half-second, but then Perl would easily break it up into words using its powerful regular expression engine. And from there I could store everything into a hash, and the hard work would be done. It all worked as I expected.

There was some additional work to be done, however. I added some regular expressions to clear out any punctuation that may have come along with the words (remember I considered words as being separated by white-space). I also wanted to implement an optional ignore list which would ignore common words. This was easy enough, except that Word uses smart quotes and the words like “didn’t” weren’t matching. I had to do some research on how to turn smart quotes into dumb quotes. It wasn’t as easy as it should have been, since for some reason the way AppleScript transfers things, it loses the unicode values. It instead comes through as a weird looking string like “‚Äô”. Once I got that squared away, the rest was simple.

You can download the Perl script [here](/weekly/wordfreq.zip "Word Frequencies for Word 2008").

Note: This requires that you have Microsoft Word for Mac running, with a document open. It uses AppleScript so it can’t work on any other platforms other than Mac. If you need this functionality on Windows, see below for my Office 2007 Windows add-in, or refer to the VBA script mentioned in the Purpose introduction.

**Visual Studio Results:**  
I was very impressed by the MSDN documentation for the Visual Studio Office Add-In development process. I literally have no experience doing this, but I was able to pick it up in no time at all. Designing the Ribbon interface is so easy that a third grader could do it, and the actual coding was rather straight-forward once I figured out how to get the whole document into a string. The algorithm and method is exactly the same as the Perl one; I use heavy regular expressions and hash tables as the data structures. The regular expressions were actually easier since the smart quotes actually came in as their normal unicode characters and not the strange things I mentioned with the Applescript solution. So all I had to do was something like this:  
```
// convert all smart quotes to straight quotes
document = Regex.Replace(document, @"[“”]", "\"");
document = Regex.Replace(document, @"[‘’]", "'");
``` 
There wasn’t much beyond the porting other than choosing to Publish it and running the setup.exe. Since the code is nearly identical (just in C# instead of Perl), I didn’t bother exporting the source code. Instead I have just included the binary setup.exe file for your downloading. If you really want the source, e-mail me.

Download the setup executable file [here](/weekly/WordFrequencyWindows.zip "Word Frequency Word Add-In for Word 2007 Windows").

Note: Running the setup file will install the add-in to Word 2007. It will appear in the ribbon at the top of the screen with the label “Word Frequency Analysis”. Just click on that, check the box whether you want to ignore common words or not, then press Analyze. On large documents it will take a second. Then the results will show up directly in the ribbon, in the “Results” box. Only the top 12 words are shown.

**Conclusion:**  
Perl’s module support never ceases to amaze me. There is truly one for every possible purpose. This whole project gave me a new-found appreciation for AppleScript as well, because now I know I can get any raw data I need from Mac applications, letting AS take care of the IPC which would be tedious in Perl or another language, and then use Perl to process the data. It seems like a nice programming model for anyone looking to hack up some quick solutions on a Mac platform, by using a program which already has the information you need.

Visual Studio also never ceases to amaze me. I absolutely love how easy it was for me to do this Word add-in with very little clue what I was doing. I will probably make some Word add-ins in the future now that I know how easy it is.