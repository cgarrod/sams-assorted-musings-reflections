"Sam, you're losing your audience!"
===================================

A day or two ago, a friend and I were chatting.  As is often the case
these days, my essays came up.  In particular, my friend said,

> Sam, you're losing your audience.  No one understands this technical
stuff.  But that's okay, you write for yourself and not your audience.

Now, I will admit that I had hoped that my audience, such as it is, would
deal with this month in two ways: Most would skim the daily essays to see
if there was anything comprehensible and plan to come back to the essays
when I was done with this detour.  I assumed that the same would happen
when I wrote the profiles of Grinnellians, and I seem to have been be
right; the non-Grinnellian friends who comment on my essays from time
to time continue to comment on my essays from time to time.

But this friend may be right.  Everyone could understand the profiles,
even if they didn't know the people, or the full context.  These new
essays devolve [1] into technical details fairly quickly, potentially
making them a complete slog for non-technical readers [2].  And, as 
someone who is governed by habits to do (or not to do) many things, I
understand that once folks stop reading these essays daily, they may 
not come back to them when the month ends.  

I guess I could assess what my readership is like, and consider what
percentage the technical essays will affect.  After all, I seem to have
a lot of technical readers [3]: current students, alumni, colleagues [4]
(including two in the English department who probably qualify as technical
readers), and perhaps a few hangers-on.  But I also believe that many of
the readers who communicate with me regularly are not technical readers.
And, in spite of what I've said, I do care about having some readers,
particularly the ones who send me comments.

I also wonder whether my technical readers are actually reading these
essays.  I assume (well, maybe hoped) that I'd get a few comments on 
whether people think the same way, or whether I've missed something, or
whether I'm going over the top, or ....  But the only comment I've
gotten so far was about an adult kitten.

So, what to do, what to do?

It's important for me to write the technical essays.  I really would like
to change the model of the course so that we have more in-class time to
discuss solutions [5].  While I think there's some significant benefit
to the back-and-forth that happens, say, when we develop [a simple C
project](cnix-simple-c-project), I think there's more benefit to looking
at how students approached problems when they had more time to think 
about them.  I also hope that I can write in a way that allows engaged
readers to experience some of that back and forth.  

More importantly, my muse is incredibly jazzed about this project, perhaps
too much so.  Every day she suggests a wide variety of things to write
about.  Here's a typical experience.

> Oooh!  Now you can follow up with an introductory essay about Make.
Wait, you should discuss procedure signatures and why they are important.
Don't forget the story of showing a student `valgrind` [6] and fixing
in five minutes a problem that they'd been debugging for two days.  Oh:
you've been been using `#ifdef` in your headers.  It's probably time
to write more about the C preprocessor.  How many essays will that be?
One for constants and macros, one for control structures, one for some
of the complexities of macros.  And once you introduce macros, you can
show them your typical debugging library and the evil hack for simulating
generics!  Of course, most C programmers wouldn't use that hack; they
just deal with generics with pointers.  And then you could come back
to how to store an integer or a pointer in the same space.  But wait!
This is supposed to be about C and Unix, and those are all C topics.  You
should write about Unix, too.  Write about regular expressions, and the
basics of `sed`, and `tr` (and why you want `tr` if you have `sed`),
and editor wars, and that great command you showed a student last semester
to edit only the files that contain a particular string, and, damn, what
else do you no normally teach in the course?

I think my muse is so enthusiastic about this project that it will be
difficult to get her to inspire me to write about other things, perhaps
so much so that I might still write about these things were the trustees
to discontinue need-blind admission [7].  Writing the technical essays
also seems to take more time than writing the non-technical essays.  In
part, that's because I have to write code in addition to writing text.
In part, it's because the technical essays seem to be a little longer
[8].  The essays also require me to think in multiple ways, both about my
writing and about programming / software development.  It also looks like
I may want to switch how I generate the Web pages for these essays, from
using Markdown plus a few custom scripts to using Jekyll.  All of that
doesn't leave a lot of time or energy for the non-technical essays.

I know, I'll see if I can jump-start my muse to think about non-technical
topics.  Maybe then she'll help me write short, non-technical "bonus" essays
at least every other day.  Let's try.

> Think of all the other things that I could write about.  Don't you
hate the word "relatable"?  When did that come into common parlance,
and does anyone above the age of, say, 25 use it? [9]  Remember that
essay on Eskimo words for snow?  You could encourage me to write an
essay on how poor my essay are in comparison to that one?  It's almost
the start of the semester.  You could have me write about the myths
of winter break and the realities, and mention Janet G's comment on
planning fallacies.  _Chronicle_ had a recent article on implicit bias;
there are things I could write about that.  [Erik Simpson](erik-simpson),
[Janet Davis](janet-davis), and [Rachel Schnepper](rachel-schnepper) all
keep mentioning "inbox zero".  I bet there's a series of essays possible
associated with that topic, starting with "inbox 220K" [10].  Two days
ago, I organized my collection of SF hardbacks and identified two boxes
worth to get rid of [11].  Doesn't that deserve an essay?

Hmmm ... that may have worked.  Stay tuned.  I'll try to get a non-technical
essay out at least every-other day.  But, at least given the list I came
up with, it looks like very few of them will be about higher ed, at least
right now.  We'll see.

---

[1] Or maybe evolve?

[2] I hope that they don't end up being a slog for my technical readers,
particularly the students upon whom I will impose those essays in less
than a month.

[3] By "technical readers", I mean readers who can also understand the
technical content of my computer science essays.  I realize that there
are other possible interpretations of that term, but you should ignore them.

[4] In the list of colleagues who are technical readers, I include two 
members of the English department.

[5] I suppose I could have added another hour to the course each week,
which, now that I think about it, students suggested two years ago,
when I last offered the class.  But it's too late for that now, and it
does increase the potential workload for students.

[6] Rhymes with "sinned".

[7] Okay, that's not quite true.  I think if we got rid of need-blind
admissions, I'd end up writing an essay.  But I'm not sure what it would
be.

[8] I'm too lazy to do firm statistics yet, but it looks like the
technical essays are averaging about 1300 words, and I believe
the traditional essays average about 1000 words.  If we ignore the
introductory essays, which were all about 1000 words, it looks like the
technical essays are averaging about 1500 words.

[9] My muse just noted that others have probably written similar
essays, so what's the point?

[10] I did have 220K messages in my inbox as of a few weeks ago.  I'm
down to about 200K, dating back to 2008.  70K of them are unread.

[11] I plan to offer those to my students, and then bring the rest to
Friends of Drake Library, which I will always think of as Friends of
New Drake Library, or "FONDL" [12].

[12] We knew that the Friends of Stewart Library were old because we
were all FOSLs.

---

*Version 1.0.1 of 2017-01-07.*
