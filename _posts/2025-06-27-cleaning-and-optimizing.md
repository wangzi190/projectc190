---
layout: project-post
title: Cleaning and optimizing the program
categories: projects
---
<!--Initial post date: 06.26.25-->
It's been some time since I've completed the steps of the project I'm about to describe, so, for the record, I am summarizing them here more for the purpose of re-familiarizing myself with my progress rather than documenting something I've recently completed.

In sum, I improved the program code by 1) reducing the number of stalls in the program by spacing branch instructions apart from the instructions setting branch comparison values, 2) formally defining every modular segment of code as a MIPS function, and 3) significantly cutting down the code required for the L and J block rotations by modifying the function to jump to segments of code I noticed I could reuse. Overall, the program is much cleaner, and shorter by a whopping ~55 lines. That's about a 13% reduction, and the program still does just what it did before! I'll explain each change in more detail, and then provide a rough outline of what's to come.

<b>Reducing Stalls</b>

In my first version of the program, I wrote large chunks of conditional logic as a decrement statement followed by a branch statement, followed by another decrement statement followed by a branch statement, and so on (The decrements change a value I'm repeatedly comparing in the branch statements).
This <i>felt</i> pretty cursed while I was writing it, but solidified as truly and objectively cursed once I learned about MIPS pipelining, otherwise known as the classic RISC pipeline.

Basically, MIPS processors carry out each assembly instruction in a series of up to five stages. These stages are called the instruction fetch, instruction decode, execution, memory access, and write-back stages, abbreviated as IF, ID, EX, MEM, and WB, respectively. Each stage is localized to a certain part of the processor, meaning the ID stage of one instruction can occur alongside the IF stage of the next. The pipelined implementation takes advantage of this property. One stage of each instruction occurs in every clock cycle, and instructions are continuously fed into the pipeline so that all parts of the processor are occupied at once. People often do laundry in the same fashion—after moving one load into the dryer, they put a new load into the washer to ensure that both machines are running at once.

The issue with the conditional logic I previously wrote is that it updates a register with a new comparison value right before a branch statement, which then requires the new value. However, the new value isn't written to the register until the final, WB (write-back) stage of the preceding instruction, which means that when the branch instruction accesses the register, it will read the old value in that register, not the new one.

Pipelined MIPS processors typically resolve this issue by stalling until the register is updated, but I didn't really want my program to stall six times in a row just to figure out what Tetris block it should display next. (I know the MARS simulator disregards this quirk of MIPS processors, but I'd like my program to, in theory, be able to run decently on the target hardware.) So, I rewrote every instance of decrement-branch-decrement-branch to a series of instructions assigning distinct values to distinct registers preceding a series of branch statements checking those registers. This way, there's enough buffer time between each register write and the branch statement that checks the written register.

<b>Defining Functions</b>

Defining segments of code as MIPS functions (i.e. calling them with jal, returning from them with jr, and pushing and popping register values on the stack to preserve them across function calls) didn't technically improve the functionality or length of the code much, so there's not as much to say about this part. I do think it helped with readability and made the code a <i>lot</i> easier to keep track of, though; if I had not done this already, I'd certainly have done it now. Put simply, this is another useful measure I took to make my code less cursed.

<b>Cutting Code</b>

This part is where it gets exciting! In my last MIPS Tetris post, I mentioned it was difficult to reuse code between the L and J block rotations due to one of the pixels having an inconsistent offset (Quote: "As for L and J, the offsets for two of the three moving pixels can be calculated with the same formula, but I couldn’t find a way to cleverly reconcile the last one [...]."). However, I didn't realize that an elegant solution was staring me in the face the whole time.
<br><img src="/images/for-posts/L_and_J_1.jpg" width="450px">
<br>After I drew this diagram to figure out how I should code the L and J rotations, I lamented that pixel #4's offset formula was "off by one": Whereas pixel #4 in the L-block had offsets +8, +128, -8 and -128 for the four rotational positions, pixel #4 in the J-block had offsets +128, -8, -128, and +8; as if you'd shifted each value over one position and wrapped it back.
<br><img src="/images/for-posts/L_and_J_2.jpg" width="455px">

Funnily enough, that's exactly the observation I needed to devise a better solution.

I rewrote the L and J rotation function to assume by default that the block it is rotating is an L-block. For each rotational position, I wrote a segment of code calculating the pixel offsets according to the L-block formula, even pixel #4, which differs between the L and J blocks. Before the pixel #4 calculation in each segment, however, I added a branch statement directing the program to the next rotational position's pixel #4 calculation if the block is a J-block. This creates the "shift one and wrap" effect needed to accurately calculate the position of pixel #4, regardless of whether the block is L or J. Thus, this adjustment makes the L and J rotation function clean and truly dual-purpose.

<b>Thoughts on Next Steps</b>

There are still three things my program doesn't yet have awareness of:
<br>1) When should blocks stop falling?
<br>2) When should a line clear occur?
<br>3) When is the game over? 

I think it makes sense for the game to check these conditions in this order; so, I'll next be working on detecting when blocks should stop like I mentioned in my previous post about this project. My brainstorming got more complicated than I thought it would, but I think I have an accurate idea that is also reasonable to implement. I'll leave it for next time to talk about.