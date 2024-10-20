---
layout: posts
title: Falling blocks, movement keys, & MARS hackfixes
categories: projectLog
---
Funnily enough, grinding out MIPS programs for class in the library until midnight last week reminded me that I actually really enjoy assembly language. I would love to know more about it, and reading my books only gets me so far, so I thought why not make a game in MIPS as a nice exercise? Snake seemed really overdone, and Centipede seemed a little ambitious, so I went with something I considered pretty reasonable for a casual, personal project: Tetris.

I wanted to focus more on program logic than the gritty details of memory allocation on some target MIPS-based device, so I figured I needed a simulator that facilitates I/O and a GUI mapped to a fixed set of memory addresses without extra complications. As such, I came across the <a href="https://www.softpedia.com/get/Programming/Coding-languages-Compilers/Vollmar-MARS.shtml" target="_blank">MARS MIPS simulator</a> online and downloaded it to my computer.

The program setup was pretty seamless on Linux; all I had to do was download the .jar file from the developer's website, right click it, go to Properties --> Permissions, and check "Allow executing file as program". From there you can run the simulator as long as you have a fairly modern version of Java installed on your computer.

To start, I configured the bitmap display to have 16px unit width, 16px unit height, 256 display width, and 512 display height, and kept the base address for the display set at 0x10010000.

Then, to get something going, I wrote a simple, non-terminating loop that erases and redraws a set of pixels 1 row lower on the screen with a buffer of 0.1 seconds between each movement. This creates the "falling block" effect I'll be using for the game.

In "real" assembly language, I'd speculate that deliberate time delays are created by getting system time from the BIOS, or by using the CPU's clock period to your advantage (though I can't imagine that'd be very practical). Thankfully, MARS includes a set of <a href="https://web.archive.org/web/20240913140616/https://courses.missouristate.edu/kenvollmar/mars/help/syscallhelp.html" target="_blank">syscalls</a> you can use for time delays and other things, so I didn't have to worry about that.

I figured that next I should read keyboard input so the player can move the blocks as they fall, or quit the game, because the infinite loop is an issue. Since I was already looking at MARS syscalls, I thought I'd have to use syscall 12 (read character) to read input, but that didn't work as intended. Instead, I found out by reviewing other MIPS and MARS programs that the ASCII value of the character last inputted is stored at address 0xFFFF0004, so I simply had to load data from that address to detect keypresses. However, it took me a while to figure out that I needed to open the "Keyboard and Display MMIO Simulator" under "Tools", connect it to MIPS, and type in the box for the simulator to register my keypresses.

While using the "Bitmap Display" and "Keyboard and Display MMIO Simulator" tools simultaneously, I found that a couple consecutive keypresses would cause the entire MARS simulator window to freeze. Through online search, I found a pretty convenient fix posted on a Wordpress site called <a href="https://web.archive.org/web/20221219133552/https://dtconfect.wordpress.com/2013/02/09/mars-mips-simulator-lockup-hackfix" target="_blank">Confect's Codex</a>, albeit for an IDE I do not use, but I got the idea and was able to make the fix with VSCode. I extracted the MARS Java source code from the .jar file, opened it in VSCode, and then edited the line in KeyboardAndDisplaySimulator.java as described in the guide. Initially, I had an issue where VSCode was not recognizing that I had Java installed, but I fixed it by opening VSCode from the command line, which apparently <a href="https://stackoverflow.com/questions/54653343/vs-code-refresh-integrated-terminal-environment-variables-without-restart-logout" target="_blank">refreshes environment variables</a>. I didn't want to have to run MARS through VSCode every time I wanted to use it, though, so I installed VSCode's <a href="https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack" target="_blank">Extension Pack for Java</a> and used the tool under the "Java Projects" tab to compile an updated .jar executable. To run it, I moved it outside of the MARS source code directory and simply clicked on it.

As of now, my program features a moveable but not rotatable falling Tetris block. It is pretty crude and unfinished, so it hasn't yet been published. Similar to my other project-related blog posts, I just thought I may as well document the process of setting up MARS and starting to write my program in case it may help someone else who wants to use these tools.

For next time I think I'll make the blocks rotatable, because detecting when a block should stop falling will depend on the rotational position of the block, and I don't want to have to backtrack.