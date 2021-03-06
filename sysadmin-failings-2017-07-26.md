Among the reasons SamR is not a SysAdmin
========================================

*Warning!  Technical issues ahead!*

Although I worked as a system administrator while in grad school [1]
and I administer my own machines, I generally do not administer machines
that others use.  Why not?  Because I don't think like a SysAdmin.  Let's
explore today's adventure to see what that means.

This week, I am running a [data science code
camp](code-camp-article-2017-07-24.md).  We are having the students use
[Jupyter](http://jupyter.org) to explore various data sets.  To make
things work better [2], we've set up a JupyterHub server.  Because we have
a server, the students should be able to access their work from anywhere.
I think there are also some collaboration features.

But someone has to run the server and that someone is me.  Setting things
up wasn't bad.  Dealing with the conflicts between Apache (already
running on our VS [3]) and JupyterHub was interesting, but something
I could handle.  I was too lazy to remember how to start a service at
startup, so I just started a `tmux` session to run the JupyterHub server.
All seemed well and good.

And then today, someone asked me to install a new Python package.  That
should be easy.  I normally type `/usr/bin/sudo pip install PACKAGE` [4]
to install Python packages.  But this time, I got an error.

<pre>
$ sudo pip install demakein
The directory '/home/administrator/.cache/pip/http' or its parent directory is not owned by the current user and the cache has been disabled. Please check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
The directory '/home/administrator/.cache/pip' or its parent directory is not owned by the current user and caching wheels has been disabled. check the permissions and owner of that directory. If executing pip with sudo, you may want sudo's -H flag.
Collecting demakein
  Downloading demakein-0.16.tar.gz (298kB)
    100% |████████████████████████████████| 307kB 1.6MB/s 
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/tmp/pip-build-kbtu5q7_/demakein/setup.py", line 10
        exec f.readline()
             ^
    SyntaxError: invalid syntax
    
    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /tmp/pip-build-kbtu5q7_/demakein/
</pre>

You may note that I was careless in my command ... I did not type the full
path to `sudo`.  That's okay; it's a fresh system and I'm pretty confident
that I get the right `sudo`.  I have in the past.  You will also note that
there are way too many error messages.  The first set seem to be because
it's attempting to store things in the administrator account.  That's 
strange.  But there's also the problem executing `readline()`.  It struck
me that I might have to address the first issue by running the command
directly as root.  So here's what I tried next.

<pre>
$ sudo su
# cd
# pip install demakein
</pre>

I no longer got the directory ownership message.  However, I continued
to get the same message that follows `Collecting demakein`.  I did a
quick Web search for some ideas, made a few changes, and then tried 
to run the command again, without thinking carefully.

<pre>
# !s
</pre>

Can you tell what happened?  Let me help you out a bit.  `!s` means
"repeat the last command that starts with the letter s".  I was thinking
of the `sudo pip install demakein` command that I typed.  But I wasn't
running the command under the administrator account, I was running the
command under the root account.  Can you guess what the last command
someone executed as root was that started with `s`?

Any ideas?

I would have thought it was something like `su apache`, because I was
doing some work on the Web server [6].  But I'd done that through `sudo`.

Well, it turns out that the last "s" command was `shutdown`.  No,
not `/sbin/shutdown`.  Just `shutdown`.  So my `!s` [7] shut down the
server immediately.  With all of our campers logged in.  And it's a
virtual machine, so I had no idea how to reboot it [8].

Are there morals to this story?  Certainly.

* Don't attempt changes to a system at critical times if you can avoid it.
* Don't rely on command history when you're in root mode.
* Try not to do work in root mode.
* Don't try to install stupid Python packages when your son asks you to.
* Don't hire me as your SysAdmin [9].

---

[1] And yes, that's one of the reasons I use `vi` rather than Emacs.

[2] Or at least I thought it would make things work better.

[3] If a VM is a virtual machine, a VS should be a virtual server.

[4] `su` is normally the `switch user` or `super user` command.  `sudo`
is an alternative that allows you to do do some root tasks, but not all
of them.  I was taught to prefix important commands with their full
path name, so I try to type `/usr/bin/sudo` rather than just `sudo`.
`pip` is the Python package installer.

[5] I did not choose the id "administrator" for the non-root admin
account.  But I seem to be stuck with it, at least until I decide to
spend some time playing with the system.

[6] Apache is a Web server.  On some installations, the Apache account
is `www`.  On some systems, the Apache account is `httpd`.  On some systems, 
the account is `apache`.

[7] Pronounced "bang s".

[8] I knew to call ITS.  And the person I talked to knew how to reboot
the machine.  But they also had no idea how I could reboot the machine
myself.  Maybe that's why JDS likes physical servers.

[9] I probably shouldn't post this musing to LinkedIn.

---

*Version 1.0 of 2017-07-26.*
