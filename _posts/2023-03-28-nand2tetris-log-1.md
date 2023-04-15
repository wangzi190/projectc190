---
layout: posts
title: Nand2Tetris Log 1
categories: projectJournal
---
<br>
<b>Concept Notes</b>
<br><br>
All boolean functions can be represented by "listing" every combination of inputs that result in an output of 1, like so:
<br><br>OR(a, b) = a⋅b + ā⋅b + a⋅b̅
<br><br>This is the <i>canonical notation</i> of the OR function. It indicates that OR(a, b) will return true when a AND b is true, OR when (NOT a) AND b is true, OR when a and (NOT b) is true.
<br><br>Because all boolean functions can be represented with the binary operations AND, NOT, and OR, any logic gate can be constructed using the gates NAND and NOR. These gates are referred to as <i>primitive</i> because they can be used as the building blocks for more complex functions, and because they are the most physically simple.
<br><br>
<b>Project Notes</b>
<br><br>
I was quickly able to write the hardware description language (HDL) for the AND, NOT, OR, XOR, MUX, and DMUX operations. So, the following describes significant ideas I considered in writing the remaining operations.
<br><br>
From AND to AND16
<br>Is using 16 AND gates to build the AND16 operation the most efficient approach?
<br><br>
Hardware description language (HDL) is a specification of the parts of a computer chip, not a calling of operations like in high-level programming.
Though it appears redundant, it'd make sense that the HDL for the AND16 operation specify 16 different AND gates, because that way, 16 bits can be operated upon in parallel.
<br><br>NOTE: Revisit when the course covers system buses.
<br>Revisiting: Okay, turns out a system bus is just a different way to conceptualize a set of <i>n</i> bits being operated upon, but it really does just require the use of <i>n</i> gates.
<br><br>
NAND vs NOR
<br>Rewriting the operations I constructed from NAND gates in NOR gates to see which is more efficient.
<br><br>I want to do this because I figure if the OR operation could be done in 2 NOR gates instead of 3 NAND gates, there might also be better ways to write the other operations. However, the Nand2Tetris software suite doesn't come with a built-in NOR the way it has built in NANDs, so I have no choice but to revisit this later if possible.
<br><br>
Revising MUX and DMUX
<br><br>
4-way and 8-way MUX and DMUX?
<br><br>