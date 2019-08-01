---
date: "2019-03-19T09:39:47-06:00"
title: "Why workshops are tough to teach"
authors: ["mespe"]
categories:
- data science
- R
tags:
- data science
- R
draft: false
---

A couple of weeks ago, I taught a one-hour workshop on the basics of writing
functions in R. Overall, it went as well as I would expect an hour introduction
workshop to go, which is to say OK but not great. Why not great? It
turns out teaching skills in workshops is really difficult. But why?

I think it boils down to a couple of aspects of workshops:

Workshops:

1. are too short
2. teach to the lowest skill level in the room
3. don't reflect how you actually learn these skill
   
# 1. Workshops are too short
	
Ask around, and you will find that almost everyone loves the short
format of workshops. Life is busy and time is precious. Asking for a four
week commitment is a lot, but ask for one hour out of someone's
afternoon easy. 

However, what can you cover in an hour?
	
Turns out, not much. When I teach, I typically assume that you can
drive home one minor idea or concept per hour of instruction. As for
major themes or ideas, I figure you can convey 4-5 in a typical 3
credit hour course. After talking to many other instructors and
professors, I don't think I am alone in this assessment. 

So, if we assume we can get a single minor concept across in an hour
workshop, what is the issue? We know that we are not going to teach
all of R, but giving an introduction to writing a function should not
be an issue, right?
	
Well, the trouble comes in with #2...
`
# 2. Workshops teach to the lowest skill level in the room

Let's say we are teaching the single skill of "writing R functions"
during a workshop. It is a simple task, well within the criteria of
"one minor idea per hour". 
	
But as we give the talk, it turns out there are a range of experiences
in the room. It turns out some people have not really been introduced
to functions at all, so we cannot teach how to write functions without
giving a brief overview of what functions are. Some people have never
been introduced to R's method for using and finding variables, so we
need to also introduce that. Related to that, most people don't know
about scoping, and get confused about how variables could be local to
a function, or why global variables are a bad idea. Lazy evaluation?
Debugging? The list goes on and on...
	
When I teach a class, I can start at the fundamentals. Once we get to
writing functions, the students should be comfortable with the basics
of the R language. The foundation has already been built, so adding to
it is an easy task.
	
With workshops, all bets are off. 
	
If you look at most workshop series (e.g. Software Carpentry
workshops), they just don't bother to teach "fundamental"
skills. *(Note, do not confuse "fundamental" with "basic" - fundamental
skills are skill which form the foundation for deep understanding.)*
But, the reason this becomes an issue is ...
	
# 3. Workshops do not reflect how you actually learn a skill
	
Find an expert in some data/software skill, and ask them how they
became an expert. I would bet good money that none of them became an
expert by attending workshops or reading a book. Why? Because to
transition from novice to expert, you need to apply the skills to a
non-trivial problem. You need to hammer away at something that is
difficult, work through issues with the tool and your understanding of
how it functions, and come out the other side with hands-on experience
to be an expert. It is the reason why most companies give problem sets
to new data science hires. Many people attend workshops, few know how
to use the skills in practice.

But you cannot solve a difficult problem in a workshop. Doing so would
require time that you just don't have. Instead, in a workshop, you are presented
with a skill and you might get to see a toy problem or two, but you never
actually apply the skill. It is extremely common for workshop
participants to leave feeling good about "learning" a skill, only to
be completely unable to apply it in practice. 

So, what's wrong with that? Well, when you tackle a difficult issue,
you need the fundamental understanding. You need to be able to reason
about what the language is doing and why. Otherwise, you end up in the
extremely common workflow of "I have this script, and I will modify it
with half-educated guesses until it appears to work". And yes, I have
seen many many workshop attendants in this mode of working. It doesn't
really work. 

How do I know this? Because the only reason I see these people is that
eventually these workshop "orphans" reach out to me for help, having
exhausted the resources provided by the workshop.
	
# So, why are workshops so popular?
	
I have heard many reasons, including time constraints and ease of
scheduling. But I think there might be a deeper issue at play.
	
Workshops have been created to optimize the wrong thing. Since it is
probably impossible for a workshop to teach fundamental or deeper
understanding, most workshops aim to produce two things: 1) capable
beginners and 2) a sense of accomplishment. Most participants of
workshops are happy at the end. They feel empowered. And there is
nothing wrong with this, per se. But we also should not delude
ourselves into thinking workshops will produce a cohort of skilled
data scientists.




