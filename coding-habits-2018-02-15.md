My coding habits
================

*Warning!  Much of this musing rambles even more than normal, assumes that 
you know programming, and lacks a point.  Skip to right before the endnotes
for something that might be more organized and comprehensible.*

As I mentioned recently, I've [inherited a bunch of
code](coding-reminders-to-self-and-students) that has a number of flaws
that make it hard to use and perhaps harder to update.  In that musing,
I reflected on coding practices that the code reminded me that I should
follow.

Today, I needed to use a separate part of the system.  It didn't work.
Fortunately, this time, the code was written in Java, so I could at least
pretend to understand enough to fix it [1].

I started with a minimalist approach:  Try to run the code on what I thought
was a valid input file.  Find a problem.  Decide whether to update the file
(e.g., remove single quotation marks from strings because the code won't
accept strings that include single quotation marks) or the code (e.g.,
make it accept the single quotation marks).  Generally decide to fix the
code.  Try the code again.  Find the next problem [4].  Decide whether to 
update the input file or the code.

But at some point my code aesthetic kicked in.  For example, I made
sure that code I added did not use "magic numbers".  Unfortunately,
there were a lot of magic numbers in the code.  So I started defining
constants for those magic numbers.  And I have to balance my own sense
of aesthetics with the preservation of the original code intent.  But I
really hate debugging code that smells.

At some point, I felt that I had made enough changes that I decided to 
shove the whole thing in a Git repo so that I could keep track of the
changes [5].  But that meant that I needed to go back to the original
version of the file and start again from there [6].

Starting again from the original file had some benefits.  For example, I
decided that the best start was to comment and arrange the code for clarity.
That would help with some understanding and would also make it more amenable
to other changes.  What were my first set of changes?  Here's the
list from my first significant commit message [7].

* Add section headers [8].
* Reinsert to-do list [9], adding some notes of my own.
* Add procedure comments when they were missing.
* Since this code uses a "debug by print" methodology, updated the
  code to make it a bit easier to turn off or manipulate.
    * Add a `REPORT` procedure [10].
    * Add a level of concern.
    * Print to stderr rather than stdout.
* Some cleanup of text (e.g., prompt is now for tab-separated values).
* Shrink some multi-line instructions into a single line [11].
* A bit of renaming for clarity.  For example, `readFile` is now
  `readScheduleFile`.

Once I made those changes, I went back and reinserted each of the
meaningful changes I had done before putting everything on Git.

But it was hard not to go back and deal with some of the code smells,
or at least to think about dealing with some of the code smells.  
For example, here's one chunk of code.

    // Note: skips to the 15th item in items (in current version)
    index += 4;

    // Sets activity notes
    if (items[index].equals("")) {
      activityNotes = "No notes";
    } else {
      activityNotes = items[index];
    }

What's wrong with this code?  It assumes a particular value for `index`
before the code starts.  It assumes a particular organization of the
table it's reading.  What happens if someone wants to try to rearrange
the code or the database designer decides to change the organization?
They need to find *all* of the places in the code in which the change
makes a difference.  As you might guess, good design suggests that
"all of the places" should be "the one place".  So I went back and 
added the line

    static final int ACTIVITY_NOTES_INDEX = 14;

which meant that I could change the code above to

    // Get the activity notes
    if (items[ACTIVITY_NOTES_INDEX].equals("")) {
      activityNotes = "No notes";
    } else {
      activityNotes = items[ACTIVITY_NOTES_INDEX];
    }

And then I added the instructions for checking size.  Of course, one
might argue that I should have written something more like the following.

    activityNotes = (items[ACTIVITY_NOTES_INDEX].equals(""))
                  ? "No notes"
                  : items[ACTIVITY_NOTES_INDEX];

