---
layout: posts
title: Boolean Arithmetic and the ALU
categories: projectJournal
caturl: "/blog/project-journal.html"
---
➤ NAND2TETRIS PART 1 PROJECT 2
<br><br>No concept notes, since I already have good background for this part (Shoutout to MIPS assembly language). Instead, here is a follow-up of the inquiry from my <a href="/projectjournal/2023/03/28/boolean-functions.html" target="_blank"><u>p</u>revious post</a>, <i>Boolean Functions and Gate Logic</i>.
<br><br>
Boolean Algebra Equivalents (Cont.)
<br><br>
I was a little confused by the posts I'd read, which implied that a truth table can directly make clear the simplest way to notate a Boolean function. After searching around online, I found that a Karnaugh map, or <i>k-map</i>, is probably the "truth table" people were talking about. The k-map technique involves drawing out 2D representations of Boolean functions so that logical adjacencies become visually evident. As an example, here is how I simplified DMux using the technique.
<br><br><a href="/images/for-posts/simplify_dmux.png" target="_blank"><img src="/images/for-posts/simplify_dmux.png" width="500px"></a>
<br><br>Because I have both built its compenents as minimally as possible, and used the k-map technique to further simplify it, this seems to be the most minimal way to build DMux.
<br><br>
<b>Project Notes</b>
<br><br>
The following describes major ideas I considered while writing the HDL for the adder, incrementer, and ALU chips specified in <a href="https://www.nand2tetris.org/project02" target="_blank"><u>N</u>and2Tetris Project 2</a>.
<br><br>Definition of NAND
<br><br>Briefly had this misconstrued while building FullAdder.
NAND is NOT(A AND B), not (NOT A) AND (NOT B).
<br><br>
ALU in <20 lines of HDL?
<br>(It's stated in the course materials that the ALU can be specified in less than 20 lines of HDL, so I wanted to achieve that.)
<br><br>I'd originally tried to "zero" inputs by AND'ing them with NOT zx (or for the y-input, NOT zy), but of course, ran into the "different bus widths" error. Then, it hit me that I could write a Mux16 with a=input, b=false, and sel=zx (or NOT zy, for the y-input). In general, I'd forgotten about some of the chips I'd previously built, and was stuck until it hit me that they existed.
<br><br>I also found myself running into the "sub bus of an internal node" error several times while writing the HDL for detecting zero or negative outputs. It worked after some syntactical adjustments, though. I used two 8-way ORs, an OR, and a NOT to detect zeros, and simply outputted the "out" value's most significant bit to detect negatives.
<br><br>Ultimately, I ended up with a pretty clean 15 lines of HDL.
<br><br>
Topics for further investigation:
<br><br>
Carry-lookahead Adder
<br><br>
I want to try building an adder that can detect whether a bit pair will generate or propagate a carry.
<br><br>