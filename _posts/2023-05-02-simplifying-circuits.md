---
layout: posts
title: Simplifying Circuits
categories: projectJournal
caturl: "/blog/project-journal.html"
---
Here's a follow-up of an inquiry from my <a href="/projectjournal/2023/03/28/boolean-functions.html" target="_blank"><u>P</u>roject 01 post</a> (It's listed under "Topics for further investigation.").
<br><br>As for the other two chips that needed simplification, Xor and Mux, I found that an algebraic approach was more suitable than a graphical one. After reading the Boolean algebra chapter of <a href="https://www.ic.unicamp.br/~pannain/mc404/aulas/pdfs/Art%20Of%20Intel%20x86%20Assembly.pdf" target="_blank"><i><u>T</u>he Art of Assembly Language</i></a>, and playing around with the canonical forms of Xor and Mux, I figured out how to reduce the chips to a minimal number of Nand gates.
<br><br><img src="/images/for-posts/xor_breakdown.png" height="260px"> <img src="/images/for-posts/mux_breakdown.png" height="260px">
<br><br>So, I was a little confused by the posts I'd read, which implied that a truth table can directly make clear the simplest way to notate a Boolean function. After searching around online, I found that a Karnaugh map, or <i>k-map</i>, is probably the "truth table" people were referring to. The k-map technique involves drawing out 2D representations of Boolean functions so that logical adjacencies become visually evident. Here is how I simplified DMux using the technique.
<br><br><a href="/images/for-posts/simplify_dmux.png" target="_blank"><img src="/images/for-posts/simplify_dmux.png" width="500px"></a>
<br><br>Because I have both built its compenents as minimally as possible, and used the k-map technique to further simplify it, this seems to be the most minimal way to build DMux.