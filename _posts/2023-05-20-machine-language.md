---
layout: posts
title: Nand2Tetris / Machine Language
categories: projectLog
caturl: "/blog/project-log.html"
---
<a href="https://www.nand2tetris.org/project04" target="_blank">Project information</a> | <a href="https://github.com/wangzi190/nand2tetris/tree/master/04" target="_blank">My work</a>
<br><br><b>Concepts</b>
<br><br>
Whereas commands in high level languages such as Java or Python embody logical concepts (e.g. "if [this event] occurs, execute [this code]"), assembly language machine instructions correlate directly to the functions of a processor chip. So, assembly machine instructions can be directly translated into the sequences of 1s and 0s read by the computer processor, called <i>machine language</i>. The data involved in assembly programs are also sequences of 1s and 0s, generally notated in hexadecimal for the sake of readability.
<br><br><b>Process</b>
<br><br><i>Nand2Tetris Assembly Language Conventions</i>
<br><br>Nand2Tetris assembly is particular because each machine instruction, being only 16 bits long, cannot encode operands as part of the instruction. Therefore, operands must be loaded into designated registers, called the D (data) and A (address) registers, before operations can commence. Also, programs need to be "terminated" with infinite loops—probably so that the test script ends, and thus so does the program. I needed to consider these details as I brainstormed different approaches.
<br><br><i>Mistakes</i>
<br><br>On several occasions, I'd do a calculation, store the result in the D register, but then forget to store it in a different register, meaning the result of the calculation would be overriden by the next. I'd also sometimes get confused as to whether I was working with the address of a register or the data in it, but managed to figure things out by running it on the emulator.
<br><br>I also struggled a lot more on the screen fill program than the multiplication one. I think my mistake was that I ran a loop to check for key presses, but then tried to jump from that loop to a different loop which would either fill or erase the screen. What ended up working was writing a loop that'd run through the "check for key presses" code on every cycle, which meant that the program could detect a key being pressed and <i>held</i>, and change pixels on the screen for as long as it continues to detect a key being held.
<br><br>I commented both programs for this project pretty extensively. You can view them <a href="https://github.com/wangzi190/nand2tetris/tree/master/04" target="_blank"><u>h</u>ere</a>.
<br><br><b>For Further Inquiry</b>
<br><br>Further investigation I can do on the topic of assembly language are better done with other textbooks and languages, so I think I can move onto the next project now.