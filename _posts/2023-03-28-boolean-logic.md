---
layout: posts
title: Nand2Tetris - 01 | Boolean Logic
categories: computer-projects
---
Building a set of elementary logic gates and their bitwise counterparts.
<br><br>
<a href="https://www.nand2tetris.org/project01" target="_blank">Project specifications</a> | <a href="https://github.com/wangzi190/nand2tetris/tree/master/01" target="_blank">My work</a>
<br><br><b>Concepts</b>
<br><br>
All boolean functions can be represented by "listing" every combination of inputs that result in an output of 1, like so:
<br><br>OR(a, b) = a⋅b + ā⋅b + a⋅b̅
<br><br>This is the <i>canonical notation</i> of the OR function. It indicates that OR(a, b) will return true when a AND b is true, OR when (NOT a) AND b is true, OR when a and (NOT b) is true.
<br><br>Because all boolean functions can be represented with the binary operations AND, NOT, and OR, any logic gate can be constructed using the gates NAND and NOR. These gates are referred to as <i>primitive</i> because they can be used as the building blocks for more complex functions, and because they are the most physically simple.
<br><br>
<b>Process</b>
<br><br>
<i>From AND to AND16</i>
<br><br>While writing my HDL, I found myself wondering: Is using 16 AND gates to build the AND16 operation the most efficient approach?
<br><br>
Hardware description language (HDL) is a specification of the parts of a computer chip, not a calling of operations like in high-level programming.
Though it appears redundant, it'd make sense that the HDL for the AND16 operation specify 16 different AND gates, because that way, 16 bits can be operated upon in parallel.
<br><br>Additional note: A system bus is just a different way to think of a set of <i>n</i> bits being operated upon, so it really does just require the use of <i>n</i> gates.
<br><br>
<i>4-way and 8-way MUX16</i>
<br><br>
In hindsight, I feel incredibly stupid for this. But, I spent a while wrestling with a bunch of irrelevant gates until I realized I could write 4-way and 8-way MUX16 with only several MUX16 gates. I also forgot that the buses are indexed right to left instead of vice versa, which also got me stuck for a while. Thankfully, this means I probably won't be making similar mistakes again.
<br><br>
<i>8-way DMUX</i>
<br><br>
Think it's worth mentioning that I had the most fun figuring this one out :)
<br><br>(Click an image to view it in full.)
<br><br>
<a href="/images/for-posts/paper_dmux.png" target="_blank"><img src="/images/for-posts/paper_dmux.png" width="300px"></a>
<br><br><a href="/images/for-posts/dmux8way_sc.PNG" target="_blank"><img src="/images/for-posts/dmux8way_sc.PNG" width="412px"></a>
<br><br>
<b>For Further Inquiry</b>
<br><br>
<i>Simplifying Circuits</i>
<br><br>
I read on EE StackExchange that someone used truth tables and DeMorgan's laws to simplify all the chips as much as possible, and want to figure out the specifics of how to do that.
<br><br>UPDATE: Continued <a href="/projectlog/2023/05/02/simplifying-circuits.html" target="frame"><u>h</u>ere</a>.
<br><br>
<i>NAND vs NOR</i>
<br><br>I'd like to rewrite the basic boolean operations I constructed from NAND gates in NOR gates to see which is more efficient. I figure if the OR operation could be done in 2 NOR gates instead of 3 NAND gates, there might also be better ways to write the other operations. However, the Nand2Tetris software suite doesn't come with a built-in NOR the way it has built in NANDs, so I'll see if I can come back to this once I'm familiar enough with the Nand2Tetris software to set it up myself.
<br><br>UPDATE: I think I understand the concepts well enough to be able to drop this idea.