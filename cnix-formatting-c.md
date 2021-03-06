Formatting C code
=================

**Part of an ongoing series of essays tentatively entitled _Don't embarrass
me, Don't embarrass yourself: Notes on thinking in C and Unix_.**

Programming languages, like most natural languages, have a syntax that
describes how you make up the various kinds of utterances.  In English, we
have sentences of various forms.  In C, we have many kinds of statements,
as well as blocks and control structures, and more.  Both C and natural
languages also have conventions about how you format the text you write.
In particular, when I write in English, you assume that I will include
spaces between words, try to make my lines of more-or-less equal length,
put no spaces before the ending period in each sentence and put space
after the ending period [1]. 

In general ,you
&nbsp;&nbsp;
would
&nbsp;&nbsp;&nbsp;
not
like
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
it if the spacing
&nbsp;&nbsp;&nbsp;
between
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
words
were 
inconsistent.You also would
&nbsp;&nbsp;&nbsp;
not appreciate it   
if  
line breaks  
appeared
&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;
&nbsp;&nbsp;&nbsp;
in odd positions
&nbsp;&nbsp;&nbsp;
.And  
&nbsp;&nbsp;&nbsp;
you would probably appreciate it  
even less  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
if the left edge of the  
&nbsp;lines were
&nbsp;&nbsp;&nbsp;
ragged  
.Nonetheless,you could likely  
&nbsp;read what I had written
.  Itmightjustrequireextraeffort [3].

While the custom of formatting sentences is fairly consistent, there is
some variance in how people format other pieces of text.  Some people
insert spaces between paragraphs; some do not.  Some people indent
the first line of a paragraph; some do not [4].  Americans tend to put
punctuation inside quotation marks, even if it's not part of the quoted
material.  Europeans and coders do not [5].

Just as there is variance in how people format their English prose, there
is also variance in how C programmers [6], even experienced programmers,
format their C code.

Let's consider a procedure that parses a string to identify an integer
contained therein. Here's the documentation.

    /**
     * Convert a string to a long, which it stores in *lp.  
     *
     * Returns
     *   0, upon success
     *   1, if presented with the empty string
     *   2, if the string includes non-digit characters other than initial
     *      whitespace
     *   3, if the string represents a value outside the bounds of longs
     *   4, for other errors
     */

Here's an example of how you should *not* format the code.

    int str2long (char *str, long *lp) { long result = 0; long sign = 1;
    while ((*str != '\0') && (isspace (*str))) str++; if (*str == '-') {
    str++; sign = -1; } else if (*str == '+') { str++; } if (!*str) return
    1; while (isdigit (*str)) { long increment = sign * convertDigit
    (*str); if ((sign == 1) && (result > (LONG_MAX - increment) / 10))
    return 3; if ((sign == -1) && (result < (LONG_MIN - increment) /
    10)) return 3; result = result*10 + increment; str++; } if (*str !=
    '\0') return 2; *lp = result; return 0; }

So, how should you format your code?  Most importantly, you should
format your code for *clarity*.  Your first goal in formatting code
is to make it easier for the reader to understand what you are doing.
For example, indentation helps clarify blocks.  A related goal is that
you should format the code *consistently*.  If your indent size is two
spaces in one part of the program, your indent size should be two spaces
elsewhere in the program [7].  Here's a consistently formatted version of
the previous program, using some customs that I do not like [8].  I won't
describe the customs used because I don't generally use them for C.

    int str2long( char *str, long *lp ) {
      long result = 0;
      long sign = 1;
      while ( (*str != '\0') && (isspace (*str)) ) str++;
      if( *str == '-' ) {
          str++;
          sign = -1;
      }
      else if( *str == '+' ) { str++; }
      if ( !*str ) return 1;
      while( isdigit( *str ) ) {
          long increment = sign * convertDigit (*str);
          if( (sign == 1) && (result > (LONG_MAX - increment) / 10) ) return 3;
          if( (sign == -1) && (result < (LONG_MIN - increment) / 10) ) return 3;
          result = result * 10 + increment;
          str++;
      }
      if( *str != '\0' ) return 2;
      *lp = result;
      return 0;
    }

What happens when multiple people are working on the project?  You need
to choose a *code formatting standard* and stick to it.  Most places you
work will have a "house style" that you should follow.  In fact, in an
ideal world, you would format your code *automatically*, taking the
guesswork out of the process to achieve consistency [9].

