Standard targets in Makefiles
=============================

You've just received a project from someone else, and it has a Makefile.
What should you make?  I suppose you could look through the Makefile
for information, and that was what folks once did.  However, these days,
most Makefiles have a few standard targets [1].

The first target in your makefile is the default target.  It is what
gets made if you execute `make` without any parameters.

Traditionally, the first target is `all`, and indicates the primary
products of your project.  For our sample project, we might make the
library our default target, or we might make the application our default
target, or we might make both the default targets.

   all: libmathlib.a gcd

By this stage in your career, you should know that it's important
to test code, even code you've received from elsewhere.  Why might
a C program break as you transport it from system to system?  Well,
you might have different versions of libraries, or missing libraries,
or who knows what else.  Hence, projects typically ship with a small
test suite for you to run.  For the [Math library project](cnix-libraries),
our test is `test-gcd`, but we don't want to force the installer
to read the Makefile to discover that.  We rely on the standard
`check` target

    check: test-gcd
            ./test-gcd

What else?  Well, we want to put the code we generate somewhere for
everyone to use.  (Okay, that's not always true.  But for many
packages, you want to eventually copy it to a standard location.)
The target for installing software is, not surprisingly `install`.
We often set the directories for installation in the variables at
the top of the file.

    INSTALL_BIN = /usr/local/bin
    INSTALL_LIB = /usr/local/lib

    ...

    install: default
            install gcd INSTALL_BIN
            install libmathlib.a INSTALL_LIB
      
What are those `install` commands?  Well, `install` is a standard
Unix utility that helps you install things in typical fashion, setting
ownership and making intermediate directories and such.  

So, where were we?  We have three standard targets: The default target,
`all`, which runs when we just type `make`.  The `check` target, which
we use to ensure that we have made code that works correctly.  And the
`install` target, which we use to install the software we've made.
Because these three targets are so typical, most C programmers,
upon downloading a new project, know to use the following sequence
of instructions

    make
    make check
    /usr/bin/sudo make install

I will note, however, that with the advent of modern package management
systems, like `apt`, it is often more convenient to just use `apt`. 
Still, it's useful to know (and use) these target.

Are there other useful targets?  There are three others that I commonly
use.

As you've probably seen, a typical C project creates a lot of cruft,
particularly `.o` files.  Since different projects create different
cruft, it's useful to have a target that lets you clean out the
project-specific cruft.  That target is `clean` [3].  Since we are
removing files, `clean` usually has no dependencies.

    clean:
            rm -f *.o

Are those `.o` files the only things we've created in building the
project?  No.  We've also created the executable and the library.
If we want to get back to a pristine distribution, we should also
remove those files.  The target for "clean up everything" is `distclean`.

    distclean: clean
            rm -f libmathlib.a
            rm -f gcd
            rm -f test-gcd

Finally, we often want to package together everything into a
tarball for distributing to others.  That target is `dist`.

    dist: mathlib-gcd.c mathlib-str2long.c Makefile mathlib.h test-gcd.c gcd.c
            rm -rf mathlib-0.1
            mkdir mathlib-0.1
            cp $^ mathlib-0.1
            tar cvzf mathlib-0.1.tgz mathlib-0.1
            rm -rf mathlib-0.1

Of course, all of this would be better if we used variables.

    PROJECT = mathlib
    VERSION = 0.1
    PROJDIR = $(PROJECT).$(VERSION)
    TARBALL = $(PROJECT).$(VERSION).tgz
    SOURCES = mathlib-gcd.c mathlib-str2long.c gcd.c
    HEADERS = mathlib.h
    TESTING = test-gcd.c
    MORESRC = Makefile
    PRJDIST = $(SOURCES) $(HEADERS) $(TESTING) $(MORESRC)
    
    dist: $(PRJDIST)
            rm -rf $(PROJDIR)
            mkdir $(PROJDIR)
            cp $^ $(PROJDIR)
            tar cvzf $(TARBALL) $(PROJDIR)
            rm -rf $(PROJDIR)

As you might expect, similar steps are common enough in Makefiles that
programmers may often use tools to build their makefiles.  However, at
this stage in your career, you are better off writing your Makefiles
from scratch.

Are there other common targets?  Certainly.  You can read more about
many of them in the GNU documentation at
<https://www.gnu.org/prep/standards/html_node/Standard-Targets.html>.

How many of these targets do you need?  Do you really need all six [4]?
Not always.  Not everyone needs a `dist` target.  These days, we use
`git` for many of our distributions anyway.  If you're not building code
for others to install, do you need `install`?  Probably not.  But even
if you don't use these targets in your own Makefiles, you should know
that they exist and how to write them.

For your own projects, you will find certainly it helpful to have the
other four targets (`all`, `check`, `clean`, and `distclean`).  Make it
your practice to create those targets whenever you create a Makefile.
Also make it your practice to use variables, as appropriate.

---

[1] In fact, a large number of Makefiles are no longer generated by hand;
instead they are generated by Automake.  Automake is a subject for another
day [2].

[2] More precisely, Automake is also a subject for another course.

[3] Yes, it may seem strange that we have a target that we use to
*remove* files, rather than creating them.  But you'll get used to
it.  It makes sense if you realize that while many targets are
files, others are *actions*.

[4] Plus others listed in that link?

---

*Version 1.0 of 2017-03-01.*
