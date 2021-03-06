Placing incoming students in Mathematics, Statistics, and Computer Science
==========================================================================

As I mentioned [about two weeks ago](sunday-2017-07-30), one of my
responsibilities is to place incoming students in Mathematics, Statistics,
and Computer Science. "Why", you may ask, "does someone in CS do the
placement?" The answer is both simple and complex.

It used to be that the department of Mathematics and Computer Science
manually placed each incoming student by reviewing their information
(e.g., scores on standardized tests, grades on high-school math courses,
numbers of semesters of various kinds of high-school math courses). Then
my colleague, Henry Walker, turned placement into a research project and
wrote a system that "automatically" places students and even generates the
placement letters. A decade or so passed, and a second research project
transformed the original system into something a bit more usable.

It made sense that Henry did the placement when we were a joint
department, particularly since he came to Grinnell as a faculty member
in Mathematics. He continued to run the placement system after the two
departments split because it served as a useful venue for advertising
CS---students read their Math/Stats placement letters; it's not clear
that they would read CS placement letters, particularly since so few
take CS in high school. Do we still need to advertise CS now that we are
one of the most popular majors on campus? Believe it or not, but we've
still found it valuable to advertise.  I know that one of my favorite
members of the class of 2020 took CS primarily because the placement
letter suggested it [1].

When Henry moved to Senior Faculty Status, I inherited the system. 
This year is my third running it more-or-less on my own. I tell myself
that it will take a bit of time. But things went so smoothly when I did
the initial placements that I thought "Oh, I've finally got things down
well enough that it won't be a lot of work." I was wrong.

After doing the initial placements, I found myself doing the following.

I met with the chair of Mathematics and Statistics to review students
who had received transfer credit. We spent about an hour going through
the placements associated with these students and thinking more generally
about how we place transfer students and students with strong AP scores.
That also contributed to further discussions of whether the placement
system was still making the "decisions" that we thought it should make.
We discussed one possible new decision and decided not to try to implement
it this year. I'll need to revisit the issue with their department some
time through this year.

I spoke with the chair of Mathematics and Statistics about other issues
in the letters. For example, we considered some language used in the
letters and we identified which faculty members would be able to talk
to students about their placement [2].

I then realized that there were some flaws in the expert system with
regards to CS. In particular, I discovered that the system was
encouraging students with weaker math backgrounds to start in our
non-majors course even when they had multiple semesters of computers
science. I thought that that recommendation would feel insulting
to students. It's also complicated by our inability to offer the
non-majors course in 2017--18. I discussed appropriate placement with
the department. I considered trying to update the rules in the system.
Instead, I ended up manually re-placing [3] the students in this category.

I realized that the language about both CSC 105 and CSC 151 was incorrect.
The former was incorrect in that we are not offering the course in
2017--18. It appears that I hadn't been looking at that issue well
last year since last year's letter said, "We will not offer CSC 105 in
2015--16" [4]. In the case of CSC 151, we are moving from a model of
the course in which students ground their learning of computer science
principles in problems in image making and manipulation to [one in
which students ground their learning of computer science principles
through activities related to data science](index-151). That meant
that I had to go through and update any of the descriptions of CSC
151. Unfortunately, we did not follow the DRY principle [5] in the
code for generating letters.  There may be a good reason. After all,
we could be trying to convey subtly different ideas to different groups
of students. But it meant that I had a lot to update.

I knew in advance that I would want to update the images in the letter.
Why are there images? Well, the letter is only one-and-one-half pages
long, which leaves room for something else. As part of the "Advertise
CS" initiative as well as a broader "Advertise the cool opportunities
in Mathematics, Statistics, and Computer Science" initiative, we've
started including images from both CSC 151 and from student MAP projects
in that space.  It makes sense to continue that practice, even
after the form of CSC 151 projects changes this semester.

That meant I had to dig out some images from my classes and from others,
think about representation issues [6], remember how the system was set
up to include images, figure out whose work the images represented,
change the layout [7], and so on and so forth.

I also fixed some long-standing formatting issues in the letter, updated
the list of people available for placement advising, dealt with language
associated with the unavailability of some folks because of the eclipse
[8,9],

When I showed a draft letter to my colleagues, they said something incredibly
helpful, approximately

> The kerning looks good for the regular face, but not so good for the
 bold face. You should fix that.

