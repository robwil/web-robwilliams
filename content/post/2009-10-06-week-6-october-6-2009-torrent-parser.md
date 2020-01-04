---
author: Rob
categories:
- Projects
date: "2009-10-06T20:13:56Z"
guid: http://robwilliams.me/?p=80
id: 80
title: Week 6 – October 6, 2009 – Torrent Parser
url: /2009/10/week-6-october-6-2009-torrent-parser/
---
Just a quick note: You may have noticed I skipped a week. I know there shouldn’t be any excuses, but I had two tests last week and I started tutoring which takes up about 4-5 hours per week (which is basically the 4-5 hours free where I normally program these projects). Even more, I had no good ideas so I really had no motivation. Hopefully this won’t happen again.

**Purpose:**  
A friend of mine told me the other day he decided to open up a .torrent file in Notepad just for kicks. He did it just to see if it was a text-only file or whatever. In any case, he quickly closed it because there was far too much gibberish. However, I decided to follow suit. Only I am not on Windows, so I used [TextMate](http://macromates.com/) (great text editor, by the way — and it’s pretty much synonymous with Ruby programming on Mac). As I opened it, I noticed the filenames of the torrent in plain text. I also noticed that in front of each string was a number representing the length of the string. There was a lot of garbage data in the file, which I expected to be the hash values for the pieces. It turns out I was right on all accounts, except I was missing a few key details.

I found an excellent resource via Google which [outlines the BitTorrent specification](http://wiki.theory.org/BitTorrentSpecification). Being the passionate programmer I am, I decided this was a good opportunity to learn how .torrent files tick. After all, I use them quite often to download <del datetime="2009-10-07T02:37:35+00:00">certain media files</del> non-copyrighted public domain files.

This week, I only wrote a solution in Ruby. I can’t imagine the other languages would have allowed me to create such a simple solution. Ruby just has so many nice things related to parsing files, not to mention its built-in support for lists (aka Arrays) and dictionaries (aka Hashes) made storing the various data types in the bencoded torrent files rather easy.

**Ruby Results:**  
It turns out .torrent files use a binary specification called “bencoding”, which has four basic types: Strings, Integers, Lists, and Dictionaries. On first glance it seemed like an easy task… but then I discovered that Lists and Dictionaries can contain other Lists or other Dictionaries, which can in turn contain Lists and Dictionaries. It turns out parsing it isn’t so simple. However, with a little recursion, the task was tackled in just over 40 lines of code. I’m actually surprised my code worked with minimal debugging, considering the complex nature of the parsing. But I suppose once you can define something recursively, the magic of recursion does all the hard work for you.

Once I implemented a class to decode bencoded files, I proceeded to actually getting the information from the torrent, such as the file names, creation date, and trackers. This was a simple exercise in parsing hashes and arrays.

One thing I can’t help but draw attention to is a particularly awesome example of Ruby code which I used in the program. The way .torrent files store the hashes for each piece of the torrent (every file in the torrent is broken up into pieces which receive a hash — this way the integrity of the files can be verified before the whole file is downloaded) is in one single string. Each piece has a 20-byte SHA1 hash, and all of these hashes are concatenated to one huge string.

The problem is that the string, when printed out, doesn’t come out quite as expected. This is because SHA1 encodes its hashes in all 256 available bytes, while there are only about 94 printable characters in the ASCII standard. When printing non-printable characters, all kinds of weird things can happen, such as random newlines and who knows what else. In general, printing raw binary data to the terminal is a BAD idea. What people usually do in the case of hashes is print out the ASCII value in hex, so that there are two characters per byte. For example, ASCII code 13 which is normally a newline and would mess up output if its string-value was printed, would be outputted as “0D” because 0D is the hex for 13.

There are a million ways to convert between ASCII values and hex and such, but the way I did it makes heavy use of Ruby’s built-in methods. I have commented the code to explain what is happening, for those not well-versed in Ruby API (and I’m not going to lie — lots of API skimming was required for me to write this code):

<pre lang="ruby">pieces = metainfo['info']['pieces']
amount_of_pieces = pieces.length / 20
for i in 1 .. amount_of_pieces
    print "Piece " + i.to_s + ": \t"
    pieces.slice!(0..19).split(//).each {|c| print c.unpack("H2").to_s + " " }
    # .slice!(0..19) takes 20 bytes off front of string -- each piece's hash is 20 bytes
    # .split(//) turns string into array e.g. "test" becomes ["t", "e", "s", "t"]
    # .each iterates over array created in last line
    # |c| makes variable c which represents the current character we're dealing with (in the loop)
    # c.unpack("H2") gets the hex value of the character
    # we print out the hex value each time, followed by a space
end</pre>

With that excitement out of the way, you can download and run the source code [here](/weekly/Week6_Ruby_TorrentParser.zip).

**Conclusion:**  
Programming this project was genuinely fun. I learned a ton about the .torrent file format, and reinforced my knowledge of file parsing. I foresee another project in the future where I parse files, except I want to do a more complex one like .PNG, for example. It’s probably important to note for those who have no clue what I mean by a “Torrent Parser” that my program doesn’t really do anything practical. It just outputs the data which is stored inside a .torrent file. This is the same data that a program like uTorrent would use to connect to the tracker, get peers and seeders, and proceed with your download. However, my program does no such thing. It just tells you what is in the file and nothing else. Still, I think it’s quite interesting at least on an academic level.