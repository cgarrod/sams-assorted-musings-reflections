A workshop on Universal Design for Learning
===========================================

*Earlier this week, I attended a Grinnell College workshop on Universal
Design for Learning [1], led by the legendary Angie Story and the almost
as legendary Autumn Wilke [2].  We are supposed to write short reports on
any workshop we attend.  Today's musing is that short report, extended
by my typical set of endnotes.*

---

On Monday, Tuesday, and Wednesday, June 5-7, 2017, I attended the summer
workshop on Universal Design for Learning.  It was an interesting and
useful workshop.  As is the case with every summer workshop I attend,
I found it useful to talk to other Grinnell faculty and staff about
issues of teaching and learning.  I learn something new every time I
talk with colleagues.

Accessibility is an issue I care deeply about; I've tried to be thoughtful
about accessibility issues in my teaching.  Some UDL issues have come up
naturally in my teaching, from the "eboards" I use for live notes during
class and that are available for students to peruse afterward to the
"There's more to life than CS" clause on my exams.  As we noted during
the workshop, a lot of UDL is simply good teaching.

From my colleagues, I learned about the value and ease of making short
audio lessons using Smart Pens, about using giving students freedom
through different forms of assessment for the same material, about
specs grading, and about a variety of other techniques.  At the same 
time, I found great value in the broader discussion of "How should I
approach this issue?" as we talked about everything from the benefits
of having a limited time for work to how we might help students better
read complex material.

This summer, I am redeveloping our introductory course.  As part of that
redevelopment, I am thinking about ways to make that course accessible
to the visually impaired.  The opportunity to play with screen readers
and think about related issues was therefore particularly valuable.
I learned, for example, that the screen reader just reads "A-" as "A",
which makes descriptions of grade scales a bit confusing [4].  Even though
I do not currently anticipate having a visually impaired student in my
class, I look forward to follow-up conversations with Angie and Autumn on
these kinds of accessibility issues because it's the right thing to do.

I already use most of the techniques for creating accessible documents
that we discussed, such as clear headings in both Word and Web and alt
text for Web images.  Still, it was very useful to hear the steps that
Angie goes through to get material in appropriate electronic (or physical)
format for students and the things she has learned along the way.  I
particularly appreciated the chance to observe that the right alt text
depends on the what you expect the reader to get out of the image.  It
turns out that a picture isn't always worth a thousand words!

Finally, as an outcome of the workshop, Elaine Marzluff and I have started
talking a bit about symbolic computation packages and accessibility.
It looks like the system that I would recommend [7] may need some
tinkering and development [8], and I'm finding myself inclined to see
if I can do some of that development.

---

[1] Universal Design for Learning (and a few variants thereof) is a
principle by which you design classes so that they are accessible to
students of a variety of abilities and disabilities so that they need
not ask for accommodations.  It falls under the "a rising tide lifts
all ships" concept; what you do to help one class of students often helps
many students.  For example, captioning videos serves not only d/Deaf
people, but also international students and even people who may have to
watch a video in a situation in which sound is not an option.  In my own
case, I record my notes during class (okay, I type my notes during class),
which serves not only those who need a note taker, but also those who may
not hear everything I say and any student who wants a record of class.

[2] Ang has been at Grinnell long enough to know where way too many
skeletons are buried [3].  Autumn has not been here nearly that long.

[3] Not literally.  I bet she has no idea where the Walrus is.

[4] I've tried "`A-<span class="sr-only">minus</span>`" [5,6] and
"`<span aria-label="A minus">A-</span>`".  Neither seems to work
quite right.  Any suggestions?

[5] `class="sr-only"` is a Bootstrap css class for "Screen Reader
only".  It does not mean "SamR only".

[6] The Hemingway Editor seems to think that `sr-only` is an adverb.
I love discovering interesting aspects of how programs are designed.

[7] [Jupyter](http://jupyter.org/), [Python](https://www.python.org/),
[SymPy](http://www.sympy.org/en/index.html), and
[jupyter-a11y](http://jameslmartin.github.io/jupyter-a11y/).

[8] It looks like jupyter-a11y is not in active development.  It also
strikes me that WAI-ARIA may be a better approach.

---

*Version 1.0 of 2017-06-08.*
