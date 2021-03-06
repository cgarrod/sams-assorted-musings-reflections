A short introduction to Make
============================

**Part of an ongoing series of essays tentatively entitled _Don't embarrass
me, Don't embarrass yourself: Notes on thinking in C and Unix_.**

As you likely know if you are a C programmer (or, in fact, anyone who does
programming involving the command line), at some point, you need to develop
some arcane incantations to build your programs.  For example, we
recently considered [a simple C project](cnix-simple-c-project) that
included a command-line tool for computing greatest-common divisors.  Let's
look at the commands we were using to build that tool [1].

    $ cc -g -Wall   -c -o gcd.o gcd.c
    $ cc -g -Wall   -c -o mathlib-gcd.o mathlib-gcd.c
    $ cc -o gcd gcd.o mathlib-gcd.o

How are you going to remember those?  Well, I suppose that you could
write them down somewhere.  You'd probably put them in a file, right?
And then maybe you'd copy and paste from the file.

As I think I told you, one of the ways real programmers approach
the world (or at least real Unix programmers) is that they strive to do
things "by code", rather than "by hand".  Copying and pasting is doing
things by hand.  We should have an alternative.  Here's one simple one,
we can build a shell script.

    #!/bin/bash
    # Build the amazing gcd application

    cc -g -Wall   -c -o gcd.o gcd.c
    cc -g -Wall   -c -o mathlib-gcd.o mathlib-gcd.c
    cc -o gcd gcd.o mathlib-gcd.o

Now, whenever we want to build this `gcd`, we just run this script.  That's
all well and good, right?

I'll note that this approach, while reasonable, has some flaws.  First,
how do we remember when we have to rebuild `gcd`?  Clearly, we need to
rebuild it when we change `gcd.c`, but will we remember that we also
have to rebuild it when we change `mathlib-gcd.c` or `mathlib.h` for that
matter?  In addition, we've hard-coded a particular set of compiler flags.
What if we decide we want to add others, such as `-DNDEBUG`?  As sensible
programmers, we should probably use variables.

This project is also small.  What happens when we get to a larger project,
one with dozens, or hundreds, or even thousands of source files?  Do we
want to re-compile each file if we change just one [2]?  

As you might expect, these are not new questions.  Sensible programmers
have thought about these issues for a long time, and have developed
a variety of build automation tools.  Most C programmers use a tool
called Make, originally developed by Stuart Feldman [3] in the mid-1970's.
Like many Unix programmers, I use Make not just for my C programs, but
also to manage a wide variety of other kinds of projects, including my
Web sites.

