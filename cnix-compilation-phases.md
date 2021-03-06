Stages of C compilation
=======================

**Part of an ongoing series of essays tentatively entitled _Don't embarrass
me, Don't embarrass yourself: Notes on thinking in C and Unix_.**

To many novice programmers, the steps from human readable program text
to running program are, at best, obscure.  Usually, it's "I click the
'Run' button in the interpreter, and it all works" or "I invoke the magic
command to create an executable, and then run the executable".  One of
the hallmarks of a competent C programmer is that they understand the 
steps in which the program text is compiled into an executable program
and how they can take advantage of those steps.

We're going to consider the standard "key" steps in compilation of C
programs to executables.  Not all compilers follow this exact sequence,
and I've left out some steps, but these are the ones that can be useful
for you to know about and use.

The first major step in C compilation is the C *preprocessor*.  What does
a preprocessor do?  It does textual substitution within the program.
Among other things, it strips out all of the comments.  It merges in
the text of any `#include`d files, it replaces `#define`d constants
with their values [1], and it replaces macro calls with the body of the
macro, doing appropriate argument substitution along the way.  The C
preprocessor can also do some fancier things, like use conditions to
choose between things to do.  Each of these topics probably deserves its
own description, so we'll leave them to future essays.  But competent C
programmers can read, use, and understand these various textual options.
Competent C programmers also know that you can use the `-E` flag for
the C compiler to run just the preprocessor [2].

Let's look at a quick example.  Here's a sample header file.

    /**
     * header.h
     *   A sample header.
     */

    /** foo takes an int and returns an int. */
    int foo(int x);

Here's a sample C program that includes that file.  (The sample program
should also include `<stdio.h>`.  But that would clutter up our example,
and so we won't include it.)

    /**
     * program.c
     *   A sample program.
     */

    #include "header.h"

    #define X 23
    #define DOUBLE(Y) (Y * 2)

    int
    main (int argc, char *argv[])
    {
      printf ("%d,%d\n", DOUBLE(5), foo(DOUBLE(X)));
      return 0;
    } // main

    int
    foo (int x) 
    {
      return x * x;
    } // foo

What happens if we run the preprocessor?  Let's see.

    % cc -E program.c
    # 1 "program.c"
    # 1 "<built-in>"
    # 1 "<command-line>"
    # 1 "/usr/include/stdc-predef.h" 1 3 4
    # 1 "<command-line>" 2
    # 1 "program.c"
    
    
    
    
    
    # 1 "header.h" 1
    
    
    
    
    
    
    int foo(int x);
    # 7 "program.c" 2
    
    
    
    
    int
    main (int argc, char *argv[])
    {
      printf ("%d,%d\n", (5 * 2), foo((23 * 2)));
      return 0;
    }
    
    int
    foo (int x)
    {
      return x * x;
    }

As you can see, the comments are all gone [4], `X` has been replaced by
`23`, `DOUBLE(5)` has been replaced by `(5 * 2)`, the text of `header.h`
now appears at the top of our code, and so on and so forth.  By making
these translations, the preprocessor simplifies the next steps of compilation.
What are those strange lines that begin with a pound sign [5]?  They tell
the next step of the program where in each file we are, so that when there
are errors, it can report more-or-less accurately where those errors are.

The second major step, at least for the end user, is translation of the
C program into some low-level intermediate language [6].  The intermediate
language is usually then translated into the semi-human-readable version
of the underlying code for the target machine.  This semi-human-readable
code is called "assembly code".  Traditionally, assembly code is stored
in a file with a `.s` suffix.  Let's look at the assembly code for this
program.  We'll use the `-S` flag [7].

    % cc -S program.c 
    % program.c: In function ‘main’:
    % program.c:14:3: warning: incompatible implicit declaration of built-in function ‘printf’
       % printf ("%d,%d\n", DOUBLE(5), foo(DOUBLE(X)));
       % ^

Whoops!  I suppose I should have included the `#include` I elided [8].  But
that's okay, it still produced `program.s`, and we can look at the contents.

            .file   "program.c"
            .section        .rodata
    .LC0:
            .string "%d,%d\n"
            .text
            .globl  main
            .type   main, @function
    main:
    .LFB0:
            .cfi_startproc
            pushq   %rbp
            .cfi_def_cfa_offset 16
            .cfi_offset 6, -16
            movq    %rsp, %rbp
            .cfi_def_cfa_register 6
            subq    $16, %rsp
            movl    %edi, -4(%rbp)
            movq    %rsi, -16(%rbp)
            movl    $46, %edi
            call    foo
            movl    %eax, %edx
            movl    $10, %esi
            movl    $.LC0, %edi
            movl    $0, %eax
            call    printf
            movl    $0, %eax
            leave
            .cfi_def_cfa 7, 8
            ret
            .cfi_endproc
    .LFE0:
            .size   main, .-main
            .globl  foo
            .type   foo, @function
    foo:
    .LFB1:
            .cfi_startproc
            pushq   %rbp
            .cfi_def_cfa_offset 16
            .cfi_offset 6, -16
            movq    %rsp, %rbp
            .cfi_def_cfa_register 6
            movl    %edi, -4(%rbp)
            movl    -4(%rbp), %eax
            imull   -4(%rbp), %eax
            popq    %rbp
            .cfi_def_cfa 7, 8
            ret
            .cfi_endproc
    .LFE1:
            .size   foo, .-foo
            .ident  "GCC: (Debian 4.9.2-10) 4.9.2"
            .section        .note.GNU-stack,"",@progbits

