---
layout: project-post
title: Nand2Tetris - 05 | Computer Architecture
categories: projects
permalink: /projects/nand2tetris/computer-architecture
---
Building the memory and CPU of the Nand2Tetris Hack computer and completing its hardware platform using components from the previous projects. 
<br><br>
<a href="https://www.nand2tetris.org/project05" target="_blank">Project specifications</a> | <a href="https://github.com/wangzi190/nand2tetris/tree/master/05" target="_blank">My work</a>
<br><br><u>Note</u>: I've decided to focus for the most part on completing each project to the best of my abilities, instead of getting sidetracked thinking about what more I could be learning about each concept (Because, well, there's always more). I'll still make note of what I'm still thinking or wondering about at the end, but won't give myself pressure to do a deep-dive on everything.
<br><br><b>Concepts</b>
<br><br>The typical computer hardware platform is made up of a CPU, which is made up of an ALU, registers, and a control unit, main memory unit(s), which store information pertaining to data and instructions, and a ROM unit, which contains critical resources such as start-up instruction data. Despite the fixed nature of the hardware platform, the computer is able to perform an immense variety of tasks by executing the instructions stored in its memory chips in different ways.
<br><br>Two of the most notable computer architectures are the Harvard architecture and the von Neumann architecture (aka Princeton architecture). The primary difference between the two is that the former has distinct address and data buses for instructions and for data whereas the latter does not.
<br><br><b>Process</b>
<br><br>This project mostly involves strategically piecing together the chips from the previous projects, so it's relatively easy as long as your previous chips work, and you understand the Hack computer's instruction encoding/decoding scheme.
<br><br><i>A Instructions vs C Instructions</i>
<br><br>While testing my CPU, I realized that A instructions, which simply load data into the A register (and nothing else), were unintentionally triggering jumps and memory writes. To fix this, I made sure to check the leftmost bit of the instruction input (which determines if the instruction is an A or C instruction) when calculating the load value for the program counter and the writeM CPU output.
<br><br>Then, while testing the Rect.hack program on my computer chip, I realized, by checking the generated ComputerRect.out file, that A instructions were also unintentionally causing the information in the D register to change. Similarly, I fixed this issue by checking the leftmost bit of the instruction input when calculating the load value for the D register.
<br><br><b>Additional Comments</b>
<br><br>I'm not really left with any specific questions; I just generally feel that I could better understand how a CPU works. I also feel that my CPU is not as efficient as it could be, so I may come back to this project later to clean it up a bit more.