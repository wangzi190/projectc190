---
layout: posts
title: Nand2Tetris - Inquiry / Simplifying Circuits
categories: projectLog
caturl: "/blog/project-log.html"
---
Here's a follow-up of the "Simplifying Circuits" inquiry from my <a href="/projectjournal/2023/03/28/boolean-functions.html" target="_blank"><u>N</u>and2Tetris Project 01</a> post.
<br><br>
So, I was a little confused by the posts I'd read, which implied that a truth table can directly make clear the simplest way to notate a Boolean function. After searching around online, I found that a Karnaugh map, or <i>k-map</i>, is probably the "truth table" people were referring to. The k-map technique involves drawing out 2D representations of Boolean functions so that logical adjacencies become visually evident. Here is how I simplified DMux using the technique.
<br><br><a href="/images/for-posts/simplify_dmux.png" target="_blank"><img src="/images/for-posts/simplify_dmux.png" width="500px"></a>
<br><br>(I'd read in a forum that Xor is most minimally built in 4 Nand gates, which is why I knew to notate that Xor == 4 Nands before having actually simplified it myself.)
<br><br>As for the other two chips that needed simplification, Xor and Mux, I found that an algebraic approach was more suitable than a graphical one. After reading the Boolean algebra chapter of <a href="https://www.ic.unicamp.br/~pannain/mc404/aulas/pdfs/Art%20Of%20Intel%20x86%20Assembly.pdf" target="_blank"><i><u>T</u>he Art of Assembly Language</i></a>, and playing around with the canonical forms of Xor and Mux, I figured out how to reduce the chips to a minimal number of Nand gates.
<br><br><a href="/images/for-posts/xor_breakdown.png" target="_blank"><img src="/images/for-posts/xor_breakdown.png" height="260px"></a> <a href="/images/for-posts/mux_breakdown.png" target="_blank"><img src="/images/for-posts/mux_breakdown.png" height="260px"></a>
<br><br>Now, I have all the fundamental chips from Project 01 (And, Dmux, Mux, Not, Or, and Xor) built using the simplest possible configurations of Nand gates. If you like, you can view my code on <a href="https://github.com/wangzi190/nand2tetris/tree/master/01" target="_blank"><u>G</u>ithub</a>.