One of them did, fortunately, provide code that is supposed to make a
difference [11]. I don't see the difference, but I'll hope that it's
there. I could, of course, compare a letter done without the extra
kerning code to one done with it, but that's not worth my time right now.

Henry and I have very different perspectives about the user interface
for tools like the placement system. Henry likes to have programs print
prompts, check the input, and then re-prompt the user if the input is
incorrect. I would much prefer to provide the input on the command line
and get an error message if the input is incorrect. Since I have to run
the system for the next few years, I thought I should make at least one
update this year, for the script that generates letters.

Here's how it looks in the Henry version.

<pre>
$ php generateLetters.php
Which year would you like to generate letters for?
2017
PHP Deprecated: mysql_pconnect(): The mysql extension is deprecated and will be removed in the future: use mysqli or PDO instead in /path/to/placement/system/mysql-connection.inc on line 7
Please enter sorting criteria: 
1 - name
2 - advisor, student name
3 - pobox
4 - studentid
1
[543 lines elided for confidentiality]
Total Number of students: 453
</pre>

Here's how it looks now that I'm done hacking at it.

<pre>
$ php generateLetters.php 2017 1
PHP Deprecated: mysql_pconnect(): The mysql extension is deprecated and will be removed in the future: use mysqli or PDO instead in /path/to/placement/system/mysql-connection.inc on line 7
[543 lines elided for confidentiality]
Total Number of students: 453
</pre>

There is, of course, more work to do. I should either take the filename
for the letters as a command-line parameter or send the data to stdout.
I need to fix the deprecated `mysql_pconnect`. But since I don't use PHP
except when working with the placement system, I don't feel particularly
encouraged to do so now.

I formatted the 453 letters [12], printed them [14], and brought them
to student affairs for them to place in the students' advising folders.

Of course, those aren't the only letters to print. I'm also supposed
to print a separate set to put in student boxes. You may have noted
that the prompts above suggest that we can order them by box number.
For some reason, that didn't work with my new command-line hack. But I
didn't realize it until *after* I'd printed them, which meant that I
started going through them to put them in order [16]. I discovered a
variety of interesting issues. Two students have the same box number
[17]. Students with apostrophes in their names lost the apostrophes.
And maybe one more.

What happened with the apostrophes? I'm pretty sure that it's something
with how the PHP script processes the input file. I wrote to Henry and
he sensibly suggested that it's not a good idea to update a system during
production time. I did, however, find the line that does it.

<pre>
  //remove ' from field values
  $string = str_replace("'","",$string);
</pre>

So that seems pretty damn intentional. I wonder why they made that
decision in designing the system. But, like Henry, I don't want to
update a system while it's being used. In any case, I went back and
identified which students had apostrophes in their name and updated the
database manually. I printed replacement letters, replaced them in the
"organized by box number" pile, and brought a second set of replacement
letters to student affairs to update the incorrect letters I had not
noticed previously.

I asked the Registrar about a strange standardized test score and a
related course equivalency. They told me to update the course equivalency.
This may be the first time I've had to do so. It's good that I know how
to work directly in MySQL.

I then generated summaries [18] and placement sheets [19] and placed them
in faculty boxes.

Am I done?

No.

I have a long-term commitment to putting everything on GitHub. That will
allow [21] me to clean up the stuff a bit [22]. It will also make it
easier for me to back out of some of the changes I've tried to make.
Perhaps most importantly, it will let me more easily run the scripts on a
virtual machine that has no other purpose than to serve as our placement
system. That switch will help better protect student information.

But putting it on GitHub probably has to wait until a lull in the
semester, particularly because I want to consider how to separate
the confidential information from the code [23]. Maybe fall break.

I also need to get the final placement data to the Registrar's office.
I've given placement sheets to my colleagues. I'll ask for those sheets
back in a few weeks, enter any changes into the database, and send
the revised placement data to the Registrar's office [24].

Once I've done those two things, I'll be done for this year.

Unless I decide to go back and fix the multiple issues I've just mentioned
[25].

Maybe next year I'll just let the Historian do it [30].

---

[1] That's not the only way we get students who might not otherwise
take CS. One of my favorite majors in the class of 2019 took CS only
because their advisor cajoled them into it.

[2] Believe it or not, although we rely on the expert system for
preliminary placement, we also want incoming students to speak with us
to help make sure that the placement is as good as it can be. That's
become even more important in recent years as the College has shortened
the deadline for students to add, drop, and switch levels.

