---
layout: posts
title: Boolean Arithmetic and the ALU
categories: projectJournal
caturl: "/blog/project-journal.html"
---
➤ NAND2TETRIS <a href="https://github.com/wangzi190/nand2tetris/tree/master/02" target="_blank"><u>P</u>ROJECT 02</a>
<br><br><b>Concept Notes</b>
<br><br>Most computer systems represent signed integers using a method called 2's complement, where, in a fixed number of bits, negative integers are represented using the negation of their absolute value plus 1. This method is convenient for a number of reasons:
<ul>
    <li>Negative integers can seamlessly be included in binary addition operations.</li>
    <li>There is only one possible representation of 0.</li>
    <li>The most significant bit of any integer indicates its sign (0 for positive integers, and 1 for negative integers).</li>
</ul>
The arithmetic logic unit (ALU) is a computer chip that performs arithmetic and bitwise operations on integers represented in 2's complement.
<br><br><b>Project Notes</b>
<br><br>
The following describes major ideas I considered while writing the HDL for the adder, incrementer, and ALU chips specified in <a href="https://www.nand2tetris.org/project02" target="_blank"><u>N</u>and2Tetris Project 02</a>.
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