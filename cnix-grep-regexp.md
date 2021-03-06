`grep` and regular expressions
==============================

**Part of an ongoing series of essays tentatively entitled _Don't embarrass
me, Don't embarrass yourself: Notes on thinking in C and Unix_.**

At some point in your career, you will realize that you have enough
files that you don't know where some things are.  It may be as simple
as wondering where a procedure is defined amongst a few dozen [1] source
files.  It may be as complex as trying to remember where you took notes
on a particular topic.  It may even involve digging something out from
someone else's files.

Fortunately, there's a standard Unix tool for finding text in files.
It's called `grep` [2].  You write `grep PATTERN FILES`, it shows
you everywhere in the files that the pattern appears.  For example,
if I want to see everywhere I use the procedure `str2long`, I'd write
`grep 'str2long' *.c` [3].  If I wanted to see where my email appeared in
a bunch of files, I'd use `grep 'rebelsky@grinnell.edu' *` [4].

Of course, we sometimes want more complicated patterns.  For example, if
I want to figure out where a procedure is defined, rather than where it
is used, I probably want the one place in which the procedure name appears
at the start of a line [5].  In `grep`, we use a caret (`^`) to represent
the start of a line.  We might want to handle multi-word phrases in which
one or more spaces can appear between words.  We might want to handle
variants of the same word.  

A long time ago [6], computer scientists [7] were looking at ways to
model language and computation.  Stephen Kleene [8] developed a set
of languages called the "regular languages" as well as a structure for
describing them, known as "regular expressions".  When you take a course
in automata and formal languages, you'll learn a lot more about regular
languages and regular expressions, including how to write machines that
match them quickly.  In any case, regular expressions have now entered
the common toolset of most computer scientists and computer programmers.
Of course, because we can't agree on anything, there are multiple
syntaxes for regular expressions, some closely related and some not so
closely related.  We will explore the basic concepts and syntax of 
regular expressions as they are used with `grep`.

* The simplest regular expression is a sequence of "regular" characters
  [9], such as numbers and letters of the alphabet.  A sequence of 
  regular characters matches the same sequence of characters in a file.
* A few special characters match positionally.  Caret matches the
  beginning of the line.  A dollar sign (`$`) matches the end of the line.
* What happens if you want to match a special character as a character,
  and not in terms of its special meaning?  You precede it with a 
  backslash (`\`) [10].
* Backslash before some normal characters can also have meaning, as in
  C.  For example `\t` is a tab.
* If you want to allow any of a collection of characters, you enclose
  that collection in square brackets.  `[aeiou]` matches any lowercase
  vowel.  Similarly `[aeiou][aeiou][aeiou]` matches any sequence of
  three lowercase vowels. You can also use a dash in a collection to
  indicate a range.  For example, `[a-z]` represents the lowercase
  letters.
* If you want to allow any single character, you use a period, `.`.  
  So `sm*g` matches `smug` and `smog` and even `smmg` or `sm g`.
* If you want to allow zero or more repetitions of a pattern, you can
  follow the pattern with an asterisk [11].  For example, `d*` matches
  zero or more repetitions of the letter `d`, and `mad*` matches
  "ma", or "mad", or "madd", or "maddd", and so on and so forth.
  Some forms of regular expressions use `+` for "one or more repetitions".
  Unfortunately, it does not seem like the standard Linux `grep` does so.
* To do grouping (e.g., for repetition), you surrounded the group with
  backslashed parens.  For example, `^\(no\)*$` matches lines with zero
  or more repetitions of `no` with no intervening spaces.  Hence, it
  will match the empty line, and a line containing just "no" or the
  Human Beinz's "nononononononononononononononononononononononononononononono'.

As you get more experienced with regular expressions, you will discover
other forms of regular expressions and the subtleties of the syntax
of each implementation.

Where were we?  Oh, yes, we were talking about `grep`.  You should also
know some of the command-line flags for `grep`.  Here are the ones I
use most frequently.

* `grep -l` lists the files that match, but not the particular lines in
  the file.  I find this particularly useful as input to another command.
* `grep -v` prints only the non-matching lines [12].
* `grep -n` adds line numbers.
* `grep -i` ignores case.

There are also many others.  You can read about them on the man page.

Once you've developed some basic skill with `grep` and with regular
expressions, you'll find that you use it regularly to search your
files.  It should make you more efficient.

---

[1] Or a few hundred, or a few thousand.

[2] [Wikipedia](https://en.wikipedia.org/wiki/Grep) tells me that 
"[i]ts name comes from the ed command `g/re/p` (globally search a regular
expression and print)".

[3] `*.c` stands for "all files ending with dot c".

[4] `*` stands for "all files in the current directory".

[5] I know that the procedure name appears at the start of a line because
I format according to GNU standards.

[6] Okay, in the 1950's or so.

[7] They may not yet have called themselves such.

[8] My great-grand advisor.

[9] As we'll see, a few characters, such as the caret we already examined,
are designated as "special characters".

[10] And yes, if you want to match a backslash, you must precede it
with a backslash.  

[11] That asterisk is typically called the "Kleene star".

[12] I had never previously tested `grep -vl`, but it appears to list
the files that do not contain the pattern.

---

*Version 1.0 of 2017-01-16.*