Don't worry!  You don't have to understand that (yet).  However,
when you start learning assembly, you can use the `-S` flag to generate
assembly from C code, and start to get some understanding of the different
instructions.  Even if you don't understand a lot of assembly, generating
the assembly can help you track down some issues in your programs.
For example, the CS faculty were recently discussing a program that
seemed not to be throwing an error when it should.  When we looked at the
assembly, we realized that a function we thought we were calling never
got called.  Why?  Because the result was never used, and the compiler
didn't think it was necessary.  You can even see some of these surprise
optimizations in the code above.  Why does the value `$46` appear?
Because we multiplied 23 by 2, and the compiler could figure out that
that is always 46, and therefore need not be computed at run time.

While assembly code is not particularly readable to the average human
(or even the average programmer), it is not yet at the stage that the
computer understands.  Hence, the next stage is to *assemble* the
assembly code into machine code, the sequences of 0's and 1's that
the computer understands.  Most typically, assembled code is stored
in a file with a `.o` suffix, for "object code".  In large projects,
each source file gets its own object code file [10].  Why?  So that when
you change one source file, you only need to recompile that one file,
rather than recompiling the whole project [11].  You can assemble
(and do all of the other steps) with the `-c` flag.

You might think we're done, at least if you've primarily worked on C
programs that use only one file.  But we're not.  It might be obvious: If
all the code is in different files, we have to put it together somehow.
And we might also have to add in code from libraries.  This last step
is called *linking*.  If you've programmed enough in C, you realize that
you need to tell the compiler what libraries need to be linked (e.g.,
with `-lm` for the Math library).  You don't typically need any flags
for the C compiler when you are linking [14].

So, when you think about building C programs, you should think about all
of these phases: preprocessing, translation, assembly, and linking.  You
may even want to do only some of the phases at times.  We'll see more
about how each phase is useful as the course continues.

Oh, almost forgot!  There are a few other phases that are important,
but less immediately obvious.  The *parser* deals with the syntax of
the language.  It's the part that gives you errors when you forget a
semicolon or an end brace.  The *semantic analyzer* checks that you
get types right (or should check that you get types right), among
other things.  The *optimizer* makes your code faster [15].  We may
come back to those phases later.

One other thing.  As you saw, there are some important compiler flags
that you should know, mostly to limit how far in compilation the compiler
gets.  Competent C programmers know a variety of flags (or at least know
of a variety of flags).  We'll cover some others as appropriate.

---

[1] The preprocessor also replaces constants defined on the command line
with their values.

[2] Competent C programmers may, on occasion, forget that the flag is 
`-E` (probably for "expand").  They do, however, know that there is a
flag and can quickly find it in the man [3] page for the compiler.

[3] "man" is short for "manual".  It is not intended as a gendered
term.

[4] More precisely, replaced by blank lines.

[5] Also known as hash, mesh, sharp, hashtag, and octothorpe, among
other things.

[6] That's not really the second major step from the compiler writer's
point of view.  There is also lexical analysis (breaking the program
text into parts of speech), parsing (combining those parts of speech
into more complex structures), some semantic analysis (e.g., type
checking), and so on and so forth.

[7] I have no mnemonic for the `.s` and `-S`.  Does any of my readers
know why that letter was chosen for assembly?  Perhaps because it appears
twice?

[8] Inserting `#include <stdio.h>` will allow the C compiler to translate
the program without alerting us of potential problems.  But we end up with
exactly the same assembly code [9].

[9] Real C programmers know that even if the output is the same in both
cases, they should do the right thing and make sure that all the warnings
go away.

[10] Okay, that's not quite true.  In many large projects, groups of
object code are often grouped together into libraries.  I guess that
means that each source file still has its own object file, but we stop
thinking of them as such once they are combined into a library.

[11] I've seen at least one prominent computer scientist [12] refer to this
as the "shibboleth of separate compilation".  However, given that I've
watched my machine take over an hour to compile the GNU Image Manipulation
Program from source code, I appreciate that when I change one file,
I don't have to wait another hour.  

[12] Almost certainly C.A.R. "Tony" Hoare in one of the versions of
"Hints on Programming Language Design".  But I can't find that version
on the Interweb.

[14] Well, more precisely, you don't need any flags other than the flags
for the libraries and the list of library locations

[15] In spite of the name, optimizers rarely make your code optimal [16].
But they do usually make it better.

[16] In fact, determining optimality of code is almost certainly as difficult
as the halting problem, and is therefore not computable.  

---

*Version 1.0.2 of 2017-02-06.*
