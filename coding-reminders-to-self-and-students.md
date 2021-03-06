Some coding reminders to myself (and my students)
=================================================

I spent the past few hours dealing with code that I inherited, written
in a language that I don't regularly use [1] Dealing with the code
reminds me of many of the coding principles I try to teach my students.
Why does it remind me? Because it violates most of them. Let's consider
a few.

**Document**. I used to joke that my first project for my advisor [2]
was one of my most interesting programming experiences because the
code I had to deal with had one comment. If I remember correctly, it
was `; bits`, and came immediately after `(define byte 8)`.  But the
rest of the code was at least sensible, with good variable names and a
comprehensible structure.

The code I'm dealing with now makes huge assumptions about structure
(e.g., that strings which begin with "xxxxxxx" are processed one way
and that strings that begin with "actionitem" are processed another)
without ever explaining the differences or which prefix you'd choose
in which situation. Fortunately, I'm making only minor changes to the
code. But if I ever have to make major changes, I expect to be cursing
[3].

**Format your code well**. Why do people use tabs to indent code,
particularly when they have more than three levels of indentation?
It makes the code unreadable in a traditional terminal window. There's a
reason that most sensible formatting guidelines [4] suggest two spaces
per level of indentation. I end up re-indenting whenever I look at code
in this project. Of course, this isn't a lesson for myself; I tend to
be fairly careful about my formatting [5].

Documenting and formatting are part of a broader practice in which you
**write clear code**. Choose good variable names. Use procedures [6]
to encapsulate code. Use sensible control structures. Avoid jumps
or go-to instructions when possible. Put related actions together. 
Often, it's easiest to teach how to write clear code by showing unclear
code and then improving it. The code I was dealing with defines some
code structures in one place and then uses them in another without
any clear relationship between the two. I'm not sure, but I *think*
the code would benefit from a very different high-level approach [7].

**Don't assume that procedure calls succeed**. When my students program in
C, I tell them to check the return values from procedures. For example,
if they call `malloc`, they should make sure that `malloc` actually
allocated something, and have a fallback if it didn't [9]. The designers
of this program do not seem to have the same fondness for robust code
that I do. I discovered this when trying to figure out why one of our
clients was saying "I used to see this data; now I don't". I had assumed
that when we "upgraded" the code, that data display had been removed.
It turns out that one in a series of data display instructions was
failing and therefore preventing the remaining instructions from running.
I'm pretty sure that the problem could have been solved were the code
more sensible. I did not want to delve into the details of their code,
which seemed to be spread out over multiple parts of the same file and,
as I noted already, was undocumented.  Hence, I just "commented out"
the failing display instruction and crossed my finger that the client
did not need that information [10].

I know that most programmers assume that calls will work and say to
themselves "I'll go back and add the error-checking code later." The
problem is that later never seems to come. Either they run out of time,
or they forget, or they never encounter an error and decide it's not an
issue or .... But, eventually, the failure to error check comes back
and bites them [11].

While I critique many things about Java and find that Java's exceptions
sometimes get in my way. But I really appreciate that Java's exceptions
mean that programmers can't just say "I'll wait until later to figure
out what to do if this call fails." The compiler generally makes you
make that decision now, not later [12].

Now, I'll admit that I also write programs that don't sufficiently check
for errors [14]. But those are programs that I've written for myself,
not for others. I don't expect others will use them, let alone have
to read and fix the source code. I also find that most error checking
doesn't really take that much more time to write.

**Use sensible commit messages**. I was trying to track down when a
bug was introduced in the program. In looking at the history of the
program, I found that most of the commits had wonderful descriptive
tags, like "asdf" and "asd" [15]. I did realize that I tend to do
one-line commit messages, which may not provide enough information.
For example, I will probably forget *why* I changed a variable name.
But knowing why may be important when I need to go back and deal with
an implication of that renaming. While I've found the commit that seems
to have broken the code, I still haven't figured out why.

**Test your code**. It's better to test with automated tests of one sort
or another. That way, when you make a change, you can see if the change
breaks something unexpected. But good tests are hard. And testing
complex systems is, well, complex. 

