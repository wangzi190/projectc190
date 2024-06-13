---
layout: posts
title: Nand2Tetris / Boolean Arithmetic and the ALU
categories: projectLog
---
<a href="https://www.nand2tetris.org/project02" target="_blank">Project information</a> | <a href="https://github.com/wangzi190/nand2tetris/tree/master/02" target="_blank">My work</a>
<br><br><b>Concepts</b>
<br><br>Most computer systems represent signed integers using a method called 2's complement, where, in a fixed number of bits, negative integers are represented using the negation of their absolute value plus 1. This method is convenient for a number of reasons:
<ul>
    <li>Negative integers can seamlessly be included in binary addition operations.</li>
    <li>There is only one possible representation of 0.</li>
    <li>The most significant bit of any integer indicates its sign (0 for positive integers, and 1 for negative integers).</li>
</ul>
The arithmetic logic unit (ALU) is a computer chip that performs arithmetic and bitwise operations on integers represented in 2's complement.
<br><br><b>Process</b>
<br><br><i>Definition of NAND</i>
<br><br>Briefly had this misconstrued while building FullAdder.
NAND is NOT(A AND B), not (NOT A) AND (NOT B).
<br><br>
<i>ALU in <20 lines of HDL?</i>
<br><br>(It's stated in the course materials that the ALU can be specified in less than 20 lines of HDL, so I wanted to achieve that.)
<br><br>I'd originally tried to "zero" inputs by AND'ing them with NOT zx (or for the y-input, NOT zy), but of course, ran into the "different bus widths" error. Then, it hit me that I could write a Mux16 with a=input, b=false, and sel=zx (or NOT zy, for the y-input). In general, I'd forgotten about some of the chips I'd previously built, and was stuck until it hit me that they existed.
<br><br>I also found myself running into the "sub bus of an internal node" error several times while writing the HDL for detecting zero or negative outputs. It worked after some syntactical adjustments, though. I used two 8-way ORs, an OR, and a NOT to detect zeros, and simply outputted the "out" value's most significant bit to detect negatives.
<br><br>Ultimately, I ended up with a pretty clean 15 lines of HDL.
<br><br>
<b>For Further Inquiry</b>
<br><br>
<i>Carry-lookahead Adder</i>
<br><br>
I want to try building an adder that can detect whether a bit pair will generate or propagate a carry.
<br><br>