or even that I should have written a helper, since this is a pattern
of code that gets used again and again.

    static String stringOrAlternative(String str, String alternative)
    {
      if ("".equals(str))
        return alternative;
      else
        return str;
    } // stringOrAlternative

    activityNotes = stringOrAlternative(items[ACTIVITY_NOTES_INDEX, "No notes");

But lets pretend that I left the code as is.  I will admit that I also
have a strong aversion for the "house style" that permits lines like

    } else {

I prefer something as close as possible to GNU style.  If I can't have
that, I prefer something more like

    if (items[ACTIVITY_NOTES_INDEX].equals("")) {
      activityNotes = "No notes";
    } 
    else {
      activityNotes = items[ACTIVITY_NOTES_INDEX];
    }

which allows me to comment the end braces.

    if (items[ACTIVITY_NOTES_INDEX].equals("")) {
      activityNotes = "No notes";
    } // if there are no notes in the table
    else {
      activityNotes = items[ACTIVITY_NOTES_INDEX];
    } // if there are notest in the table

I accept that very few Java programmers use anything like GNU style [12].
So I kept the style as it was, as painful as I found that choice.  I
probably should have used one of the alternate approaches.

At some point, I realized that the program was making a new connection to
the database for each line in the input file (all 200+ lines).  Since I
was running the code using a local database server, each connection
generated a warning.  That made it hard to trace anything.  So I updated
the code to use a single database connection.  That change, plus the
use of my `REPORT` method, made it much easier to see what was happening.

Or at least I thought it did.  The code still didn't work.  So I tried
taking parts of the input file.  And the code didn't work in new and
interesting ways.  It looked to me like it should throw an exception.
But it didn't.  It just stopped at some line in the input.

Eventually [14], I found the problem.  The main processing loop
looked like this.

    Scanner inputData = ...;

    while (inputData.hasNextLine()) {
      line = scheduleFile.nextLine();
      String[] items = line.split("\t");
      int index = 0;
      String activityType = items[index];
      index++;
      String activityDate = items[index];
      ...
    } // while

It seems straightforward, doesn't it?  Guess what?  `java.util.Scanner`
is a finicky beast.  Once I figured out how to step through the program
using `jdb`, I discovered that `input.hasNextLine()` would return false
if it was displeased with an upcoming character (an ñ, in this case).
It's not that it would give up in the line in which the odd character is;
that would be too easy.  Rather, it gives up about eight lines beforehand,
at least in the data I was using.  I detest those kinds of issues [15].

At that point, I just gave up and cheated.  So someone's name is now
incorrectly spelled in our system, using an "n" instead of an "ñ".

But part of me wants to go back and deal with the problem in the code.
What's the problem?  At one level, the problem is that the programmer
decided to use `java.util.Scanner` when that's not the right job for
the purpose of "repeatedly read lines of code until you reach the
end of the file".  I'm not a professional Java programmer, but it
strikes me that that's more of a use case for `java.io.BufferedReader`.

But at the broader level, the issue is that they decided to implement
this program in Java in the first place.  The problem of "repeatedly
read a line of data, break it into constituent parts, and then insert
it into a database" is a problem that is best addressed by a scripting
language.  For example, most scripting languages provide an easy way
to split a string apart and then simultaneously assign the split string 
to a collection of variables  I'd use Perl [15].

    while ($line = <STDIN>)
      {
        chomp($line)
        ($activityType, $activityDate, ...) = split /\t/, $line;
        ...
        if ($activityNotes eq "") { $activityNotes = "No notes"; }
        ...
      } # while
      
If I were in more of a traditional Perl mindset and could assume
that my reader knew some Perlisms, I might write the following code.

    while (<STDIN>)
      {
        chomp;
        ($activityType, $activityDate, ...) = split /\t/;
        ...
        if ($activityNotes eq "") { $activityNotes = "No notes"; }
        ...
      } # while

Maybe I'll rewrite it some day.  Or maybe I'll just pass the slightly
better code on to someone else.  Oh well, let's move on to the next
issues that made my life challenging for a few days.

---

Wait! Didn't I say that this musing was about "coding habits"?  I did.
What does my meandering, diatribe-filled, narrative say about my coding
habits?  Here are some things.

* I have a sense of code aesthetics.  Many things I find unpleasant
  others find equally unpleasant, such as magic numbers or code that
  diperses related logic and concepts.  Some are likely personal
  concerns, such as the commenting of end braces.
* I find it difficult to leave code as it is.  When I see code that
  violates my sense of code aesthethics, I feel a compulsion to change it.
  And once I've changed it, I think of other changes to make.
* I like using a source-code control system.
* Because I'm opinionated, I tend to spend time on issues that affect
  the aesthetics of the program, even if they don't necessarily have
  an impact on the performance or necessarily on my ability to debug
  or improve the code.
* Even thought I tell my students to debug using a debugger, and I
  use debuggers myself, I also have a standard practice for when I want to
  "debug through printing".
* I do try to think about picking "the right language for the job".
  That's a skill I try to instill in our students, too.
* I generally prefer to solve a problem now rather than to hack around
  it.  (I could, for example, have just modified the input text to eliminate
  all single quotes.)  But, when time is running short, I'm willing to
  accept the hack solution.

Damn.  I thought it would be deeper than that.

---

[1] The other system was written in PHP.  I don't really know PHP [2].

[2] The other day, I said "I don't know PHP" to an alum.  They said
"Um, Sam, you taught me a course on PHP."  I had no memory of doing so.
I replied "Didn't you take that from Walker?"  But no, it appears that
[in the fall of 2010](http://www.cs.grinnell.edu/~rebelsky/Courses/CSC325/2010F/Handouts/schedule.html), I taught our "Web software" course and focused
on PHP.  It must have been a traumatic enough experience that I completely
blanked it out [3].

[3] Alternately, I'm just old and forgetful.

[4] The first problem was actually that the database associated with the
program has limits on certain field lengths and the code does not ensure
that the string are of the appropriate length.  The second problem had to
do with the quoted strings.  I'm pretty sure that my combined fix introduced
a new potential bug, but I'm leaving that for later.

[5] No, the original was not in a Git repo.  An early design decision
was to include some passwords in the code.  I decided that I was better
off telling people to update the code before running it than I was to
do without a Git repo.

[6] Perhaps I didn't *need* to go back to the original.  My world view just
suggested that it was the best thing to do.  That way, I could have a much
clearer log of what I'd changed.

[7] That's not quite accurate.  The first thing I did was observe that
there were two essentially identical files in two different packages.
I merged the packages and threw away one of the files.  My, that felt
good.

[8] I find code easier to read when it has big separators for the
different sections, such as the following.

    # +-----------+------------------------------------------------------
    # | Constants |
    # +-----------+
    
    ...
    
    # +-------+----------------------------------------------------------
    # | Types |
    # +-------+

[9] The class comment in the original was a big to-do list.  But it
wasn't clear which things had and had not been done.  And some of the
things mentioned particular events from a certain year.  I trashed that
to-do list.

[10] Here's a variant of my approach to "If you're going to debug with
printing, at least do it in a reasonable way."  In some cases, I also
allow some global constant to specify where the debugging output goes.
I normally do something a bit more robust, but my time was limited.

    /**
     * Report something about the state of the system.
     *
     * @param level
     *   An indication of the importance of the report.  Lower numbers
     *   are more important.  Level should be less than 10. 
     * @param message
     *   A string.
     */
    public static void REPORT(int level, String message)
    {
      if (level <= REPORT_LEVEL) {
        System.err.print("                    ".substring(0, 2*level));
        System.err.println(message);
      }
    } // REPORT

[11] What do I mean by that?  The original had something like

    System.out
            .println("All queries for room have been executed.");
    
I changed that to

    REPORT(2, "All queries for room have been executed.");

If I had not added my `REPORT` method, I would have written

    System.err.println("All queries for room have been executed.");

[12] I'm not sure why, but I've never found that Java programmers are
all that thoughtful about their code style.  Or maybe their code style
is just so far from what I think is reasonable that I find them thoughtless.
Who knows.

[14] "eventually" = "a few hours later".

[15] I was going to say "I detest those kinds of bugs", but there's
a chance that someone, somewhere, doesn't consider this a bug.  It's
just part of the design of `java.util.Scanner`.

[16] If I had not been corrupted by having spent too many years writing 
mediocre Perl scripts [17], I'd probably use Ruby.

[17] Is "mediocre Perl scripts" redundant?  I don't think so.  Even for
Perl scripts, mine are often mediocre.

---

*Version 1.0 of 2018-02-15.*
