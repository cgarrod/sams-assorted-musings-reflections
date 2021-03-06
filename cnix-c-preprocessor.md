The C Preprocessor
==================

**Part of an ongoing series of essays tentatively entitled _Don't embarrass
me, Don't embarrass yourself: Notes on thinking in C and Unix_.**

As we saw in [an earlier essay](cnix-compilation-phases.md), the first
step the C compiler does is to *preprocess* the source file.  Preprocessing
generally involves simple textual manipulation.  There are a variety of
things that the preprocessor does.

* The preprocessor strips out any comments in the file.  Why?  Because
  comments are generally for human beings [1].
* The preprocessor replaces any line of the form `#include <FILENAME>`
  with the contents of the named file, using a standard list of directories 
  to look for the file.
* The preprocessor replaces any line of the form `#include "PATH_TO_FILE"`
  with the contents of the named file, treating the path relative to the
  current directory [2].
* The preprocessor replaces any constants defined on the command line
  with `-DCONSTANT=VALUE` with the corresponding value.
* The preprocessor replaces any constants defined in the file with
  `#define CONSTANT VALUE` with the corresponding value.
* The preprocessor handles conditional sections, which we will cover a
  bit below.
* The preprocessor handles macros, which we will cover in subsequent readings.
* The preprocessor inserts comments that help the remaining compiler
  steps identify where in the original file they are so that they can
  provide appropriate error message.
* A few more things that you don't need to know right now.
* Many more things that I either never knew about or forgot about.

Let's look at the first few in turn.

First, let's watch the preprocessor strip comments.

<pre>
$ cat example1.c 
/**
 * example1.c
 *   An example to illustrate comment stripping.
 *
 * <insert appropriate FLOSS license>
 */

int
main (int argc[], char *argv)
{
  return 0;
} // main

$ cc -E example1.c
# 1 "example1.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example1.c"







int
main (int argc[], char *argv)
{
  return 0;
}
</pre>

Not very interesting, I know.  You'll note that even though the preprocessor
has stripped the comments, it has left blank lines so that other programs
count appropriately.

Now, let's try working with an included file.  

<pre>
$ cat example2.c 
#include "include2.c"

int
main (int argc, char *argv[])
{
  return result ();
} // main

$ cat include2.c 
int
result (void)
{
  return 0;
} // result

$ cc -E example2.c
# 1 "example2.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example2.c"
# 1 "include2.c" 1
int
result (void)
{
  return 0;
}
# 2 "example2.c" 2

int
main (int argc, char *argv[])
{
  return result ();
}
</pre>

You may note that I've included a `.c` file, rather than a `.h` file,
and that the included file contains code.  C doesn't care what's in the
included file; it just grabs the contents.

What happens if we try to include the same file twice?

<pre>
$ cat example3.c
#include "include3.c"
#include "include3.c"

int
main (int argc, char *argv[])
{
  return result3 ();
} // main

$ cat include3.c 
int
result3 (void)
{
  return 0;
} // result3

$ cc -E example3.c
# 1 "example3.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example3.c"
# 1 "include3.c" 1
int
result3 (void)
{
  return 0;
}
# 2 "example3.c" 2
# 1 "include3.c" 1
int
result3 (void)
{
  return 0;
}
# 3 "example3.c" 2

int
main (int argc, char *argv[])
{
  return result3 ();
}
</pre>

As we can see in the preprocessed source, we get the contents of the file
included twice.  Is that a problem?  Let's see.

<pre>
$ cc example3.c
In file included from example3.c:2:0:
include3.c:2:1: error: redefinition of ‘result3’
 result3 (void)
 ^
In file included from example3.c:1:0:
include3.c:2:1: note: previous definition of ‘result3’ was here
 result3 (void)
 ^
</pre>

