Watching terms lose their meaning: Algorithm
============================================

For as long as I've been teaching computer science, I've been telling my
students that computer science is the study of algorithms and data [1].
I then suggest to my students that an algorithm is a clear, unambiguous
[2], and terminating [3] set of instructions.  When I hear the term
"algorithm", I think in terms of those kinds of instructions, things like
Quicksort, which puts a collection of values in order, or binary search,
which provides an efficient way to search collections once they've been
put in order.  Both of these algorithms, like most of the algorithms I
have my students study, can be proven correct.

Hence, I've been a bit surprised to see claims in the news media
about "biased algorithms" or the need for "algorithm accountability".
When I dig deeper, I see that all of these so-called "algorithms"
are things that I would not call algorithms.  Invariably, they are
a set of rules or processes, generated by another program, that are
used to make decisions.  I'd call such things "heuristics" rather than
"algorithms".  

I agree not only that they are biased, but that we need systems for
accountability.  Too many of these heuristics are serverely biased,
whether it's in how they classify different kinds of faces, what
recommendations they make for sentencing, what gender they assign to you,
and mother other things.  Why are these heuristics biased?  Because most
of them are generated by running a bunch of examples through a program
that generates the heuristic.  If you don't choose your examples well, the
heuristic works well for things like the examples, but not other things.
If you train something to find topics in my musings and then apply it to
real essays, you would not expect it to work well.  However, too many
developers at major corporations aren't good at choosing data sets,
and so they make stunningly bad conclusions about many groups of people.
We've seen this behavior particularly when people in a dominant group don't
think to include people from a less dominant group in their data set.
That sucks, and probably violates the ACM Code of Ethics [4].

But they are not algorithms.

Unfortunately, it appears that the folks in the AI community spent so much
time calling their work algorithms that "algorithm" is not the accepted
term for these heuristics.  In that case, "algorithmic bias" and
"algorithm accountability" are perfectly reasonable.

It bothers me that we've lost use of the term.  But we've clearly lost
use of the term, not only in the popular media, but even in the profession.
For example, there's an NSF-sponsored workshop on [Auditing 
Algorithms](http://auditingalgorithms.science).

So, how do we help the public distinguish between the things I think of
as algorithms, like Quicksort and binary search, and the heuristics that
are now called algorithms?  The former are provable and readable.  The
latter are approximate and obtuse.  We've lost use of a term that's been
in effect since the late 17th century, at least if Google is to be 
believed.  What should we call them instead?  "Real algorithm"?
Maybe "Adas", in honor of Augusta Ada King-Noel, Countess of Lovelace.
"WFAs", like WFFs in logic [5]?

Unfortunately, I don't have the power to make that change.  I'm not sure
I even know someone who has that power.  

I guess I'll just whine.

---

[1] Okay, there are variations.  Sometimes I say "ways of structuring data"
rather than just "data".  But it's close enough for folk, or at least for
a musing.

[2] It's not clear that there's a difference between clear and unambiguous,
but I like to use both terms.

[3] At least one demo suggests why I say "nonterminating".

[4] I need to reread that code, as well as its draft successor.

[5] WFA: "Well-formed algorithm".  WFF: "Well-formed formula".