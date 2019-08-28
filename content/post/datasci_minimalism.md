---
date: "2019-06-03T11:42:51-06:00"
title: "Data Science Minimalism"
authors: ["mespe"]
categories:
  - data science
tags:
  - data science
draft: false
---

When I got my first taste of data science, it was intoxicating. With a
few lines of code, I could visualize and explore data and make some
inferences. I was drawn to data science by the possibilities it represented. The
data science toolbox represented a key to unraveling the data I found
all around me. Learning how to use these tools was exciting and made
me feel powerful and capable. 

These days, I am much less excited. It has been difficult for me
to peg down exactly why. This is my feeble attempt to catalog what I
think is going on - imperfect and bumbling as it may be...

# What tools do you actually need?

In my spare time, I am a woodworker. My workshop is fairly spartan -
all my tools fit on a wall-mounted peg board and a half-height
bookshelf. I have no power tools. Most of my tools were manufactured
long before I was born, and the "newer" ones are based on old
designs. There are no pocket jigs, special router tables, or fancy
hardware. 

When people hear this, the most common response is "How can you do
that?" or "Are you a glutton for difficult work?" But the truth is,
this way of working is in many ways more efficient, safer, and results
in furnature meant to last a long time and be easy to
repair. 

The merits of my minimalist tooling are many:

  - Give me a saw, a chisel, and a hammer and I can build just about
  anything. Most work can be boiled down to requiring a handful of
  essential skills and knowledge about the mechanics of wood. I don't
  need a special jig or tool for every new piece of furnature I
  build. I don't need the latest, greatest creation of the power tool
  makers. In my workshop, skill and knowledge take the place
  of complex tooling.

  - I have less than $300 in all my essential tools. Contrast this with the X
  "must have" power tools recommended for beginner woodworkers - any one
  of which costs more than my entire collection, plus extra for each
  specialized tool/attachment needed for each new surface, joint, and
  furnature type. It is not uncommon for people to spend thousands on
  woodworking tools. 

  - By reducing the tools available, I focus more on what is needed
  for each piece and step. With complexity, it is easy to lose sight
  of the goal. Worrying about what tool/bit/fastener I need for a
  project never happens - instead I am concerned with the proportions
  of the piece, the characteristics of the stock I am using,
  etc. Minimal tooling brings the important stuff to the forefront.

# Ok, but what does this have to do with data science?

In data science, I see many parallels. Data science has become
dominated by a glut of hype around the latest tools, frameworks, and
various helpers. Twitter is full of people trying to sell you on the next big
thing for data science... the language/library/etc. that is going to
revolutionize data science as we know it. At least once a week, I get
asked about some new package which promises to "fix", or worse
"transform" how I work with data. 

People constantly come to me for help with R, but 98\% of the time,
they know very little about R. Instead, they are tied in knots over
the details of some package. However, it is shockingly common for them to
struggle with the seemingly inocculuous question "What are you trying
to do?" It is easy to get so caught up with "How does dplyr work?"
that you forget to ask "Should I be using dplyr?"

Data science in this landscape of hype is an overwhelming, never-ending
slog. But most importantly, it distracts from learning the key skills
and foundational knowledge needed to really become proficient. Hence a
proliferation of "expert beginners" in the R community. Many people
can tell you how to pipe a bunch of functions together, but far fewer
can tell you general strategies for working with messy data. Many
people know about package X, but few are experts in working with data
more generally. 

# So, what do you need?

I will admit that I am somewhat of an outlier. My development environment, is very
simple. I use R, but I don't use Rstudio, the tidyverse, etc. I work
with R in Emacs, but even there I use a very minimal set of
packages. And in R, I rarely use more than a handful of packages. My
day to day work leans hard on the basics of the language. 

Most data science, from cleaning to modeling to visualization can be
broken into three basic tasks:

1. Divide data 

2. Apply an operation, possibily looping over divisions

3. Combine data

Of course there are numerious details hidden in each step, but most
tasks boil down to just that. 
