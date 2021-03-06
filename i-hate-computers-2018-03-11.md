I hate computers
================

_Warning!  This musing is both technical and disjoint.  Blame my (mostly)
dead computer._

This morning, the screen on my MacBook stopped working.  Completely.
It's just black.  I tried rebooting.  No luck.  I tried attaching an
external monitor.  No luck.  It's a particular pain because I have a
lot of work to do on the computer today. [1] It's hard to work without
a screen.

I looked the problem up online.  I found [a helpful page on OS X
Daily](http://osxdaily.com/2014/11/22/fix-macbook-pro-booting-black-screen/).
I tried resetting the System Management Controller.  No luck.  I tried
resetting the PRAM.  I could see the screen for a moment during the boot
process, but then it went off again.  At this point [2], I decided that
it was more than just a software issue.  

But I had not backed up in about two days, and I'd done a lot of work in
those two days.  I also know that it's sometimes a pain to get things off
of a backup for another Mac.  So I thought about what else to do.

My first goal was to log in.  It seems that's possible.  I typed my username,
a tab, and my password.  I clicked the "Play" button, and iTunes started.
So I know I was logged in.  I then typed command-space and "terminal" to
get to the terminal.  I verified that I was in my terminal by typing

    say Hello

I'm glad the Mac has a `say` command.  

So the next question was how to make copies of certain folders.  Some
experimentation led me to the following series of commands.  They are a
lot of fun to type without seeing.

    scp -r /Users/rebelsky/directory-to-copy /Volumes/Backups/Emergency/; say Done
    diskutil list | grep Backups | say
    diskutil eject /dev/disk3 2>&1 | say

If you care, `2>&1` is the bash command-line option to redirect standard
error to standard output.  It's among the least intuitive commands I've
used recently.

Of course, typing those commands also required that I know what directories
I wanted to copy.  Fortunately, judicious use of `ls` and `say` helped me
identify some of the more important ones.  I'm now waiting to see whether
the copying of some of the bigger directories worked.

I have limited time right now, so I'm not going to write much more.
Suffice to say that it's also [a pain to switch back to an old laptop.
Among other things ...

* Applications want to update themselves.  Lots of applications.  Particularly 
  Microsoftware.
* It takes awhile to download new email messages [3].
* My disk utility software wants to rescan my old laptop.  And rescan it
  again.  Then it puts up scary warning messages [4].
* And, of course, I need to [transfer my digital workspace](transferring-digital-workspace-2018-01-04).  Since my "old" laptop was in use two months ago, I'm going to wait to worry about that until spring break.

One more thing: **I hate computers.**  But I'm glad that I know enough to
figure out how to deal with a screen-less computer, at least for a time.
Next up: Figuring out whether my third-party warranty company will fix
the damn thing.

---

Postscript: Here's the really sad part.  After I did all of that, I
shut down my old MacBook from the terminal with the command

    sudo shutdown -h +1

I then reset the SMC one more time.  This time, the computer booted
with a working screen.  So it will be difficult to send it in for repair
and say "My computer screen is not working".  Did I mention that I hate 
computers?

And, of course, a minute after I typed that, the computer screen went
blank again.  It appears that computers hate me as much as I hate them.

---

Postscript: Did I mention that I got essential *no* work done today?

---

[1] At this point, it's more of the a situation in which I *had* a lot
of work to do today.  That work did not get done.

[2] "At this point" = "after about two hours of futzing around".

[3] Since Mail.app is not happy about running on multiple machines at
once, I needed to quit it on my dying Mac.  Once again, command-space,
terminal, and say were my friends.  I used command-space to get to Mail.
I crossed my fingers that Ctrl-Q would work correctly.  I used `ps`
in the terminal, along with `say`, to figure out if I'd succeeded.

[4] It turns out that the messages were easy to misread.  I thought
I was getting a SMART error from my disk, which is a sign that the disk
is likely to die soon.  But it turns out I just had some damaged files
(which I had downloaded from somewhere).

---

*Version 1.0 released 2018-03-11.*

*Version 1.1 of 2018-03-14.*
