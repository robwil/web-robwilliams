---
author: Rob
categories:
- Projects
date: "2009-11-19T13:06:06Z"
guid: http://robwilliams.me/?p=91
id: 91
title: Week 8 – November 19, 2009 – Reading Buddy
url: /2009/11/week-8-%e2%80%93-november-19-2009-%e2%80%93-reading-buddy/
---
**Purpose:**  
Have you ever been reading a book, knowing you wanted to finish it by a certain date, but not in the mood to do the math required to figure out how many pages you should be tackling a day? I know, it probably doesn’t come up so often where you need to read a book for a dead line, but regardless, I thought it might be nice to have a program which helps with this.

The program I created prompts for the amount of pages in the book, how many have already been read, reading speed (in pages per hour), and the Finish By date. It also allows you to specify the maximum number of hours you can read per day of the week (for example, I can read way more on Wednesdays than Tuesdays due to my class schedule).

**Flex Results:**  
I only decided to implement this program in Flex this week. The program’s GUI design was rather straight forward and as always Flex made everything very easy. As for the actual calculations involved in making up the reading schedule, I decided to make an algorithm that in my mind optimizes the schedule. I thought the most desirable feature was to spread out the hours you are expected to read per day as much as possible, so that the amount read each day was minimized, but still enough so that you would reach the goal.

It accomplishes this by looping through the days over and over again, assigning each day the average amount of hours required per day (calculated from the average amount required per week divided by 7 days in a week), unless of course that average amount is greater than the maximum for that day. In those cases, it maximizes the hours that are read that day, and then on the next iteration of the loop, doesn’t consider that day as part of the days when it divides average amount per week by the amount of days (e.g. if Monday is already at its max hours, the next iteration would do required\_hours\_per\_day = required\_hours\_per\_week / 6, as opposed to dividing by 7 in the first iteration). This continues until the hours left per week is less than 0.1, since it will never reach 0 due to floating point arithmetic, and because 0.1 hours is six minutes and I doubt that will affect things too much.

![Reading Buddy screenshot](/images/screens/ReadingBuddy.jpg) 

It would probably make more sense if you read the code, which you can do by right-clicking in the program and going to View Source, as always.

You can download the program with source code [here](/weekly/Week8_Flex_ReadingBuddy.zip "Week 8 Flex Program").

**Conclusion:**  
This was a quick exercise, but the algorithm was fun to think up. I’m sure it has a lot of bugs, because I’ve only really tested deadlines that are more than one week away. Also the program isn’t terribly useful because right now there is no way to export the reading schedule (to Excel, for example, would be nice). I don’t really plan to use this program, or else I would add more features. If anyone does plan to use it, let me know and I can add a lot of things and clean it up, making sure everything works as its supposed to.