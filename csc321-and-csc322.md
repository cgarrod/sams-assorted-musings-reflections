CSC 321/22 — The evolution of Grinnell's software design curriculum
===================================================================

Software design is a core part of a strong undergraduate CS education.
However, in my time at Grinnell, I've seen every faculty member who
teaches software design struggle with the course.  I know I have.  Why?
It feels like a bit of a Frankenstein's Monster of a course, combining
a variety of topics both theoretical and practical.  What topics?
We look at big picture methodologies (e.g., waterfall, agile) and detailed
practices (e.g., refactoring).  We consider concepts (e.g., modular design
or collaborative work) and particular tools (e.g., UML or git).  We try
to make sure that students learn their responsibilities as professionals
[1].  And, on top of that all, we have a large group project.

We've tried multiple approaches to the course.  Different kinds of
projects, different texts, different languages.  It's hard.  I've seen the
same approach for the course work very well one semester and incredibly
poorly the next semester.

About five years ago, my colleague, Janet Davis, decided to spend her
sabbatical completely redesigning our software design course.  She made
some bold, but important, decisions.

* She split the concepts and tools (CSC 321) from the practicum (CSC 322);
  each is now a separate two-credit course.  While we encourage students
  to take both in the same semester, they may take CSC 321 one semester
  and CSC 322 a subsequent semester.
* She decided that we should take on projects large enough that it was
  unlikely that they would be completed in a single semester [2].  Students
  therefore get the experience of leaving an incomplete project for another set 
  of students or of inheriting a project from a previous group [3].
* She partnered with nonprofits in town to come up with useful but
  non-mission-critical projects.
* Splitting the course allowed her to develop a model in which 
  students may take CSC 322 more than once.  The hope was that some of
  the repeaters would come back to the same project and could serve as
  team leaders [4].
