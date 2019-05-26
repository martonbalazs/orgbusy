orgbusy
=======

Analyses how busy you are based on your org-mode style todo list, tells you if
you should panic or rather relax a bit.

You have a report to do by next Thursday (10 hours of work), a paper to read by
Monday (2 hours), prepare for a meeting on Wednesday (1 hour), review another
project by Wednesday (3 hours), you will be away travelling on all day Tuesday,
and you have that large project to be completed 29 days from now (100 hours of
work still left with that one). Now, are you overloaded by these deadlines, or
is it not that bad? 

This script will tell you. It needs an always up-to-date .org file with these
deadlines and amounts of work left in the following format (c.f. Emacs'
org-mode):

```
** <2014-10-23 Thu> Report						 :10:
** <2014-10-20 Mon> Read paper						  :2:
** <2014-10-21 Tue> Prepare meeting for Wednesday			  :1:
** <2014-10-21 Tue> Review project for Wednesday			  :3:
** <2014-10-21 Tue> Travelling all day					  :0:
** <2014-11-12 Wed> Large Project due					:100:
```

(the ** in front of the date is not important, the first "<" is searched for).
Entries with ":0:" tag are considered days on which no work can be done. (Enter
weekends this way manually if you don't work on weekends...) Once you have this
file, use the script as

```
orgbusy [file.org]
```

The script shows you a plan of work-hours per day that

- makes sure all deadlines are met;
- gives you the most uniform workload possible in time with this constraint.

In the output "Now" assumes that today is still available for work, "Evening"
does not count today as a work-day.

If you have Gnuplot installed then it also shows you a graph of your cumulative
workload as a function of number of days into the future. Suggested work-hours
per day are simply the slopes of the green line for the respective days, and
from the graph (or the screenshot in the repo) you'll immediately see what the
algorithm does.

It also decides if

- you should not even touch any of these deadline jobs, because they are too
  far away for their amount of required work, OR
- you should work on your deadline jobs, OR
- you should PANIC, because the daily charge is more than what you can
  comfortably handle.

The two thresholds for daily workload that separate the above three categories
can be set in ~/.orgbusy/[file.oby] in the format

```
"leaveit=1.3"
"panic=4"
```

(in separate lines). If no such file exists, defaults are 1.3 for leaveit and 4
for panic. (Notice: leaveit<=panic/3 implies peaceful weekends under all
circumstances. :-) )

The PANIC warning also comes on if any job is due today. On the other hand it
goes away even above the panic threshold if the "Evening" daily workload
projected for the evening became smaller than the one at the beginning of
today.

Using as

```
orgbusy [delay no of days] [file.org]
```

it looks into the future [delay no of days] many days assuming no work has been
done by that date, but jobs with earlier deadlines are not counted. This way
you can check if you can get a day or two off without much danger. 


```
inorgtask [file.org] [size] [charge]
```

is a scheduler. It tells you on which date, starting from tomorrow, a task of
size [size] can be inserted so that no daily charge is raised above [charge].
Some charges might already be higher than [charge] anyway, those of course
stay, but the newly increased numbers for later dates will not go higher than
[charge]. The org file is not changed. The script silentorgbusy is just an
auxiliary one used here (it just outputs the charges as orgbusy 1).

Finally,

```
orgdateocheck [file.org]
```

is a parser to check if todos are in time-order. I often mixed up the dates of
todos and that came with the danger of missing deadlines. Now this script warns
me if I ever do this again.

I'm no programmer, so please don't blame me on the quality of the code. :-)
Licensed under GNU GPLv3.
