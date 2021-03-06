Writing about documentation
===========================

_Is nothing at all like dancing about architecture._

I'm behind on writing the readings for CSC 151.  That should not be a
surprise to anyone. But I'm even more behind than I normally am.  It
doesn't help that [I made some choices recently](bad-choices-2017-09-06)
that interfered with writing.  

We're talking about documentation in class on Friday and so I really
had to write the reading on documentation tonight.  Fortunately,
I've written similar readings before, so I had [something to start
with](http://www.cs.grinnell.edu/~rebelsky/Courses/CSC151/2017S/readings/documentation-rgb-reading.html).

At first, it seemed to be fairly smooth work.  I update my text when
I can, so I made some simple edits.  Fortunately, the old reading
generally seemed good.  Then I hit the first example in the reading.
Examples are core to the readings, since they show students what's really
happening.  Our first example was `square`.  I'd recently re-written my
documentation for `square`, so I thought it was useful to compare the two.
And I quickly realized that I document a bit differently these days.
The example did not have much on types, other than to over-constrain
the input to an integer.  I want my students to think about types,
so I had to add that.  I also found some other things to think about,
particularly with regards to how to phrase postconditions.

That wasn't so bad.  And I think my updates made the reading better.

Then I hit the second example.  I had known that the second example
had to go, since it focused on RGB colors, a topic we are not covering
this semester [1].  That meant that I had to come up with another
example.  I thought about dropping the second example altogether.  But
there was this paragraph at the end of the second example.

> It took a bit of effort to get the documentation right, or close enough
to right. We hope that it was useful effort. First, it required us
to carefully think through what we wanted the procedure to do and to
differentiate aspects of our current implementation from the more general
goals. Second, it required us to think about special cases. We'll find
that many of the procedures we write work fine on many cases, but not
on the more extreme cases, which we will often call "edge cases" or
"corner cases". In this instance, the procedure behaved differently
on large components. Finally, we had to balance the needs of the client
programmer and the implementing programmer. You'll find that a lot of
procedure design requires such a balancing act.

Yeah, the documentation for `square` didn't do that.  So I had to come
up with an example and guide them from what we might "logically" start
with to what would be more sensible to say.  I'm not sure that I came up
with an excellent example, but I think it's passable: scaling grades [2].

It took much longer than I'd planned to think through the example, to
write up what I'd thought through, to realize from writing that I'd made
some bad choices in thinking about the problem, to revise the example,
and then to write some more.  But [I finished](http://www.cs.grinnell.edu/~rebelsky/Courses/CSC151/2017F/00/readings/documentation.html).  I'm not ashamed
of it.  

However, given the time on writing that reading, I don't really have
time to muse much.  Hence, I'm musing on writing that reading [3].

What's next?  Unit testing!  Damn.  Another case for a new example.  I'll
need to come up with a good one, so I'm going to sleep on it.

---

[1] Or at least not in the same way.

[2] Oooh!  Exciting.  At least it's about transforming data, which is a
key topic in the course.

[3] I also finished two short musings that I had started previously [4].

[4] I should learn to make better choices.

---

*Version 1.0 of 2017-09-06.*
