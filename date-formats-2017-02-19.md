Date and time formats
=====================

I've been grading take-home exams this weekend.  On my take-home exams,
I ask students to log the time they spend on each problem [1].  I've
been interested to see the wide variance in how they represent dates
and times on their exams, particularly since times and dates are among
the things about which I tend to be overly precise [2].

I am amazed at the number of people who don't seem to realize
that a date like 4/11 is ambiguous [3].  In the US, it likely
represents April 11.  But in much of the rest of the world, it likely
represents November 4.  These days, I try to either spell out the
month ("April 4, 2017" or "4 April 2017" [4]) or to follow ISO [5]
[standard 8601](http://www.iso.org/iso/home/standards/iso8601.htm)
[6], which says that I should use YYYY-MM-DD format.  I will admit
that too often, I've used some variant thereof, such as YYYYMMDD, but
[XKCD](https://xkcd.com/1179/) reminded me that I should be consistent
with standards.  One key advantage to ISO 8601 over most of the others,
particularly the ambiguous ones, is that it sorts nicely.  If you use
YYYY-MM-DD as a suffix to files that you have in different versions,
you can not only tell immediately which is which, your filesystem
automatically puts them in order.  If you use something more like an
American system, they aren't necessarily in order.  When looking at your
filesystem, wouldn't you rather see

    essay-2014-03-11.txt
    essay-2015-04-23.txt
    essay-2016-02-12.txt
    essay-2016-02-13.txt
    essay-2017-01-01.txt
    essay-2017-02-11.txt
    essay-2017-02-12.txt
    essay-2017-02-13.txt

than

    essay-01-01-2017.txt
    essay-02-11-2017.txt
    essay-02-12-2016.txt
    essay-02-12-2017.txt
    essay-02-13-2016.txt
    essay-02-13-2017.txt
    essay-03-11-2014.txt
    essay-04-23-2015.txt

or, worse yet

    essay-apr-23-2015.txt
    essay-feb-11-2017.txt
    essay-feb-12-2016.txt
    essay-feb-12-2017.txt
    essay-feb-13-2016.txt
    essay-feb-13-2017.txt
    essay-jan-01-2017.txt
    essay-mar-11-2014.txt

Yes, I realize that most file browsers let you sort by date.  But what
if you want files with similar titles together *and* sorted by date [7]?
YYYY-MM-DD naming makes life much easier.

What about times?  Although it did not crop up much on this examination,
 object to people using 12:00 pm and 12:00 am because they are both
ambiguous and meaningless.  Are you sure that 12:00 pm is noon?  Why?
Doesn't "pm" stand for "post meridiem", or "after noon"?  Doesn't "am"
mean "ante meridiem" mean "before noon".  Arguably, noon is neither
before nor after noon, and so should be neither am nor pm [8].  Even the
US National Institute of Standards tell us [not to use 12 a.m. and 12
p.m.](https://www.nist.gov/pml/time-and-frequency-division/times-day-faqs).
But people still seem to think that these terms have meaning [9].  I know
that I read somewhere that trains never arrive or depart at midnight or
noon so that they can always use unambiguous times.  I wish we all did
the same.  I suppose a twenty-four hour clock would also suffice, but,
well, that's a battle I'm not ready to fight [10].

That's about all I have to say about times and dates.  It's clear that
I've been spending enough time grading that my muse decided to take
a vacation [12,14,15].  Maybe I'll do better next time.

---

[1] Time logs are supposed to serve a number of purposes.  First, they
help students reflect on how they are spending their time, both while
they are spending their time ("What was I just doing?") and later, as
they try to figure out when they were and were not productive.  Second,
they help me understand how students are spending their time, so that
I can perhaps guide them better.  Third, they serve as a kind of evidence
for when students take the "There's more to life than CS" option on exams.
But the first two reasons are much more important.  

[2] I was going to write "are things I tend to be overly precise about",
but then I heard the stupid "Where's the library at?" joke in the back
of my mind.

[3] Maybe they are aware and just don't care.

[4] I prefer "4 April 2017" to "April 4, 2017" because the former makes
a natural progression from small units to large, while the latter has
medium, small, large.  Who came up with such a strange ordering?

[5] International Standards Organization.

[6] ISO standard 8601 gives more than the YYYY-MM-DD format.  I believe
the whole thing is about forty pages, but certain portions are clearly
generous in the use of whitespace.

[7] I suppose the obvious answer is "Use Unix and come up with the
appropriate shell command, such as `ls -lt | grep essay`."

[8] Potentially, that means that both 12:00 am and 12:00 pm should both
represent midnight.

[9] Some of those people are descriptivists, who are simply observing what
people do.  But too many are people who thoughtlessly use 12:00 a.m. and
12:00 p.m. without realizing that the terms are ambiguous.

[10] Given the number of Quixotic [11] battles I do fight, you may
be surprised that I'm not willing to add the twenty-four hour clock.
But I do have limits.

[11] I'm  game, why doesn't
[Merriam-Webster](https://www.merriam-webster.com/dictionary/quixotic)
capitalize Quixotic?  And will I be in trouble for doing so?

[12] I think at some point I said that one of the important lessons from
this series of essays was that it's important to try every day, even if
some what what you produce is not up to your own standards.  

[14] Some time after my muse returns, I may take another stab at this essay.
Fortunately, I've named it in such a way that you can tell the difference.

[15] My muse did stop back in for a moment to ask me to consider whether
I prefer Quixotic descriptivism to descriptivist Quixotism.  But she left
quickly enough that we'll leave the consideration of that question to 
another essay.

---

*Version 1.0 of 2017-02-19.*