[3] Placing again, not replacing.

[4] We did offer CSC 105 in 2016-17 and we hope to offer it again in 2018-19,
although that version may be taught by an affiliated faculty member from 
another department.

[5] Don't Repeat Yourself.

[6] That is, what does including particular images say about who "does"
computer science, mathematics, or statistics and who participates in
projects.

[7] We had relied on square images in the past. This year, I used two
landscape-orientation images from summer MAP projects.

[8] No, we don't have faculty members who are so terrified of the eclipse
that they are cowering in their basements. We do, however, have faculty
members who consider it important to go to a spot where they can view
the 100% eclipse rather than 95% eclipse.

[9] The letter now indicates something on the order of "If
you want to speak to someone in CS, see X on Monday or Y on Sunday or
Tuesday [10]."

[10] Three of us are out of town for the Eclipse. Two of us are teaching
Tutorial and should be spending Monday and Tuesday with their tutees.
One of us is on leave and should be avoiding administrative work.
One of us is brand new to Grinnell and should not advise students on 
placement.  The numbers of available faculty don't look good.

[11] Here's what they sent me.

<pre>
\usepackage{microtype}

%%% Set up microtype %%%
\microtypesetup{
  final,
  tracking=true,
  kerning=true,
  spacing=true,
  factor=1100,
  stretch=20,
  shrink=20
}

%%% No tracking for smallcaps %%%
\SetTracking{encoding={*}, shape=sc}{0}
</pre>

[12] `pdflatex letters2017.tex`; it's not hard and it's relatively fast.
Switching to pdflatex was one of the first things I did when taking over
the system.

[14] Printing is less easy. Our printer freezes if I try to print all
906 pages [15]. So I have to break them up into groups of fifty or so
pages. And then I have to keep track of which ones I printed.

[15] 453 double-sided letters.

[16] Yes, I eventually decided to ask our ASA to do that task.

[17] I reported that issue.

[18] We placed _N_ students in CSC 151, _M_ in CSC 161, etc.

[19] Stu Dent has recommended placement of 151 for CS, 208 [20] for 
Statistics, and 131 for Math.

[20] 208 is code for "Take MAT 131 (Calculus I) and then take MAT 209
(Applied Statistics)." We contrast it with 209, which is for students
who have already taken Calculus I.

[21] encourage?

[22] There are way too many files that Henry or I created along the way.

[23] The confidential information includes not just the FERPA-protected
information, but also the password and account for the MySQL database.

[24] If our ITS folk have their way, I'll place the data on an encrypted
flash drive and hand-carry it over. That actually seems like a reasonable
approach to me.

[25] Have you kept count? I haven't. Let's see ... (a) We're relying on a
deprecated SQL library. (b) My command-line hack for the generate letters
script doesn't seem to work for organizing the letters by box number. (c)
The generate letters script should take the file name as a command-line
parameter. (d) We need to figure out why they are intentionally removing
apostrophes from the input.  If there's a good reason, I should write
a more sensible script to fix the names the "remove apostrophes"
code munges. (e) I should find more sensible ways to notate the
more complicated placements [26]. (f) I should make more scripts work
from the command line. (g) I should check on the kerning.  Oh, so much
fun [27]!

[26] Did I mention that one?  One of my colleagues complained about
the strange numbers.  My understanding of the system suggests that
it requires the numbers for some of the "cascading" decisions (e.g.,
"If the math placement is about 131 ...").

[27] I'm both serious and facetious.  Since I enjoy writing code, there
are parts of updating the code that I will probably enjoy.  I even enjoy
debugging because I find a sense of accomplishment when I figure out
the cause of a bug [28].  On the other hand, since it's someone else's code
written in a language I don't regularly use, there are other parts I
will most certainly not enjoy.

[28] The word "I" appears a lot in that sentence.  But I like that sentence
more than "I even enjoy debugging because there is a sense of accomplishment
that accompanies the discovery of a bug."  Perhaps it's because the Little
Red Schoolhouse emphasized the value that readers find in active verbs 
rather than nominalizations [29].

[29] Yes, there are better ways to write that sentence, too.  But it's
late.

[30] Feel free to ignore this inside joke [31].

[31] I'm relatively sure that a week from now, I won't be one of the
insiders [32].

[32] That means that I'll have forgotten what the joke is about.

---

*Version 1.1 of 2017-08-12.*