Yup, that's definitely a problem.  Now, you maybe thinking to yourself
"Sam, no one would ever include the same file twice."  But it turns
out that that's not true.  Sometimes, you will include two files, and
each will include the same other file [3].  There are also a few times
that programmers intentionally include the same file, but they then 
arrange to avoid overlaps [4].  How do we handle the inadvertent
double include?  We'll get to that topic soon.

Next up are constants.  You may recall that one way to define constants
is on the command line, with `-DCONSTANT=VALUE` [5].  Let's explore
that approach.

<pre>
$ cat example4.c 
int
main (int argc, char *argv)
{
  return ZERO;
} // example4

$ cc -E example4.c
# 1 "example4.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example4.c"
int
main (int argc, char *argv)
{
  return ZERO;
}

$ cc -DZERO=0 -E example4.c
# 1 "example4.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example4.c"
int
main (int argc, char *argv)
{
  return 0;
}

$ cc -DZERO=1 -E example4.c
# 1 "example4.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example4.c"
int
main (int argc, char *argv)
{
  return 1;
}
</pre>

Wasn't that fun?  As you can see, defining constants with command-line
flags lets us quickly reconfigure our program [6].  However, we more
often define the constants directly in the file, with `#define`.

<pre>
$ cat example5.c 
#define ZERO 0

int
main (int argc, char *argv)
{
  return ZERO;
} // example5

$ cc -E example5.c 
# 1 "example5.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example5.c"


int
main (int argc, char *argv)
{
  return 0;
}
</pre>

What happens if we try to redefine `ZERO` on the command line?  Let's
see.

<pre>
$ cc -DZERO=7 -E example5.c
# 1 "example5.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example5.c"
example5.c:1:0: warning: "ZERO" redefined
 #define ZERO 0
 ^
<command-line>:0:0: note: this is the location of the previous definition


int
main (int argc, char *argv)
{
  return 0;
}
</pre>

As one would hope, the preprocessor issues a warning.  It appears that
it uses the most recent definition.

Of course, it seems a bit silly to define `ZERO` as 0.  But we will
more frequently use constants which represent values that are constant
throughout the program, but which we may want to vary when compiling
the program.  Here's one such example.

<pre>
$ cat example6.c 
#define ARRAY_LEN 16

int
main (int argc, char *argv)
{
  int values[ARRAY_LEN];
  int sum = 0;
  int i;

  for (i = 0; i < ARRAY_LEN; i++)
    sum += values[i];

  return sum;
} // example6

$ cc -E example6.c 
# 1 "example6.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example6.c"


int
main (int argc, char *argv)
{
  int values[16];
  int sum = 0;
  int i;

  for (i = 0; i < 16; i++)
    sum += values[i];

  return sum;
}
</pre>

