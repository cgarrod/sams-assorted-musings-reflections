Don't embarrass me, Don't embarrass yourself: `assert`
======================================================

Responsible programmers want to make sure that the code they write is
correct.  But how?  One approach is to "prove" your code correct, by
analyzing it logically.  But that's difficult and time consuming and,
not so surprisingly, programmers seem to be as likely to make errors in
their proofs as they are to make errors in their code [1].  So most
do some sort of testing, preferably unit testing that considers a wide
variety of cases, including edge and corner cases [2], but even interactive
ad-hoc testing is useful.  Often, testing reveals errors, and programmers
face an additional challenge in making sure their code is correct: Finding
their error.  Of course, a good debugger will help with that.

However, C also includes an awesome feature that addresses both these
issues: the `assert` macro.  `assert` looks like a one-parameter function
that takes a Boolean expression as input.  Semantically, `assert`
evaluates the expression and does nothing if the expression evaluates
to true and issues an error message if the expression evaluates to false.

This form makes `assert` useful for "quick and dirty" unit tests.
For example, if we had written a `square` procedure that computes the
square of a long integer, we might write some simple tests as follows [3].

    int
    main (int argc, char *argv[])
    {
      assert (square (1) == 1);
      assert (square (-1) == 1);
      assert (square (10) == 100);
      assert (square (-15) == 225);
      return 0;
    } // main

We can run the tests by compiling and running the program.

    $ make square
    cc -g -lm -Wall    square.c   -o square
    $ ./square
    $

Yay!  No output.  That's a sign of success, right?  Well, it could be
that `assert` literally does nothing, and our program does nothing more
than the `return 0`.  So let's look [4]

    $ cc -E square.c
    ...
    # 8 "square.c" 2
    
    long
    square (long x)
    {
      return (long) pow (x, 2);
    }

    int
    main (int argc, char *argv[])
    {
      ((square (1) == 1) ? (void) (0) : __assert_fail ("square (1) == 1", "square.c", 18, __PRETTY_FUNCTION__));
      ((square (-1) == 1) ? (void) (0) : __assert_fail ("square (-1) == 1", "square.c", 19, __PRETTY_FUNCTION__));
      ((square (10) == 100) ? (void) (0) : __assert_fail ("square (10) == 100", "square.c", 20, __PRETTY_FUNCTION__));
      ((square (-15) == 225) ? (void) (0) : __assert_fail ("square (-15) == 225", "square.c", 21, __PRETTY_FUNCTION__));
      return 0;
    }

Well, it certainly looks like there's some code.  We may not be sure what
`__assert_fail__` and `__PRETTY_FUNCTION__` are, but it looks like it's
evaluating the expression and then making some decisions.  You can generate
the assembly, too, if you'd like, but that seems unnecessary.  So, what
happens if the Boolean expression is false?  Let's add a few more tests [5].
 
      assert (square (2147483647L) == 2147483647L*2147483647L);
      assert (square (-2147483648L) == 2147483648L*2147483648L);

