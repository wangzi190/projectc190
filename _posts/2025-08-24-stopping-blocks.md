---
layout: project-post
title: Stopping blocks, clearing rows, and ending the game
categories: projects
permalink: /projects/mips-tetris/stopping-blocks
---
<style>
    .emote {
        vertical-align:middle;
        width:18px;
    }
</style>
<a href="#1">Stopping Blocks</a>
<br><a href="#2">Clearing Rows</a>
<br><a href="#3">Ending the Game</a>
<br><a href="#4">Residual Quirks</a>
<br><a href="#5">Project Trivia</a>
<br>The program code as of this point can be found <a href="https://github.com/wangzi190/mips_tetris/blob/main/tetris_v3%20NOW%20FUNCTIONAL!!.asm" target="_blank">here</a>.
<hr>
As I mentioned last time, I need my program to be able to resolve the following 3 questions:
<br>1) When should blocks stop falling?
<br>2) When should a line clear occur?
<br>3) When is the game over?

<span id="1"><b>Stopping Blocks</b></span>

Detecting when blocks should stop falling was pretty simple. I tried to lay out an elaborate plan beforehand, but realized I was only confusing myself and would probably have an easier time just coding directly. Thankfully I was right about that, and once I completed coding the obstruction detection function it worked seamlessly.

Basically, it works like this: The obstruction detection function is called in every iteration of the main game loop (or in simpler words, with every downward movement). It checks for obstructions by checking the pixels 1 below each pixel in the active block. ("Active block" is how I've been referring to the currently-falling block in my head; I think the name makes sense.) For each of those 1-below pixels, the function checks 2 things: 1) Is the pixel's hex color's rightmost bit 1? and 2) Does the pixel's memory address coincide with any of the memory addresses of the pixels in the active block? If at least one pixel's hex color's rightmost bit is 1 AND its memory address does not coincide with any of the memory addresses of the pixels in the active block, then there is an obstruction. This logic works because if a pixel's hex color's rightmost bit is a 1, it's a non-background pixel (i.e., an obstruction). The 2nd, equally as important condition is included to make sure that the program does not detect any pixels in the active block as an obstruction to itself.

To detect the bottom of the screen, I simply loaded 1 into the 16 memory addresses following those correlated to the bottommost row of the screen. This causes the row just underneath the bottom to be detected as an obstruction, and stops the blocks right where they need to be. Work smarter, not harder <img src="/images/emotes/nukoNerd2.gif" class="emote">

<img src="/images/for-posts/blocks_now_stack.png" width="390px">
<br><i>Voilà! It works.</i>

<span id="2"><b>Clearing Rows</b></span>

My first attempt at implementing row clears didn't go as smoothly, however. I started by programming something so convoluted I decided it'd be easier to delete it all and try again. On take two, I managed to accurately program most of the logic in a much simpler way, but a small detail I missed caused an interesting bug. Once I adjusted the code, though, it worked just as intended.

The row clear function is called every time a block stops falling after hitting an obstruction. In a nutshell, it 1) checks a row that contains a pixel in the just-stopped block, 2) erases the row if it is completely full, and 3) collapses the rows above it to fill the empty space. I repeat these 3 steps in sequence for each pixel, meaning the function code is looped 4 times, once for each of the 4 pixels in the active block. To simplify the function, I realized you can calculate the address of the leftmost pixel of the row that contains some pixel <i>p</i> by using this formula: pixel_addr - (pixel_addr % 64) - 64, where pixel_addr is the address of pixel <i>p</i>. After calculating the address of the leftmost pixel, I could iterate through the rest of the pixels in the row by adding 4 to the address multiple times (Rows are 16 pixels long, so 15 times, to be exact). To check whether a row should be cleared, I load a designated "boolean" register with 1, and then AND it with the rightmost bit of the hex color of every pixel in the particular row I am checking. If the boolean register is 1 after AND'ing the value from every pixel in the row, then that row should be cleared, because had there been even a single background pixel, the bit in the boolean register would've been flipped to 0. Then, clearing the row is just a matter of iterating through it and loading each pixel address with the background color.

Everything worked fine before I added the code to bring down the rows from above; rows would clear correctly in a significant variety of situations but just leave an empty row behind. Once I added the new code, I encountered a bug where completing multiple rows with an I block only causes the bottommost filled row to clear. After closer investigation, I realized this glitch occurs because the pixels in the I block are numbered 1-4 from bottom to top, and the rows are checked, cleared, and brought down starting with the row occupied by pixel 1, then the row occupied by pixel 2, and so on. This is to say that the bottommost filled row will clear, and the filled row above, which contains pixel 2, will be brought down onto the row that was once occupied by pixel 1. However, the program hasn't yet registered that pixel 2 has shifted, so when it checks the row occupied by pixel 2, it will check the row that pixel 2 was previously in, skipping over the filled row that has just been brought down.

To fix this, I added a segment of code in the loop that bring rows down to check whether a pixel it is copying down is in the active block, and to update the correct pixel address register accordingly. Essentially, the loop that brings rows down iterates through the rows above the last-cleared row, copying the pixels in each row to the one below it. If the row whose pixels it just copied was empty, then it breaks out of the loop. With the addition of the code segment I previously described, the loop works as intended in all cases.

