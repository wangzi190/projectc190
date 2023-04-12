---
layout: posts
title: Nand2Tetris Log 1
categories: projectJournal
---
<br>
<b>Concepts</b>
<br><br>
All boolean functions can be represented by "listing" every combination of inputs that result in an output of 1, like so:
<br><br>OR(a, b) = a⋅b + ā⋅b + a⋅b̅
<br><br>This is the <i>canonical notation</i> of the OR function. It indicates that OR(a, b) will return true when a AND b is true, OR when (NOT a) AND b is true, OR when a and (NOT b) is true.
<br><br>Because all boolean functions can be represented with the binary operations AND, NOT, and OR, any logic gate can be constructed using the gates NAND and NOR. These gates are referred to as <i>primitive</i> because they can be used as the building blocks for more complex functions, and because they are the most physically simple.
<br><br>
<b>Documentation</b>
<br><br>
I was quickly able to write the hardware description language (HDL) for the AND, NOT, OR, XOR, MUX, and DMUX operations. So, the following details how I revised and expanded upon the HDL for those operations.
<br><br>
From AND to AND16
<br>Is using 16 AND gates to build the AND16 operation the most efficient approach?
<br><br>
Hardware description language (HDL) is a specification of the parts of a computer chip, not a calling of operations.
Though it appears redundant, it'd make sense that the HDL for the AND16 operation specify 16 different AND gates.

With 16 different gates, 16 AND operations can be performed in parallel. 

Each AND gate takes a 1-bit input, so 16 different AND gates would allow for an AND operation to be done on two 16-bit inputs at the same time.
<br><br>
NAND vs NOR
<br>Rewriting the operations I constructed from NAND gates in NOR gates to see which is more efficient.
<br><br>
Text
<br><br>
Revising MUX and DMUX
<br>
<br><br>