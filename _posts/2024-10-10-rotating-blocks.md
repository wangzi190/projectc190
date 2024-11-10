---
layout: posts
title: Spotting patterns and rotating blocks
categories: projectLog
---
<style>
    .emote {
        vertical-align: bottom;
        width: 18px;
    }
</style>
<i>It's dangerous to go alone!</i>

Before you read, here is <a href="https://github.com/wangzi190/mips_tetris/commit/3ebc842c2c48d6134f6f9968edc3c174c6d050be" target="_blank">my code at this point of the project</a> and <a href="/images/for-posts/tetronimoes.jpg" target="_blank">a picture of all the Tetris blocks with labels</a> for your reference.
<hr>
<b>Preliminary Draw Function</b>

Since I'd previously (see <a href="/projectlog/2024/09/22/falling-blocks.html">Falling blocks, movement keys, & MARS hackfixes</a>) only drawn the square Tetris piece to test basic program elements like animation and movement controls, before proceeding to enable rotating blocks I needed a function to select a block type at random and draw it at the top of the screen.

(Disclaimer: I say "function" to refer to the general concept of a segment of code that performs a specified task. I did not define some "functions" formally as MIPS functions if it felt less intuitive.)

To do this, I decided to use MARS syscall 42, which generates random integers within a specified range (more details <a href="https://web.archive.org/web/20240913140616/https://courses.missouristate.edu/kenvollmar/mars/help/syscallhelp.html" target="_blank">here</a>), so I could quickly write the code and proceed with the more interesting parts of the program. With that said, I did come across the code for the rand() function in the C standard library while considering different approaches, so I may write an assembly language adaptation of the function to replace the syscall later if I feel like it. (That'll have to come after a lot more work on expanding and optimizing the program, though...)

<b>The Planning I'd Done</b>

After writing out the aforementioned function, I was ready to start banging out the code for rotating the blocks thanks to the planning I had done even before that. I spent a considerable amount of time drawing out each block (excluding the square block, which "doesn't rotate") in each of its 4 possible positions. My strategy was to map each pixel to a register (e.g. $s1 holds the address of pixel 1, $s2 holds the address of pixel 2, etc.) and create the illusion of "rotating" the blocks by calculating the offset of each pixel, rewriting the necessary register values, and then writing to the memory at each adjusted register value.

To reduce the complications of this task, I marked out the pixels whose positions are the same irrespective of a block's rotational position, so I know what not to worry about. Next, I was able to spot some patterns in the offsets for the pixels in blocks which were mirror images of each other, that is, the S and Z blocks and the L and J blocks. The offsets for the moving pixels of the S and Z blocks can be calculated with the exact same formula, which means I can simply write one function and apply it to either block. As for L and J, the offsets for two of the three moving pixels can be calculated with the same formula, but I couldn't find a way to cleverly reconcile the last one, so frankly the code for that looks a bit awkward.

Here's what I sketched to figure this out, if you're so inclined.
<br><img src="/images/for-posts/mirrored_blocks.jpg" width="475px">
<!--<br>EXPLAIN HOW YOU CALCULATED THE OFFSETS-->
<br><br>And, just for the record, here are my diagrams of the remaining two rotatable blocks. The I block looks relatively uninteresting, and the T block I pretty much had to hardcode, so I almost didn't include these until I realized that they'd probably still help someone understand my code better.
<br><img src="/images/for-posts/I_and_T.jpg" width="475px">

<b>The Overarching Logic</b>

As of this part of the project, the program works by iterating through a "game loop" indefinitely. The game loop executes 3 important steps in order until the user specifies to quit the game. First, it checks for keyboard input. If the user pressed a movement key ('a' for left, 'd' for right, or 's' for down), the default offset added to the address of each pixel in the block to move it down 1 row is adjusted to create the illusion of either a left, right, or downward motion in addition to the default downward movement (The actual movement, i.e. the writes to the memory addresses correlating to the bitmap display, does not occur until the next step in the game loop, however). If the user pressed the "rotate" key ('w' for, well, rotate), the program jumps to a segment of code that rotates the block depending on its type and its rotational position as of the jump (It will, of course, return to the game loop segment afterward). After detecting the key press, as long as the user did not specify to quit by pressing 'q', the program will erase the block pixel by pixel by filling it with the background color, and then redraw it 1 row lower on the screen (In the case of a movement key press, it will also create a left, right, or additional downward motion simultaneously by calculating the block pixels' new positions using the adjusted offset). Finally, the game loop will buffer slightly before calling itself again. Hopefully that explanation was not too long-winded; I just didn't want anything to be confusing or ambiguous <img src="/images/emotes/nukoEmbarrassed5.gif" class="emote">

Since my last update on this project, I worked mostly on the rotation function, only adjusting other parts of the program to make it more compatible with the new code segment.

<b>The Coding Process, in a Nutshell</b>

The rotation function was not technically difficult to code. I'd say just planning it out was most of the work, and the rest was correcting silly mistakes I made while implementing the plans. The fundamental logic is not so difficult either—if the block is [insert block type] and in [insert position #], apply this offset to this pixel. It's only a little gnarly when you have 6 rotatable blocks (we exclude the square block, which "doesn't rotate"), 4 rotational positions per block, and 4 pixels per block, making for a total of 96 different conditions if hardcoded. As such, I tried to reuse as much code as possible; for example, I have a function that calculates (-1)^n (where n is the integer associated with the block's rotational position) which I call to calculate the offsets of pixels in the S, Z, and I blocks. However, I'm not sure if doing this is necessarily efficient. My program also has a godless number of consecutive branch statements, but I'm not yet clear on what I can do about that.

<b>What Next?</b>

I think I had some awareness that the way I implemented some of the program logic was a little cursed, but that has since been very surely confirmed when I learnt about the details of pipelined MIPS implementations and realized that my program requires a horrific number of stalls. I figure I should probably reorder and rewrite some sections of my program to make it more efficient before moving on to the sensible next step of the project—writing a function to detect whether a block should stop falling. As for that function, though, I had the foresight to pick my background and block hex color values in a way that makes it fairly easy to make the distinction between background and block, so hopefully it will not be too difficult to program <img src="/images/emotes/nukoNerd2.gif" class="emote">