* She decided to use an xMOOC/SPOC [5,6] to provide many of the core learning
  materials.  In particular, she drew upon the [Berkeley CSC 169](https://courses.edx.org/courses/course-v1:BerkeleyX+CS169.1x+1T2017SP/course/) course and
  the associated Fox/Patterson textbook, [_Engineering Software as a
  Service: An Agile Approach_](http://www.saasbook.info/).
* She recruited an alumni mentor for each project.
* She involved our office of Careers, Life, and Service in helping students
  understand their particular strengths and skills, either through a
  Myers-Briggs test and debrief or through a similar StrengthsFinder
  test and debrief.

It's a wonderful concept for a pair of courses.  I particularly appreciate
that the structure of CSC 322 means that students are working on at
a variety of "soft skills". They need to communicate regularly and
actively with a real, busy, nontechnical client.  They need to learn
to work as a team on a complex project [7].  They need to learn how to
communicate with a remote manager/mentor.  And they need to regularly
communicate about their progress to the class as a whole.  In addition,
they have to think about their predecessors and successors on the project.

Both courses also put students in a situation they are not necessarily
comfortable with: They have to work on things in which there isn't always
an answer or sufficient guidance in the textbook [8].  Some students take
well to that environment; others don't take enough responsibility.  As I
tell my students, "You get out of these courses what you put into them."

During her first year teaching these new courses, Janet got recruited away
from Grinnell to found a new CS program at Whitman College.  That meant
that one of the other faculty needed to take on the courses.  I seemed to
be the member of the department least unsuited to teaching the courses,
so I took them on.  It's been a struggle [9].  But it's a worthwhile struggle
since I do believe that the courses serve an important role in our
curriculum.

We've learned a lot along the way, which has led to some changes in the
course.  I expect it will continue to change.

* It's hard for students to build the project while learning a new language
  (Ruby), a new platform (Rails), new approaches, and new concepts.  Given
  that we want students to be able to take both CSC 321 and CSC 322
  in the same semester, I've restructured CSC 321 so that it's a half
  semester course.  That means that the first half of CSC 322 focuses more
  on requirements gathering and really understanding the project.
* To have enough time for learning tools and techniques in CSC 321, we 
  moved the consideration of ethical and professional responsibilities
  from CSC 321 to CSC 322.  Students also suggested a better reason for
  the switch: "We should think about our ethical responsibilities in the
  semester in which we have to live out those responsibilities."
* The Berkeley course seems to assume a full semester's worth of
  work for a regular course, which makes it hard for a half-semester
  full course (or a full-semester half-course).  The Berkeley
  course is also popular enough that when students search for
  answers to problems, they get solved solutions, rather than the
  related-but-not-exact kinds of solutions that would put them in the
  proximal zone of development.  I've tried alternate approaches to get
  students to learn Ruby and Rails, particularly using Michael Hartl's
  [Ruby on Rails Tutorial](https://www.railstutorial.org/).  But that
  has also proved problematic.
* I've discovered that students need more practice with basic
  "object-oriented thinking".  I'm working on ways to
  incorporate Sandi Metz's [_Practical Object-Oriented Design in
  Ruby_](http://www.poodr.com/) into the courses.  It looks like it will
  work better in CSC 322 than CSC 321, but I'm not quite sure what to
  do about repeaters.
* I consider Design Patterns an appropriate part of CSC 321 and CSC 322.
  But I've found that what students find in the book
  and on the Interweb is too imprecise.  I've decided
  to include a chapter or two of the original [_Design
  Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) in future
  offerings of the course.
* In the first year I taught the course, I learned that students had
  trouble finding time to work together as a team outside of class.
  Since I had switched CSC 321 to a half-semester course, I decided to
  expand the in-class time for CSC 322 during the second half of the
  course.  In the second half of the semester, students do almost all
  of their CSC 322 work in class (five or six hours per week in class,
  one or two hours per week outside of class).
* I've dropped both Myers-Briggs and StrengthsFinder.  From what I can
  tell from the Psychology literature, neither is particularly reliable
  [10].
* Three-hour periods seem too long for students (and for faculty) [11].

There's still more to do.  But we seem to be moving in the right direction.

In making these changes, we've also changed the time-slot-structure of
the course.  I count about five different models.  Let's see.

* *Model 1* from Fall 2014.  CSC 321 met in two one-hour slots
  (TuTh 10:00-10:50).  CSC 322 met in a three-hour slot (Th 1:15-4:05).
* *Model 2* from Spring 2015.  CSC 321 met in a two-hour slot
  (Tu 2:15-4:05).  CSC 322 met in a three-hour slot (Th 1:15-4:05).
* *Model 2b* [12] from Fall 2015.  CSC 321 met in a two-hour slot
  (Tu 2:00-3:50) for the 
  full semester.  CSC 322 met in a three-hour slot (Th 1:00-3:50) for the 
   full semester.  
* *Model 3* from Spring 2016.  CSC 321 met in three one-hour slots
  per week (MWF 1:00-1:50) for the first half of the semester.  CSC 322
  met in one three-hour slot (Th 1:00-3:50) for the full semester.
* *Model 4* from Fall 2016 and Spring 2017.  CSC 321 met in three
  one-hour slots per week (MWF 1:00-1:50)
  for the first half of the semester.  CSC 322 met for three
  one-hour slots per week (2:00-2:50) for the first
  half of the semester and three two-hour slots (1:00-2:50) for the 
   second half of the semester.
* *Model 5* from Fall 2017.  CSC 321 met in two two-hour slots
  per week (TuTh 8:00-9:50) for the first half of the semester.  CSC 322
  met in two one-hour slots per week (TuTh 10:00-10:50) for the first
  half of the semester and will meet in two two-and-a-half-hour slots
  (8:30-10:50) for the second half of the semester [14].

Model 4 is clearly the best.  But it does require six in-class hours per
week of faculty time [15].  Our current enrollments suggest that we'll
need two sections per semester, or perhaps three sections per year.

And I'm about to go on leave.  That means someone else will be taking over
the course.  I wouldn't want a faculty member teaching two sections of
CSC 321/22 plus a third course [17].  I'm not sure that we'd be able to
find someone other than me willing to do so [18].  And it's also not a
workload I consider appropriate for our early-career faculty.  So it's
time for some rethinking and retinkering and renegotiating.

Where will the course go next?  We'll see.  I look forward to departmental
discussions.

---

[1] We work on making sure that they consider ethical and professional
responsibilities throughout the curriculum.  But software design is where
they look most seriously at the ACM Code of Ethics and other related
documents.

[2] I did say that she is brave.

[3] Which group would you prefer to be in?

[4] We think the ability to repeat the course is important enough that
we've designated it as one of the options for the "Research Opportunities
for All" initiative.

[5] xMOOC: Massive, Open, Online Course.  SPOC: Small, Personalized,
Online Course.

[6] While the popular press uses "MOOC", scholars of learning at
scale and online education prefer to distinguish the two models of MOOCs
as xMOOCs and cMOOCs.  cMOOCs are collaborative (hence the "c") whose
focus is people working together to help each other learn.  xMOOCs, in
contrast, are based more on the traditional lecture plus assignment plus
some kind of discussion section.  While cMOOCs are conceptually better,
xMOOCs seem to be the more popular model.  Collaboration is hard.

[7] At least it feels complex to them.  And it is, for new software
developers.

[8] Welcome to real life.

[9] I don't regularly program in Ruby or use Rails.  My alums in the
Rails community say that it takes about two years of full-time Rails
programming to become comfortable with Rails.  So I'll never be really
comfortable with Rails.  I'm also busy enough that juggling the various
contacts (alumni mentors, CLS, community partners) is really hard.

[10] I realize that many of us cling tightly to our Myers-Briggs identities.
But it does sound like these are transitory enough that it may not
be worth promoting them in class.  And StrengthsFinder is painful to
take.

[11] I realize that in the workplace, three-hour shifts are not surprising.
But I expect that most workplaces find time to break up even three-hour-long
shifts.  I've certainly seen the students be less productive once we
hit the third hour.  And when it's a lecture/discussion class, rather
than primarily group work, it's even worse.

[12] The time slots for class periods changed between spring 2015 and
fall 2015.  Otherwise, model 2 and model 2b are the same.  

[14] Given that I knew that a three-hour period was a bad idea, why did
I return to that model for Fall 2017?  Because I had no open teaching
slots on MWF.  My students report that 3-hour classes on Tuesday and
Thursday mornings are a bad idea.  I'm returning to model 4 for
spring 2018, although with the times shifted slightly to avoid a conflict
with CSC 341.

[15] I worry less about the six in-class hours per week of student time.
A four-credit course should require twelve to fourteen total hours per
week, between in-class and out-of-class time [16].  A two-credit course
should require six to seven total hours per week.   So, for CSC 322,
students have more in-class time and less out-of-class time.

[16] That'ss a large amount.  But it's what College policy
states.

[17] I realize that our Studio Art faculty regularly have to teach three
six-hour courses per week.  I think that load is also inappropriate. I've
said so to administrators and external reviewers.

[18] While I've been willing to teach one six-hour "course pair" per
semester, I would not be willing to teach two.  Given recent experiences,
I'm not sure that I'm still willing to teach one any more for only one
course credit [19].

[19] Dear Grammarly [20]: "one course credit" is correct.  The adjective
"one" modifies "course credit".

[20] Yes, I still ask Grammarly to review the occasional musing, particularly
the longer musings.  While I ignore much of the advice, it does catch some
things that I don't catch myself.

---

*Version 1.0 of 2017-10-21.*

