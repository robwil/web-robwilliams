---
author: Rob
categories:
- Projects
date: "2009-09-02T14:00:26Z"
guid: http://robwilliams.me/?p=54
id: 54
title: Week 2 – September 2, 2009 – Calculator
url: /2009/09/week-2-september-2-2009-calculator/
---
**Purpose:**  
I think one of the best ways to make sure you know all the aspects of a graphical programming environment is to make a calculator program. More often than not, all of the actual functionality of the calculator is provided by the programming language itself. Things like addition, division, square roots, and logarithms are usually part of the built-in functions. Therefore, making a calculator is less about the actual code and more about handling click events and displaying a result on the screen. Dealing with floating point numbers also lets you dabble with formatting output.

The iPhone environment uses Cocoa-derived UIKit to provide various built-in buttons, text fields, etc. Graphical programs are designed and laid out using Interface Builder and then saving it to an XIB file, which the program itself uses to show the interface when it starts up. One of the key concepts of iPhone development is connecting the graphical components in Interface Builder to the source code in XCode using Outlets and Actions. 

I was originally going to do this project in Flex as well (but not Ruby because a web interface for a calculator would be either very clunky or very taxing on the server with a million AJAX calls), but after the implementation turned out to be quite tedious (handling events from ~20 buttons can do that), I decided against it. Besides, I have recently been relearning Flash and practicing a lot with ActionScript (finishing up my [Lynda.com](http://lynda.com) Essential Training video series for AS3), so I didn’t think it would be a problem if I only did this project for iPhone.

**iPhone Results:**

Since I was developing for iPhone, screen real estate was a concern. But then I decided I was only going to implement a fairly basic calculator (supporting addition, subtracting, multiplication, division, decimals, square roots, and powers), which left a lot of space open for more things. What I decided to do with all the open space was implement a type of “memory bank” where the user can press the “sto” (store) button to save a value, and then be able to subsequently “rcl” (recall) it. I implemented the memory system using a UIPickerView, which is quite a fancy component provided by UIKit. If you’ve ever used the iPhone App called Urban Spoon, or set an alarm for a certain time using iPhone’s Clock App, you’ll be familiar with the “Picker”. It lets you slide the values up and down until you select one — it resembles a slot machine more than anything. I thought it was a nice feature for a calculator to have, and it perfectly filled up the space I had left.

The whole development took 3:04, which is much less than the iPhone app took me last time. I again ran into some Objective-C issues, since I expect to be able to do C-like things and have them work, when they often don’t. Other than some issues using enums to make my big switch statement look nicer, the majority of time was spent debugging the actual functionality. I probably should have put more thought ahead of time into the state variables to keep track of intermediate values and such, but it all turned out okay in the end. The UI is kind of spartan, but I wasn’t trying to win a design award. Apart from that, there isn’t much to say. It’s just a calculator after all.

You can download the source code [here](/weekly/Week2_iPhone_Calculator.zip "Week 2 iPhone Source Code").

**Conclusion:**

Developing a calculator for the iPhone certainly accomplished my goal. It made me better understand the way the GUI components interact, and now I venture to say that I fully understand the Action/Outlet MVC architecture that iPhone uses. In the future I expect to be able to create applications with whatever interface I can dream up.

I still have no clue how Obj-C memory management works. In sample code for iPhone apps, I see a lot of crazy memory stuff going on, with AutoRelease pools and releases all over the place. Like I said last week, it definitely doesn’t work like Java’s garbage collector. Maybe in the future I will do a project that will help me figure that out.

I just wanted to add a quick note about my laziness this week in only implementing the project in one environment. Since I foresee a repeat of this behavior, I have decided to make it a rule for myself that I can’t ever skip a language/environment more than twice in a row. That way, I never go more than three weeks without practicing in any given language, to keep myself fresh.