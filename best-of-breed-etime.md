Best of breed?
==============

I am told that the College has a policy wherein we focus on buying the
"best of breed" software for each administrative office. However, whenever
I deal with one of our major software systems on campus, I wonder what
that phrase really means.

Consider, for example, our electronic time card system, eTime [1]. I
would expect that such a system would (a) make it easy for people to
enter their hours, (b) make it easy for supervisors to confirm those
hours, and (c) make it easy to extract data for analytics. As best
I understand it, eTime is good at none of those things. Let's consider
what I mean.

It's been awhile since I've helped students with eTime, but when I did,
I felt sorry for them. It relies on an old version of Java (or it did),
which makes it a security risk. It won't let you enter consecutive
shifts; I recall one student spending an hour trying to figure out why
they kept getting an error message when they logged one shift as 7:00-9:00
p.m. and another as 9:00-11:00 p.m. The answer turned out to be that
you can't have two shifts that include the same time (in this case, 9:00).
But the system won't tell you that you can't do that. It just issues
a vague error message.

You might hope that it's better for supervisors, but I hear that it's
equally bad. Here's the biggest pain: Because there are four different
student wage categories, if your students are not in the "typical" wage
category, you have to manually switch them to the correct wage category
*for every single shift that they work*. How's that for a good use of
manager time [2,3]?

I've asked whether we can just set a default for each student/department
pair and I've been told that that's not possible, since the system
isn't really designed for people who work multiple jobs and we want to
be careful that we don't overpay students.

Two down, one to go. What about analytics? Well, here's a question that I
recently asked: Can I get the shift data for all of the CS peer educators
for the 2016-17 academic year?  Why would I want such data? Well,
as I mentioned in [an earlier musing](budgeting-screwups-2017-07-11),
I managed to significantly overspend our peer educator budget. I'd like
to know why. I'd also like to be able to better predict what our spending
will be in future years.

I asked our ASA for the data. Although she approves all of the hours,
she cannot get the compound data. So she asked our accounting office.
After a few weeks, they sent us a spreadsheet with the total number of
hours each of our peer educators worked for the whole academic year.
That doesn't help much, since students worked different jobs each
semester. So a few weeks later, I got totals for each semester. One
of the first things that was obvious from those data is that people
work very different numbers of hours for the same job.  That makes
me want to look more deeply into the individual shift data.

Now, it should be simple to get the data I need.  In pseudo-SQL,
the query would be something like this.

    SELECT id,name,starttime,endtime,hours,rate FROM timelogs
      WHERE department=1234567;

But I'm told that there's no automatic shift report for all employees
in a department.  So if I want to know what shifts our employees worked,
someone in accounting has to separately pull shift data for each of our
seventy or so employees.  That's not a good use of their time.

My ASA can produce a report for everyone in our department, but it's
as a PDF, rather than as a spreadsheet.  And it's poorly formatted.
It also includes all of the jobs the students worked, not just the ones
in our department.  "Can you get it in Excel or CSV format?"  "No."
"Can you restrict it to just the hours worked for our department?"  "No."
"Can you do all of our peer educators?"  "Yes."

So, I'm compromising on what I get.  I'm getting the data I need,
along with some data that I don't need.  I'm not getting it a
particularly usable form, but I'm getting something.

But boy, I'd expect better from "best of breed".

---

[1] I'm not sure what it is officially called. It may just be the 
"ADP portal".

[2] Or anyone, for that matter.

[3] One of my manager friends says "I dread the biweekly email prompt."

---

*Version 1.0.2 of 2017-08-01.*