Once again, we can compile and run the code.

    $ make square
    cc -g -lm -Wall    square.c   -o square
    $ ./square
    square: square.c:22: main: Assertion `square (2147483647L) == 2147483647L*2147483647L' failed.
    Aborted

See!  When you assert things that aren't true, `assert` reports an error.
What?  You think those should have succeeded?  Clearly, you weren't
paying enough attention to the definition of `square` above [6].
That's why we test edge cases!  If we replace the definition with this
more straightforward version, the code works correctly.

    long
    square (long x)
    {
      return x*x;
    }

Let's check.

    $ make square
    cc -g -lm -Wall    square.c   -o square
    $ ./square
    $

You've now seen how to use `assert` for simple unit testing.  You've
probably also seen a potential issue: `assert` quits your program as
soon as it finds the first error.  Ideally, your unit tests will report
all of your errors.  But that's why I call it "simple" unit testing.  

What about when we find errors?  Sometimes, you need to look at the code
to figure out what is wrong.  Sometimes, you need to use a debugger.
However, `assert` is useful for an intermediate approach.  When thinking
through your code, you will often realize that you have certain expectations.
For example, consider the following code to combine a first name and a
last name into a single string [8].

    void
    join (char *name, char *fname, char *lname)
    {
      strncpy (name, fname, MAX_STR);
      strcpy (name, " ");
      strncpy (name, lname, MAX_STR);
    } // join

What do you expect to hold in order for this code to work?  Got it?
Compare your answers to mine [9].  Now, let's write some of those
expectations as assertions.

    void
    join (char *name, char *fname, char *lname)
    {
      // Check initial expectations
      assert (name != NULL);
      assert (fname != NULL);
      assert (lname != NULL);
      assert (name != fname);
      assert (name != lname);
      // Do the first step
      strncpy (name, fname, MAX_STR);
      // Make sure it worked
      assert (strncmp (name, fname, MAXSTR) == 0);
      // Do the remaining work
      strcat (name, " ");
      strncat (name, lname, MAX_STR);
    } // join

There are certainly other things to check.  For example, we might want
to make sure that the total length of the result is correct.  And, 
don't forget, we should check that we've allocated enough space for
`name`.  Can you figure out how [11]?

If all of these assertions pass, I'm pretty sure that the code will
work correctly.  If any fail, I'll know one of the things that's wrong,
and I can start to address the issue.  You could achieve similar effects
by putting in a lot of `println` statements, but then you have to read
the output to make sure it's what you expect, and then you have to pull
them out of the program before you release it [12].

Of course, given that we use the C programming language because we 
want to write fast code, you may be reluctant to put in so many 
assertions.  And you definitely worry about that extra `strncmp`,
because that's an expensive operation.  But don't worry!  One of the
great aspects of `assert` is that you can turn it off by adding
`-DNDEBUG` to your compile flags..

    $ cc -E join.c
    void
    join (char *name, char *fname, char *lname)
    {
    
      ((name != NULL) ? (void) (0) : __assert_fail ("name != NULL", "join.c", 7, __PRETTY_FUNCTION__));
      ((fname != NULL) ? (void) (0) : __assert_fail ("fname != NULL", "join.c", 8, __PRETTY_FUNCTION__));
      ((lname != NULL) ? (void) (0) : __assert_fail ("lname != NULL", "join.c", 9, __PRETTY_FUNCTION__));
      ((name != fname) ? (void) (0) : __assert_fail ("name != fname", "join.c", 10, __PRETTY_FUNCTION__));
      ((name != lname) ? (void) (0) : __assert_fail ("name != lname", "join.c", 11, __PRETTY_FUNCTION__));
    
      strncpy (name, fname, 128);
    
      ((strncmp (name, fname, MAXSTR) == 0) ? (void) (0) : __assert_fail ("strncmp (name, fname, MAXSTR) == 0", "join.c", 15, __PRETTY_FUNCTION__));
    
      strcat (name, " ");
      strncat (name, lname, 128);
    }

    $ cc -E -DNDEBUG join.c
    void
    join (char *name, char *fname, char *lname)
    {
    
      ((void) (0));
      ((void) (0));
      ((void) (0));
      ((void) (0));
      ((void) (0));
    
      strncpy (name, fname, 128);
    
      ((void) (0));
    
      strcat (name, " ");
      strncat (name, lname, 128);
    }

See!  All of the `assert` expressions got changed into noops.  Hence,
you can use `assert` while you are developing your program, and then
turn it off for the release version by adding a few characters to your
Makefile.  Of course, you may still encounter Heisenbugs [13], but any
debugging mechanism can

As I hope these examples suggest, `assert` is a useful, multi-faceted
tool.  It's not as rich and robust as some tools, but it gets lots of
jobs done.  I encourage you to use it [14].

---

[1] My colleague, Peter-Michael Osera, is working to change that situation.
Stay tuned for the results of that area of his research.

[2] We'll revisit unit testing in a future module.

[3] If we were in class, I'd write the first test and then ask the 
students to suggest some additional tests.  Are there any you'd add?

[4] I've removed the translated header files.

[5] I expect that you also came up with these tests when I asked if there
were any that you'd add.

[6] If you are still not sure why the original definition of `square`
is incorrect, I guess you'll just need to take my class [7].

[7] Or talk to a competent C programmer.

[8] Yes, I realize that this code suffers from the Shlemiel the painter
problem.  Deal.

[9] I expect that `name`, `lname`, and `fname` all refer to valid memory
locations, and that the memory associated with `lname` and `fname` has
been initialized to hold a string, rather than random data.  I expect that
there's enough memory associated with `name` to hold the total string [10].
I care that neither `fname` nor `lname` is the same memory location as
`name`, since I think making them the same is likely to screw up my program
elsewhere.  

[10] What's enough?  I think `2*MAX_STR + 2`.  I'll let you figure out why.

[11] I can't.  C isn't very nice about telling you how much memory
you've allocated.

[12] Of course, debugging with `printf` is a horrible idea, and not
just because you should be using `fprintf(STERRR, ...)`.  Sensible
programmers use debuggers or `assert` statements.

[13] A Heisenbug is a bug that doesn't occur when you debug the program,
but does appear when the program runs without the debugging information.
That is, looking for the bug makes it disappear.

[14] I will, however, admit that I don't use `assert` nearly enough in
my own C code.  I tend to use somewhat more verbose hand-crafted
alternatives.  We'll see some of those when we start exploring macros.

---

*Version 1.0.1 of 2017-01-08.*