In this course, we will use a slight
variant of the [GNU Standards for writing C
code](https://www.gnu.org/prep/standards/html_node/Writing-C.html),
particularly [the standards that focus on
formatting](https://www.gnu.org/prep/standards/html_node/Formatting.html#Formatting).
Why those standards?  Well, for a long time, a lot of the C code I read
and wrote was associated with the GNU Image Manipulation Program (GIMP),
and GIMP is written to follow those standards.  I also find the standards
sensible.  

What are the slight variants?  Having dealt with way too many
problems having to do with mismatched braces, I like to see a comment
associated with every end brace that indicates what you are ending.
Because I know that different people set their tabs differently, I
discourage you from using tabs for indentation.  Spaces may take up a
bit more disk space, but it's little enough that you can ignore it.

You should be able to read the linked documents to learn those standards.
Here are some of the key features that I will note.

* We indent by two spaces.  Why two?  It's big enough to be visible, but
  small enough that we can indent fairly deeply and still fit the code
  on one 79-character line.
* Open and close braces are on lines by themselves.  
* We put spaces between a function name and its parameters, and between
  control structures (e.g., `if`, `while`, `for`) and their parenthesized
  control information.
* In function declarations, we put the function name in the leftmost
  column.  Why?  That makes it *much* easier to find the declaration.
  As you might expect, we also put the opening and closing braces in
  the leftmost column.

Fortunately, you can achieve GNU style by using the command `indent -nut
FILE.c` [11].  Emacs is also pretty good about maintaining C style.  The
one difficulty in using `indent` is that it doesn't put the closing comments
for end braces where I'd like them [12].

Here's the same code, indented using the GNU style [14].

    int
    str2long (char *str, long *lp)
    {
      long result = 0;
      long sign = 1;
      while ((*str != '\0') && (isspace (*str)))
        str++;
      if (*str == '-')
        {
          str++;
          sign = -1;
        }
      else if (*str == '+')
        {
          str++;
        }
      if (!*str)
        return 1;
      while (isdigit (*str))
        {
          long increment = sign * convertDigit (*str);
          if ((sign == 1) && (result > (LONG_MAX - increment) / 10))
            return 3;
          if ((sign == -1) && (result < (LONG_MIN - increment) / 10))
            return 3;
          result = result * 10 + increment;
          str++;
        }
      if (*str != '\0')
        return 2;
      *lp = result;
      return 0;
    }

Finally, here's the same code with the comments I would expect to see.

    int
    str2long (char *str, long *lp)
    {
      long result = 0;              // The computed intermediate result.
      long sign = 1;                // The sign of the value.
    
      // Skip over whitespace
      while ((*str != '\0') && (isspace (*str)))
        str++;
    
      // Check for the sign
      if (*str == '-')
        {
          str++;
          sign = -1;
        }
      else if (*str == '+')
        {
          str++;
        }
    
      // Sanity check
      if (!*str)
        return 1;
    
      // Read all of the digits
      while (isdigit (*str))
        {
          long increment = sign * convertDigit (*str);
          // Upper-bound check
          if ((sign == 1) && (result > (LONG_MAX - increment) / 10))
            return 3;
          // Lower-bound check
          if ((sign == -1) && (result < (LONG_MIN - increment) / 10))
            return 3;
          // Update the result
          result = result * 10 + increment;
          // And move on to the next character
          str++;
        }                           // while
    
      // Sanity check
      if (*str != '\0')
        return 2;
    
      // I think that's it.
      *lp = result;
      return 0;
    }                               // str2long

As you may have noted, one disadvantage of GNU-style formatting is that
it tends to create more lines of code.  But I find the additional lines
make it easier to read, and so I think it's worth it.  Given how easy
it is to achieve GNU-style formatting, I expect to see all submitted
code use that style of formatting.  At worst, you can write using
whatever style you prefer, make a copy, and then use `indent -nut` to
turn it into something I'll accept.

---

[1] How much space goes after the period is a complex issue.  In
typeset manuscripts, I believe that the norm is about 1.5 times the
space between words.  Since that wasn't possible on most typewriters, 
people who type usually put two spaces at the end of a sentence.  In
contrast, most Web browsers seem to put one space after the period [2].

[2] I assume that's because it's actually computationally difficult to
determine whether a period serves as the end of a sentence or in some
other form.

[3] When writing for the Web, it is also harder to write inconsistently
formatted text.

[4] Ideally, designers make these decisions, and make it thinking about
the impact of the different approaches.  But designers are people, too.

[5] I'm clearly too provincial to know what people not from America and
Europe do with punctuation and quotation.

[6] Yes, programmers are people too.  When I write "programmers", I 
generally mean "the subset of people who program".

[7] There are, of course, certain exceptions to this rule.

[8] Why am I using a style that I do not like?  Just so that we have
something to compare.

[9] Those of you who have taken one of Grinnell's Scheme/Racket courses [10]
know that we often rely on DrRacket's "reindent" feature when we're looking
for possible errors in your program.  That's because indentation (formatting)
tells us something about the likely behavior of the program and because
DrRacket will format for us automatically.

[10] That should be all of you.

[11] Do not try to run `indent` on more than one file at a time.

[12] Some day, I'll write a quick utility to fix that issue.

[14] I created it using `indent -nut` and the ugly code
we started with [15].

[15] In reality, I'd written the original code without `indent`.  My code
looks essentially the same.  I then removed most of the inessential
whitespace to achieve the ugly version.


---

*Version 1.0.1 of 2017-02-06.*
