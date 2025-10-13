---
layout: project-post
title: "Programming and Pro Gaming: Playing SIC-1"
categories: projects
permalink: /projects/esolangs/playing-sic-1
---
<style>
    .comments {
        color: #999999;
    }
</style>
Lately I've been obsessed with SIC-1, an open-ended programming game where you learn to write programs in the esoteric assembly language Subleq, which only has a single instruction, to perform various tasks. The Subleq language's single instruction is, as you might be able to guess, <i>subleq</i>—"subtract and branch if less than or equal to 0".
<br><img src="/images/for-posts/how_to_subleq.png" width="480px">
<br>I really like the gamified format because it's very engaging, and its clearly designated challenges eliminate the hesitance or indecision I'd usually have when trying to learn an esoteric language with no particular aim. I'm able to gain a meaningful understanding of the esolang and have lots of fun all the while. The vibrant interface, soothing soundtrack, and goofy corporate lore (in the game, you work for a company called "SIC Systems" and get "promoted" as you complete more programs) are just the cherries on top. And even better yet, the game is completely free! I'd highly, highly recommend it to any assembly language or esolang enthusiast.

As of writing this, I'm about halfway through the game, and so far it's really not as scary as it might sound. I've refused to use a single hint, and the difficulty level has been very reasonable. (Here's to hoping I can do a full, no-hints run...) Thinking in terms of Subleq becomes increasingly intuitive as you progress. But enough shilling the game—what non-trivial information have I learned about Subleq so far?
<hr>
Initially, I wasn't sure how to discern between negative numbers and 0 for the "Sign Function" challenge, considering that the subleq instruction branches if less than <i>or equal to</i> 0. Then, I realized I could simply do a process of elimination instruction by instruction. To take input, I inevitably need to write:

<b>subleq @tmp, @IN</b>    <span class="comments">; @tmp = -input_value</span>
<br><span class="comments">; (@tmp was initialized to 0)</span>

So, what if I make it branch?

<b>subleq @tmp, @IN, @is_ge_0</b>    <span class="comments">; @tmp = -input_value</span>

By definition, this instruction branches if @tmp = -input_value is less than or equal to 0. The only way for the negated input value to be less than or equal to 0 is if the original input value is greater than or equal to 0. Thus, if the instruction branches, the input value was >= 0, which means I can write code to handle negative numbers immediately after.

<b>subleq @tmp, @IN, @is_ge_0</b>    <span class="comments">; @tmp = -input_value</span>
<br><span class="comments">; handle negative numbers</span>

After that, discerning between positive numbers and 0 is pretty straightforward:

<b>@is_ge_0:</b>
<br><b>subleq @tmp2, @tmp, @is_eq_0</b>      <span class="comments">; @tmp2 = -(-input_value)
<br>; handle positive numbers
<br>; (@tmp2 was initialized to 0)</span>

<b>@is_eq_0:</b>
<br><span class="comments">; handle zeroes</span>

Ultimately, it is a lot cleaner to discern between positive numbers and 0, and the developer of SIC-1 is well aware of that. Subsequent challenges involve reading in sequences of positive numbers terminated with 0s, so you can simply write:

<b>subleq @zero, @negated_num, @terminate</b>
<hr>
Probably the most interesting aspect of Subleq (that I know of so far) is its capacity for self-modification, which opens up new avenues for all sorts of wacky solves. Self-modifying code is also necessary to create essential functionalities.

Each operand in a Subleq instruction represents a memory address, and instructions only use and change the values <i>at</i> those memory addresses. Unlike conventional assembly languages, there exist no instructions that enable loading and saving bytes from memory using the value at a certain memory address as a pointer to a different address. This necessitates program self-modification; otherwise, there would be no pointer functionality to speak of, and with it, none of the nifty functionalities that pointers enable.

<span class="comments">; a simple demonstration of pointers in subleq<br>; the 0 in the 1st instruction will be modified by the 2nd</span>
<br><b>@loop:</b>
<br><b>subleq @OUT, 0</b>
<br><span class="comments">; ^ outputs the byte at mem[0] negated. 0 is the "pointer value"</span>
<br><b>subleq @loop+1, @n_one</b>   <span class="comments">; pointer++</span>
<br><b>subleq @zero, @zero, @loop</b>   <span class="comments">; unconditional branch to @loop</span>
<br><span class="comments">; on the next iteration of the loop, the program will output the byte at mem[1] negated, then mem[2] negated, and so on</span>

As seen in the above example, program self-modification enables "pointers" in the sense that it enables you to rewrite the operands of any instruction by incrementing or decrementing it by any value.

Why is this helpful? Consider the "Reverse Sequence" challenge: How would you store a sequence <i>of unspecified length</i> in memory, and then output the sequence in reverse, if you couldn't access memory using pointers?
<br><img src="/images/for-posts/rev_seq.png">
<br>Without the pointer functionality enabled by program self-modification, you'd essentially be forced to hard-code memory addresses, which would obviously not be reasonable. A hard-coded Subleq program cannot reasonably anticipate sequences of varying lengths, and keep track of where each sequence starts and ends in memory.
<hr>
Anyways, I've been having lots of fun with SIC-1 and Subleq so far, and I'm considering making a certain spin-off project after I beat the game. Not only that, but I even have other esolang programming games on my wishlist...