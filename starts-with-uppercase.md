Determining whether or not a string starts with an upper-case letter (or why Sam may fail programming interviews)
=================================================================================================================

At an upcoming session of CS table, we are discussing
a series of essays on the programming interview process.
One of the essays is [Version 3.0 of Joel Spolksy's Guerrilla Guide to
Interviewing](https://www.joelonsoftware.com/2006/10/25/the-guerrilla-guide-to-interviewing-version-30/).
About midway through the essay, Spolsky gives a variety of simple
programming challenges that "any programmer working today should be able
to solve in about one minute".

Here's the first one:

> Write a function that determines if a string starts with an upper-case
letter A-Z.

Reading that got me thinking about how I'd perform at one of these
interviews.  Here's the dialog I imagine with the interviewer.  I'll put
the interviewer's text in bold and mine in italics.

**Write a function that determines if a string starts with an
upper-case letter A-Z.**

*Okay, what language would you like me to use?*

**Well, you've taught a course on C, why don't you use C.**

*Great.  I love C.  Can I use `isupper` from the standard C library?
It's a nice, general-purpose procedure.*

**No, I'd like you to think through the details.**

*Okay, am I working in English, or am I possibly working in some other
language that may have a different alphabet?*

**You can assume that you're working in English.**

*Thanks.  The encoding of characters is not guaranteed in C.  Can I
assume that we're working with ASCII, or at least an encoding in
which the uppercase letters appear in order?*

**You may assume that the upper-case letters appear in order.  Note, however,
that you should pronounce the dash [1].  Are you done with these
questions yet?  It feels like you're trying to delay writing the code.**

*I'm not trying to delay.  I'm trying to understand the parameters
of the problem so that I write a procedure that meets the requirements.
Should I write robust code that checks for a null string?*

**Whatever you like.  Just write the code!**

*Well, I tell my students to do test-first design, so I'll begin
with a few tests.  I'm just going to use `assert`, unless you have
a preferred testing framework.*

**`assert` is fine.**

*What would you like me to name the procedure?*

**Whatever you like.**

*Do you prefer `camelCase` or `snake_style` procedure names?*

**Are you ever going to write code?**

*I'll take that as a "either".  I'm going to call it `starts_with_upper_case`.
I know that violates the C custom of short and barely understandable 
procedure names, but I like readable procedure names and a good development
environment makes it easy to use them.   Here are a few of the tests
that I'd start with.*

<pre>
assert (! starts_with_upper_case (NULL));       // Non-string
assert (! starts_with_upper_case (""));         // Empty string
assert (starts_with_upper_case ("A"));          // One-character string, edge
assert (starts_with_upper_case ("Z"));          // One-character string, edge
assert (starts_with_upper_case ("E"));          // One-character string, other
assert (starts_with_upper_case ("Q"));          // One-character string, other
assert (! starts_with_upper_case ("a"));        // One-character string, edge
assert (! starts_with_upper_case ("z"));        // One-character string, edge
assert (! starts_with_upper_case ("0"));        // One-character string, nonchar
assert (! starts_with_upper_case ("."));        // One-character string, nonchar
</pre>

**Stop!  You're just writing a simple C program!  You don't need that many tests.**

*It appears that you haven't seen the code that my students sometimes
write [2].  But okay, I'll move on.  Here you go [3].*

<pre>
int
starts_with_upper_case (char *str)
{
  return (str != NULL) 
    && ('A' <= str[0])
    && (str[0] <= 'Z');
} // starts_with_upper_case
</pre>

**Are you sure that this works correctly with the empty string?**

*Isn't that why I write tests?  Are you just reading some questions from
a list?*

**Um, maybe.**

*Well, in addition to knowing that this code passes my tests, I know
that the empty string has one character, the end-of-string mark, or
zero.  You've told me that the uppercase letters appear sequentially,
so I know that zero is not in that range.*

**I have to ask: If I hadn't stopped you, what other tests would you
have written?**

*I would have looked at longer strings.  I would probably have found a
way to create a string that starts with the character right before `'A'`
and another string that starts with the character right after `'Z'`
so that I check boundary conditions.  If I was particularly energetic,
I might have written a loop.  But I agree, I do tend toward overkill in
writing tests.  As I said, I teach novice programmers.  They make the
most fascinating mistakes*

**Why are you returning an `int` rather than a `bool`.**

*Um.  I learned K&R C and don't believe in `bool`.  But I can adapt
if you'd like.*

**No, that's okay.  Let's move on to the next problem.  Add up all the
values in an array.**

*Should I worry about overflow?*

**Wow, where does the time go?  This interview is over.  It looks
like we don't have any more scheduled for you today [4].  Thanks for visiting.
Don't call us, we'll call you.**

---

[1] I said "uppercase".  The interviewer said "upper-case".

[2] My students are novices.  They occasionally write incorrect code.
Good tests help catch errors I'd never expect.

[3] Yes, it took me under one minute.

[4] Spolsky suggests that a good strategy is not to tell someone in
advance how many interviews they are doing.  That way, when they bomb
one interview, you can just send them away.

---

*Version 1.0 of 2017-04-10.*