<img src="/images/for-posts/4rowclear_1.png" width="210px"><img src="/images/for-posts/4rowclear_2.png" width="210px" style="margin-left:10px;">
<br><i>Quadruple row clear before/after.</i>

<span id="3"><b>Ending the Game</b></span>

As for detecting when the game is over, I simply wrote a function to detect whether there are any pixels of the last-stopped block in the topmost row of the screen, and to end the game if there is.

Despite the accurate and conceptually simple reasoning behind the function, I still ran into a truly strange bug while testing it. For reasons I did not yet know, if the first block generated by the game was a Z block, it'd stop in the first row and end the game immediately. After stepping through the program while observing the memory addresses correlated to the bitmap display, I realized that, for some reason, the .data section I'd defined the colors of the blocks in were stored starting at the same base address as the bitmap display, even though the stored hex codes were not being rendered as colors on the display. The Z block just so happens to be shaped such that it gets snagged on one of the .data hex codes at the top of the screen (which the program detects as an obstruction), stopping there and triggering the game over function. After removing the .data section and replacing all the references to it in the program (e.g. replacing references to "blue" with the hex code I defined it as), the function worked just fine. It was pretty mysterious, but thankfully I was still able to diagnose and solve the issue without much trouble.

<span id="4"><b>Residual Quirks</b></span>

I figure I should be transparent about the quirks I noticed in my program while testing it, so think of this section as both a set of disclaimers and an outline for what I may improve about the program next. While the game is now <u>entirely functional and playable as a Tetris game</u>, some aspects, such as its error handling, definitely still need work.

First, here are the quirks related to checking the validity of block movements:

Though I added barriers on the left and right edges of the screen to prevent blocks from "wrapping around" (i.e., coming back in on the left if moved off the right of the screen and vice versa), blocks can still be <i>rotated</i> out of bounds. Fixing this is likely just a matter of checking which blocks in which positions will go out of bounds if rotated at either edge of the screen, and skipping the rotation if certain combinations of conditions are detected. Also, since I have not covered implementing barriers anywhere else, I'll mention here that the leftmost and rightmost columns of the screen can be detected by checking whether [pixel_addr % 64 == 0] or [(pixel_addr - 60) % 64 == 0] respectively.

I noticed this next problem by playing the game and intentionally trying to break it. Basically, blocks can be moved left or right into another block, because obstruction checks are only applied to blocks directly below the active block. So, I think I may have to either incorporate more cases into my obstruction detection function, or program a check directly into the game loop.

Speaking of varying cases in the obstruction detection function, though, I found that adding a special case to handle obstructions when the down button is pressed creates a new quirk. Sometimes, spamming the down button as the block approaches an obstruction gets it stuck 1 pixel above it. The down button moves blocks down 2 pixels rather than the default of 1, which is why I programmed the special case to check the pixels 2 below the active block (as opposed to 1 below). This prevents blocks from cutting into other blocks if the player spams the down button, but evidently doesn't come without faults. As of now, I'm not sure how to eliminate this issue without just disabling the down button entirely.

Next, here are some other, miscellaneous quirks:

While testing the game, I've found that the MARS random syscall is conspicuously patterned; often, only the same few blocks are repetitively drawn. So, I think I may have to look into creating an assembly language adaptation of rand() like I mentioned in a previous post. 

Another noticeable issue is that blocks flicker while free-falling, presumably because they are being quickly erased and redrawn. I attemped to fix this a few times, but found that my solutions only created more problems. In a nutshell, I tried various methods of selectively redrawing pixels to reduce flickering, but found that the pixels needed to be redrawn in an order specific to block type and position, similar to the selective redrawing I did to create block rotations. (This selective redrawing is why blocks flicker a <i>lot</i> less when being rotated, so I guess my hard work on that was warranted.) Otherwise, blocks would either continue to flicker or appear very misshapen. I then realized that, even if I do manage to fix the flickering, there's still some that can't be helped, such as when the I block is horizontal; the block inevitably has to be erased and redrawn entirely. Considering that I started this project with the simulator window crashing my computer after a keypress or two, I figure I should be happy with what I've managed to make in it, even if it's got oddities like this. I've decided to chalk this particular quirk up to the small screen resolution, and the simulator's likely suboptimal Java implementation of my assembly code. Plus, the flickering gives it a sort of retro vibe anyway...

Finally, code-wise, I know I can condense the obstruction detection function by looping similar code instead of writing it out multiple times, but my register usage in the function seems to make it somewhat awkward to restructure. It's not a strictly necessary fix, but I might try to get to it anyway.

<span id="5"><b>Project Trivia</b></span>

As a fun way to conclude this post, here are a few inconsequential but interesting facts about the process of this project <img src="/images/emotes/nukoHappyDance.gif" class="emote">

Not long after I started working on this project, the MARS syscall reference page went down, so I started using the Internet Archive copy of it instead. Then, shortly after that, the Internet Archive got hacked... I don't exactly remember the timeline, but I think all of these things may have happened within a week.

I've made a habit of listening to <a href="https://youtu.be/dkvoUi4ivbw" target="_blank">S&M Mashup by Ayesha Erotica</a> on loop while working on this project. I think I found the song around the time I was planning and coding the block rotation function, and the rest was history. When I say I don't think I'd have gotten this far on the project without Ayesha Erotica, I'm being dead serious LOL.

Also, I actually found myself frequently referencing my previous write-ups when picking up this project again. They turned out to be pretty useful!