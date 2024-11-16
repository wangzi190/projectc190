---
layout: posts
title: Nand2Tetris - 06 | Assembler
categories: computer-projects
---
Writing an assembler for the Hack assembly language.
<br><br>
<a href="https://www.nand2tetris.org/project06" target="_blank">Project specifications</a> | <a href="https://github.com/wangzi190/nand2tetris/tree/master/06" target="_blank">My work</a>
<br><br><b>Concepts</b>
<br><br>The circuitry that makes up a digital computer's underlying hardware can only recognize sequences of 1s and 0s, called machine code. Each line of machine code encodes a specific operation, such as "Add the contents of registers 1 and 2, and store the result in register 3", according to the wiring of the hardware, and agreed upon conventions for writing the code. In order to make the computer reasonably usable at this level, computer processors come hand in hand with human-readable ways of notating their respective machine code, called assembly language (e.g. "Add the contents of registers 1 and 2, and store the result in register 3" in MIPS machine code is 0x00221820, whereas in MIPS assembly language it would simply be <i>add $3, $1, $2</i>). Therefore, assembly language instructions must be translated into their binary code equivalents before being loaded into memory for executing. In the past, this meticulous process would have been done by people, but in the modern day, it is done by a computer program called the <i>assembler</i>.
<br><br><b>Process</b>
<br><br><i>Handling Program Symbols</i>
<br><br>Seeing as I got a non symbol-handling version of the Nand2Tetris assembler working pretty fast, I'd agree that the trickiest part of developing the Nand2Tetris assembler is, like stated in the project information, the symbol handling.
<br><br>(So that the following description is not confusing: I decided to make a two-pass assembler, where the first pass handles symbols and generates no code, and the second pass generates code and handles variables. I struggled with the code for the first pass.)
<br><br>I wrote several overcomplicated and incorrect first pass scripts, one of which activated a "search mode" upon reading a pseudoinstruction, and added an entry to the symbol table upon reading the first instruction to pass after the activation of this "search mode". This didn't work because it'd fail when processing two or more consecutive pseudoinstructions. To try and fix this, I changed the program so that when it detected a pseudoinstruction, it'd stop the overall processing of the assembly language file to simply read the following few lines until it found the next instruction. This was not only terribly inefficient, but also resulted in incorrect line counts in the entries on the symbol table.
<br><br>Finally, I sobered up and realized that I simply had to increase the line count when reading an instruction, or add an entry to the symbol table with the current line count when reading a pseudoinstruction. This works in all cases because the instruction that any pseudoinstruction should point to happens to be the one at the line count when reading the pseudoinstruction, regardless of how many pseudoinstructions there are consecutively.
<br><br>Here's what I wrote out to confirm this.
<br><img src="/images/for-posts/label_handling.jpg" width="412px">
<br>(See above.) The number to the left of each line is the "technically..." numbering, whereas the small numbers to the right of each instruction line is how the program should be numbered in practice. Notice how the "technically..." numbering of the pseudoinstructions match with the true numbering of the instructions they should point to.
<br><br><i>Mildly Confusing Specifications</i>
<br><br>The predefined symbols table in the textbook threw me off until I realized that it simply meant that the same RAM addresses had distinct names. (There can be many labels to a single address, but not vice versa). Accordingly, I had to change the tables in my program so that the label names were the unique keys instead of the address values.
<br><br><b>Additional Comments</b>
<br><br>I decided to write the assembler program in C++ to get more familiar with the language. After writing it, I am still not sure what is best practice for a project like this. In general, I think I got the conceptual understanding I needed from this project, but could use more practice making these types of programs.