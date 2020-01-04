---
author: Rob
categories:
- Projects
date: "2011-12-16T23:03:33Z"
guid: http://robwilliams.me/?p=242
id: 242
title: Flashcard .NET Application
url: /2011/12/flashcard-net-application/
---
I’ve been using [Mnemosyne](http://www.mnemosyne-proj.org/) for the past year and a half with a decent amount of success. I like the concept of spaced repetition and believe it to be a legit technique. However, the main content that I have been memorizing is Bible verses, and the spaced repetition doesn’t seem to be working very well. It simply does not repeat the cards enough for me to retain the memorized verses. I think the reason is because the technique is meant to be used for learning vocabulary in a language, or otherwise rather simple recollections involving one or two words per card, whereas verses are entire sentences.

After much frustration with the spaced repetition, I decided to do a very simple flash card system. I got the idea to simply allow the user to apply a weight to each card in the deck, and then when running through the deck it will give it to you in weighted order. This doesn’t employ any type of spaced repetition, but the idea is that you could pick up the application for 5 minutes a day and just run through cards. If you ever feel like you want to practice the ones you know the least, you just “reset” the deck and the high weighted cards will be shown to you first. It’s a very roll-your-own sort of system where you have to really know what you are trying to get out of it, and use the weights appropriately. But I do think it has a nice level of flexibility where there may be some emergent uses for this I am not even thinking of right now. The point is, I’ve been using it for the past week and it’s been working very well for me. Maybe someone else will find it useful.

Feature List

  * Import exported Mnemosyne XML files
  * Add/Edit/Delete slides (question and answer flashcards)
  * Mark cards as memorized or not memorized at any point
  * Assign weight to cards

If you are trying to use the program, press the “H” key to show the Help Window which will explain what the key mappings are. Sorry for the minimalistic UI, but I am very keyboard-centric and didn’t want to design a full UI with buttons for everything. It will require the .NET Framework 4 to run or build.

&nbsp;

The [latest source](https://github.com/robwil/VerseMemory) is available on Github.

The build as of 12/17 is available [here](http://robwilliams.me/weekly/VerseMemory.exe "VerseMemory EXE download").

[![VerseMemoryScreen](http://robwilliams.me/wp-content/uploads/2011/12/VerseMemoryScreen.jpg)](http://robwilliams.me/wp-content/uploads/2011/12/VerseMemoryScreen.jpg)