Make is a powerful and complex enough tool that it's impossible to
cover all of Make in a few essays.  So I'm going to focus on some key
points that I find valuable, and note that there are great resources
for learning more about Make, including [_Managing Projects in GNU
Make_](http://www.oreilly.com/openbook/make3/book/index.csp) and [_The
GNU Make Manual_](https://www.gnu.org/software/make/manual/).  

Let's start with the basics.  If you think about the instructions we
wrote above, as well as the related questions, you'll see that there
are generally three things we need to think about when deciding the
steps of building software.

First, there are the *targets*, the things that we build.  In our simple
project, we have the `.o` files, the main executable, the test program,
and, perhaps, a library file.  Second, there are the *dependencies*.
These indicate what we have to rebuild when something changes.
For example, if we change `mathlib-gcd.c`, we'll need to rebuild both
`gcd` and `test-gcd`.  However, if we change `test-gcd.c`, we only need
to rebuild `test-gcd`.  Finally, we have the *instructions* that indicate
how we build our targets from the dependencies.

In Make, we lay out this information using the following form.

    TARGET: DEPENDENCIES
    	INSTRUCTIONS

That is, we write the target, a colon, a list of files the target depends
upon, and the instructions for building the target.  Note that the whitespace
before the instructions must be a tab; a sequence of spaces doesn't count.

So, let's look at some of the rules for our project.  First, we will note
that we need to rebuild `mathlib-gcd.o` if either `mathlib-gcd.c` or
`mathlib.h` changes.

    mathlib-gcd.o: mathlib-gcd.c mathlib.h
    	cc -g -Wall   -c -o mathlib-gcd.o mathlib-gcd.c

Similarly, we need to rebuild `gcd.o` if either `gcd.c` or `mathlib.h`
changes.

    gcd.o: gcd.c mathlib.h
    	cc -g -Wall   -c -o gcd.o gcd.c

Finally, to build the executable, we link the files together.

    gcd: gcd.o mathlib-gcd.o
    	cc -o gcd gcd.o mathlib-gcd.o

Let's see how well that works.  If you put those lines into a file
called Makefile, you can create `gcd` by typing `make gcd`.  Here's
what I see when I do so.

    $ make gcd
    cc -g -Wall   -c -o gcd.o gcd.c
    cc -g -Wall   -c -o mathlib-gcd.o mathlib-gcd.c
    cc -o gcd gcd.o mathlib-gcd.o

Yay!  It even figured out the order in which to do the instructions.
What about the tests?  Well, let's add two sets of rules, one for the
`.o` file and one for the executable.

    test-gcd.o: test-gcd.c mathlib.h
    	cc -g -Wall   -c -o test-gcd.o test-gcd.c

    test-gcd: test-gcd.o mathlib-gcd.o
    	cc -o test-gcd test-gcd.o mathlib-gcd.o

We're even going to add a special kind of target, one that refers to
an action, rather than to a file.  We want to run tests.  What do we
need to do to run tests?  Right now, we just need to generate test-gcd
and run it.  Later, we might have other tests.  So we'll write a target
called `test`.  

    test: test-gcd
    	./test-gcd

Let's see how well that works.

    $ make test
    $ cc -g -Wall   -c -o test-gcd.o test-gcd.c
    $ cc -o test-gcd test-gcd.o mathlib-gcd.o
    $ ./test-gcd
    $

Make figured out that it didn't need to recompile `mathlib-gcd.c`, since
it hadn't changed.  So it compiled `test-gcd.o`, linked it with
`mathlib-gcd.o`, and then ran the result.  Pretty awesome, isn't it?

Now, let's see what happens if we change a file.  Suppose we change
`test-gcd.c`.  What do we need to rebuild?  Let's see if Make can tell.
(Note that we are using the `touch` utility to update the modification
date of a file to simulate changing the file.)

    $ touch test-gcd.c
    $ make gcd
    make: 'gcd' is up to date.
    $ make test-gcd
    cc -g -Wall   -c -o test-gcd.o test-gcd.c
    cc -o test-gcd test-gcd.o mathlib-gcd.o

Once again, Make figured out what work had to be done and what work didn't
have to be done.  Let's try a few more examples

    $ make gcd test-gcd
    make: 'gcd' is up to date.
    make: 'test-gcd' is up to date.

    $ touch mathlib-gcd.c
    $ make gcd test-gcd
    cc -g -Wall   -c -o mathlib-gcd.o mathlib-gcd.c
    cc -o gcd gcd.o mathlib-gcd.o
    cc -o test-gcd test-gcd.o mathlib-gcd.o

    $ touch mathlib.h
    $ make gcd test-gcd
    cc -g -Wall   -c -o gcd.o gcd.c
    cc -g -Wall   -c -o mathlib-gcd.o mathlib-gcd.c
    cc -o gcd gcd.o mathlib-gcd.o
    cc -g -Wall   -c -o test-gcd.o test-gcd.c
    cc -o test-gcd test-gcd.o mathlib-gcd.o

Yup, it looks like things are going well, and we'll leave it at that.

You now know the basics of Make.  In designing a project, you should
think about the source files you will create, the target files you
will generate, the ways in which the targets depend upon the source
files (and upon each other), and the instructions to build the targets.
All of that should happen, whether or not you are using Make.  Once you
start using Make, you take the additional step of expressing them in this
simple syntax, and type `make TARGET` to generate a target.  It doesn't
take a lot of effort beyond what you'd have to do to figure out the magic
incantations in the first place and, after that, Make remembers the rest.

Are we done?  No. While you can certainly get a lot out of Make
with these basic ideas, there's a lot more that you should know [5].
For example, at some point, you probably noticed that we were writing
very similar instructions.  The Don't Repeat Yourself (DRY) principle
suggests that that's bad practice.  How can we make our code more concise
and less repetitions?  We'll consider those issues, and many more,
in future sections.

---

[1] Yes, I realize that there are other sequences of instructions we could
use, particularly if we want to put our library code in, well, a library.
But we'll stick with those for now.

[2] I think the answer is "it depends".  If other files depend on the
*source code* in that file (e.g., if the file is a header file), then we
probably do need to recompile everything, or at least many things.  But
if we're just using that file as a utility, we probably just need to
recompile that one file and then relink everything.

[3] I recently met Feldman at a conference and thanked him for creating
Make [4].  He said something like "I still regret that one really bad
design decision I made."  Of course, I'm clueless, and couldn't think
of any really bad design decisions in Make, even though there's one I
curse every time I use Make.

[4] If I'd seen him later, I would also have thanked him for his talk,
which was aimed mostly at students, and, in effect, was of the form "If
you want to get an interesting programming job, particularly one at Google,
you better know how to do running-time analysis."

[5] In fact, no competent Make user would write the instructions I wrote
above.  But I think it's worth seeing some basic instructions to help
you understand what's going on before you delve into complexities.

---

*Version 1.0.1 of 2017-02-06.*
