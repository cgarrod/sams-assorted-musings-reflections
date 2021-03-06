Learning to satisfice
=====================

The other day in class, I was walking students through the design
of a recursive procedure that they had done on a recent homework.
We spend some time developing a correct procedure The procedure had
some repeated code, which I tell students to avoid [1].  So we rewrote
the procedure to avoid the repetition.  Then I observed that it was
calling another procedure on every recursive call and that, since that
procedure was expensive, we should consider how to avoid those repeated
procedure calls.  Along the way, students had suggestions and questions
on other improvements.

When we were done, I asked the students what they'd learned from the 
exercise.  One of them said something like "It seems like you can always
make another change to improve a procedure.  Know when to stop."  So
I wrote the following on the eboard [3].

> Learn to satisfice.

My class mentors looked at each other and made faces that implied "Sam
is making up another word."  But I wasn't.  "Satisfice" is a real word.
According to [Wikipedia](https://en.wikipedia.org/wiki/Satisficing) [5],

> Satisficing is a decision-making strategy or cognitive heuristic that entails searching through the available alternatives until an acceptability threshold is met.

Yup.  Stop when your procedure is above an appropriate acceptability threshold.

Alternately, stop looking for topics to muse about when you find an 
acceptable one.  "Satisfice" satisfices.

---

[1] Upper-level students know to keep their code DRY [2].  The introductory
students are more likely to repeat code, and I've been working on helping
them understand that repeated code incurs not only computational costs,
but maintenance costs.

[2] Don't repeat yourself.

[3] The eboard is my electronic whiteboard.  Basically, I open a terminal
window at the start of class, start vi, and type on the computer rather
than write on the board.  That lets me keep a nice record of each class
that students can refer to later [4].

[4] There are some questions about whether this approach is best, since it
leads some students to stop taking notes.  There's evidence that note taking
helps you learn.  Good students still take notes and rely on the eboards for
additional detail.

[5] Wikipedia may have taken the definition from  

> Colman, Andrew (2006). *A Dictionary of Psychology*. New York: Oxford
University Press. p. 670. ISBN 0-19-861035-1.

---

*Version 1.0 of 2017-11-09.*