What are preprocessor conditionals?  The simplest and most commonly
used conditionals simply check whether or not a name is defined (using
`-D` or `#define` and make different choices if it is.  For those cases,
you use `#ifdef NAME` at the beginning of a block of code and `#endif`
at the end.  If you want an else clause, you use `#else`.

Let's consider a simple example.  While we are developing our program,
we always want to seed the random number generator with the same value.
However, when we deploy, we most likely want to seed the random number
generator in a less predictable way [6].  In the example below, we
simply print out a random number.  Note that I have elided the cruft
that comes from including `<stdio.h>`.

<pre>
$ cat example7.c
#include <stdio.h>
int 
main (int argc, char *argv[])
{
#ifdef TESTING
  srand (0);
#else
  srand (time (0));
#endif
  printf ("%d\n", rand ());
  return 0;
} // example7

$ cc -DTESTING -E example7.c
# 1 "example7.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example7.c"
# 1 "/usr/include/stdio.h" 1 3 4
# ...
# 943 "/usr/include/stdio.h" 3 4

# 2 "example7.c" 2
int
main (int argc, char *argv[])
{

  srand (0);



  printf ("%d\n", rand ());
  return 0;
}

$ cc -DTESTING example7.c -o ex7test

$ ./ex7test
1804289383

$ ./ex7test
1804289383

$ ./ex7test
1804289383

$ cc -E example7.c
# 1 "example7.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example7.c"
# 1 "/usr/include/stdio.h" 1 3 4
# ...
# 943 "/usr/include/stdio.h" 3 4

# 2 "example7.c" 2
int
main (int argc, char *argv[])
{



  srand (time (0));

  printf ("%d\n", rand ());
  return 0;
}

$ cc example7.c -o ex7

$ ./ex7
128869637

$ ./ex7
1972234548

$ ./ex7
292607164

$ ./ex7
2130073865
</pre>

Conditionals are one way we normally avoid repeated includes.  Here's
the earlier example, using a sensible "`#ifndef` wrapper" [7,8].

<pre>
$ cat example8.c 
#include "include8.c"
#include "include8.c"

int
main (int argc, char *argv[])
{
  return result8 ();
} // main from example8

$ cat include8.c 
#ifndef _INCLUDE_8_
#define _INCLUDE_8_
int
result8 (void)
{
  return 0;
} // result8
#endif // _INCLUDE_8_

$ cc -E example8.c
# 1 "example8.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example8.c"
# 1 "include8.c" 1


int
result8 (void)
{
  return 0;
}
# 2 "example8.c" 2


int
main (int argc, char *argv[])
{
  return result8 ();
}

$ cc example8.c
</pre>

Isn't that much nicer?  Now you know why most header files start
with an `#ifndef`.

We can also use a similar approach to define values only when they
are not defined on the command line [9].

<pre>
$ cat example9.c 
#ifndef ARRAY_LEN
#define ARRAY_LEN 16
#endif

int
main (int argc, char *argv)
{
  int values[ARRAY_LEN];
  int sum = 0;
  int i;

  for (i = 0; i < ARRAY_LEN; i++)
    sum += values[i];

  return sum;
} // example9

$ cc -E example9.c
# 1 "example9.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example9.c"




int
main (int argc, char *argv)
{
  int values[16];
  int sum = 0;
  int i;

  for (i = 0; i < 16; i++)
    sum += values[i];

  return sum;
}

$ cc -DARRAY_LEN=128 -E example9.c
# 1 "example9.c"
# 1 "<built-in>"
# 1 "<command-line>"
# 1 "/usr/include/stdc-predef.h" 1 3 4
# 1 "<command-line>" 2
# 1 "example9.c"




int
main (int argc, char *argv)
{
  int values[128];
  int sum = 0;
  int i;

  for (i = 0; i < 128; i++)
    sum += values[i];

  return sum;
}
</pre>

As I hope these examples suggest, the preprocessor gives you a lot of
power to easily customize your programs.  You'll find that these aspects
of the preprocessor, along with the wonder of macros, will give you
great power and flexibility.

---

[1] There are, of course, some exceptions.  Some comments can serve as
compiler hints in some languages.

[2] Even if you execute the compilation command from another directory,
the path is relative to the directory in which the compiled file is.

[3] I'm too lazy to work out the example, but you know what I mean.  If
you don't, drop me a message and I'll write the example.

[4] That's a subject for a future essay.

[5] At times, we may write `-D'CONSTANT=VALUE'` if we are worried
about the shell doing something strange with the name or the value.

[6] I'm using `time (0)` as the seed.  That's not ideal.  But it
varies enough that I'm comfortable with it as an example.

[7] `#ifndef` is like the inverse of `#ifdef`.  It means "if not
defined".

[8] I feel way too pleased that example 8 is a variant of example 3,
given that 3 and 8 look similar.  I'm almost as pleased that this
endnote ended up as number 8.

[9] I'm also happy that the variant of number 6 is number 9 [10].  

[10] Unfortunately, when I hear "number 9", I think of the phrase "turn
me on dead man" [11].

[11] If you did not immediately understand that comment, I'm sure
that a Web search would help you.  I'm not sure what will help me
stop making stupid associations.

---

*Version 1.0 of 2017-03-24.*