Do I think a good test suite would have caught this particular error?
Since I don't know what the error is, it's hard to say. It may be that
we're at enough of an edge case or a slightly unexpected configuration
that a test suite wouldn't have helped. But it couldn't have hurt.

Now, I'll admit that I don't always test as much as I should, particularly
in my quick hacks for myself. Still, it's a worthwhile and important
step in writing code you expect others to use.

**Check for security holes**. This step is difficult. Most security flaws
come from someone who approaches the program from a different perspective.
The person writing the program is ingrained enough in the development of
that program that they may not come up with other perspectives. But you
should look for common errors: unchecked array indices, unsanitized
input, and so on and so forth.

Yes, there appears to be a security hole in the code I'm dealing with [16].
No, I won't tell you what it is. 


---

I've covered most of the important issues, so let's go through the list
again to make sure [17]. Write clear code. Format it to support clarity.
Document it. Make sure that it's robust. Put it in a repository and
use clear messages when you commit. Test early and often. Strive for
security. Those make a good starting point.

There are certainly other things a good programmer does, such as check
for memory leaks and avoid abusing language features for purposes other
than those for which they are intended. But most of those other things
get covered by the practices I've mentioned.

What important practices have I left out? Let me know.

---

Although I mentioned my students in the title of this musing, this musing
is not about code I inherited from my students. If I'd inherited it from
my students, you might have been reading a much shorter musing about my
attempts to retroactively fail students and, if necessary, invalidate
their degrees [18].

I mentioned my students in the title only to indicate that these are
practices that are as important for my students as they are for me, and
that I should inculcate these habits in my students.

---

I realize that this musing is below my standards. They can't all be
winners. Still, I found it emotionally useful to reflect on the many
reasons I dislike working with the code I inherited.

---

[1] PHP. I think it either stands for "Portable Hypertext Processor"
or "PHP Hypertext Processor". My general experience with the PHP code
I see is that it should stand for "Pretty Horrible Programming".

[2] A compiler. I was going to write an explanation of the project here.
But the explanation started to get long. So I'll just say "Stay tuned
for a forthcoming musing".

[3] And cursing a lot.

[4] E.g., the [GNU standards](https://www.gnu.org/prep/standards/standards.html)
and [one of the more common Ruby standards](https://github.com/bbatsov/ruby-style-guide).

[5] I'm less careful about formatting when I work in similar languages
with different conventions. For example, my C code looks different than
my Java and Perl. So when I switch between the languages, my formatting
tendencies sometimes follow along and my formatting becomes inconsistent.

[6] functions, subroutines, methods, whatever you want to call them.

[7] Basically, the program makes a list of pairs of values (SQL
query, description of query) at the start of the program and then
iterates through the list, executing each query in different ways
depending on some extra text in the query. I think they would have
been better off turning the body of the loop into a three-parameter
procedure and the list of context/query/description "pairs" into calls
to that procedure. The code would be more readable and probably easier
to document and modify. If I need to do long-term maintenance, I'll
almost certainly go back and make that change [8].

[8] The code bothers me enough that even if I don't need to do long-term
maintenance, I may go back and make the change.

[9] The fallback is most frequently that their procedure should fail and
signal the failure to its caller.

[10] If you care, the code was doing a series of SQL queries and assuming
that all of the queries succeeded. There was too much program logic to
unpack to figure out what to do if each query failed.

[11] Or bites their successor, in this case, me.

[12] Yes, there are workarounds, such as having all of your methods
include `throws Exception`. But most programmers get frustrated at
that approach.

[14] I almost always check for returns from `malloc`. What I tend
to fail to check is whether or not a file-open call succeeds.

[15] For those who aren't sure, "asdf" is a horrible commit message.
It represents the action of a programmer who is forced to come up
with a commit message but is too damn lazy to explain what they just
did.

[16] No, the security hole is not "They used PHP". But PHP is probably
a security hole.

[17] In point of fact, I did go through the list again while writing
this musing and found that I had missed a few issues, which I then went
back and added.

[18] No, I wouldn't really try to retroactively fail students. But
it's sometimes nice to pretend.

---

*Version 1.0 released 2018-01-12.*

*Version 1.0.1 of 2018-01-13.*
