---
date: "2019-08-19T10:03:30-06:00"
title: "Future of R (per Hadley)"
authors: ["mespe"]
categories:
  - R
  - data science
tags:
  - R
  - data science
draft: false
---

Quartz had a pretty interesting [interview with Hadley Wickham on the
Future of
R](https://qz.com/1661487/hadley-wickham-on-the-future-of-r-python-and-the-tidyverse/).
While the interview was pretty short, it has many interesting tidbits
about how Hadley thinks about R.

# The problem with the future of R according to the tidyverse and RStudio

Many of the tidyverse developers are employed by RStudio, a
commercial company. It is, in many ways, strange to be asking an
employee of a commercial company about the future of an open source
software. Why did they not interview any of the members of R Core? The
justification given is that Hadley has an out-sized influence on the R
community, which is certainly true. However, aside from a brief
mention of this friction, the interview is largely unquestioning of
this sticky relationship. Why is this a sticky relationship? 

  - The actions of commercial entities, no matter how beneficial, are
    not purely altruistic. After all, they exist to make money. So, the
    future of R according to RStudio is a future in which they can
    make money. Is that true of R Core? 
	
  - Therefore, it is worth asking how RStudio makes money. Thankfully,
    we can go straight to the [horse's mouth on this one](https://rviews.rstudio.com/2016/10/12/interview-with-j-j-allaire/):
	
	>Our open source products can typically be adopted by many, many
    >people without buying our professional products, and that’s how we
    >want it to be. When customers get more serious about R, or they
    >are deploying R in a larger environment, they tend to buy our
    >professional products. 
	
	Notice the phrase "when customers get more serious about R"?
    Presumably, people have been serious about R since long before
    RStudio was around. So why would it be needed here? This is
    mirrored in Hadley's comments... 
	
  - Hadley suggests that R users are not programmers, and should not
    be. He suggests that the tidyverse has benefited users by reducing
    the amount of programming that needs to be known to use R. I
    agree with Hadley that fewer programming concepts need to be known
    to get started with the tidyverse. I am undecided whether or not
    this is purely a good thing.
	
  - The push to make R less "program-y" has contributed to increasing
    the number of people using R by lowering the bar, but with this
    the skill level of the average R user has decreased. For example,
    in my own experience, there are proficient tidyverse users who
    cannot use `[` to subset. 
	
  - This situation is good for RStudio, who sells services to people
    who wish to "get more serious about R." Having a large but less
    technical audience is _ideal_ for RStudio, much better than having
    a small and technical audience. Would RStudio survive in Python,
    Julia, or Go? It could be argued that RStudio benefitted from
    directly from encouraging the idea that R users are not
    and should not be programmers.
	
  - But this begs the question... is this good for R?
  
# What is good for R?

This is a very strange question to ask. R is a tool, and it is either
suited to a task or not. Asking what is good for R is akin to asking
what is good for a hammer. What is good for a chisel? 

Although this seems like a silly comparison, it might actually be quite
useful. There are bins of neglected and abandoned tools in every
thrift store and flea market. How did they get there? Typically, a
handful of factors contributed make them no longer useful and left
them to their flea market fate:

1. They broke: a broken tool is not a useful tool. Some tools can be
   repaired, but many cannot (easily). This includes tools that dull
   easily.
   
2. They were superseded by (seemingly) better technology. Why
   "seemingly"? Often, it takes the test of time to fully see what is
   actually better, and what is a fad. A good metric is to look in the
   toolbox of a working craftsperson - they typically don't have time
   to waste on fads.

3. They were unreliable. Related to tools that break, but slightly
   different. These are the tools that will not hold true, fall out of
   adjustment easily, or have other unpredictable behavior.
   
4. The skills to use them properly became rare. Some of my best tools
   were found in a bargain bin in a shop where the keeper had no idea
   what they were for. Dividers seem like an anachronism, useless in a
   modern world of lasers and micron-calipers. [But to someone versed
   in the traditional methods](https://blog.lostartpress.com/2017/09/08/creating-evenly-spaced-intervals-with-dividers-or-a-sector/), they are indispensable. 

5. They are no longer relevant because the task is no longer
   done. These are the "horse-and-buggy" repair tools. 

So, what does this have to do with R? Well, we can ask ourselves if
the recent developments in R are contributing to any of these
factors. 

1. Tidyverse code tends to be more fragile. This is because: 1) the
   tidyverse developers are less committed to stability and backwards
   compatibility than R Core has been. Hadley has publicly stated
   that he would rather move "forward" and break peoples' code than
   move slower. The most famous example of this is the changes to the
   ggplot2 package. 2) Non-standard evaluation relies on non-standard
   behavior, some of which is not well documented and can behave in
   unexpected ways. 

2. Many argue that the tidyverse represents better technology compared
   to base R. But what about the tidyverse vs. Julia or Python. Here
   the distinction gets muddy. The claim is that the advantages of
   having non-program-y syntax and therefore wider acceptance and use
   is worth the degraded performance. However, the fact that cutting
   edge work is being done in R less and less signifies that there
   might be an issue where R is being replaced by better tools which
   are faster and scale better. Technical people are needed to push
   technology forward, but what happens when the user-base is
   encouraged to be less technical? 
   
3. Related to #1, tidyverse code can be less reliable because it has
   increased complexity. There are a lot more operations happening in
   a tidyverse call - it is not uncommon to have a call-stack 20-30
   calls deep even for simple operations. Despite our best efforts,
   increased complexity typically results in lower stability and
   reliability.
   
4. The tidyverse developers are actively advocating that R users
   should not have to worry about the details, even with basic
   operations such as subsetting or adding elements to a
   data.frame. The art of programming in R is being replaced with
   learning the tidyverse patterns for R.  
   
5. By increasing the audience and diversity of the R community, the
   tidyverse has ensured that it will remain relevant because there
   are just so many more people using R. Hadley makes this point in
   the interview, and I agree - the increase in both the size and the
   diversity of the R community has been incredible. R being the
   domain language of only statisticians and "serious" programmers was
   not good for R's long-term future. This relates to my [previous
   thoughts on the tidyverse](/post/quick_tidy_thoughts/) - namely
   that the community expansion has been an unqualified and immense
   success, and has happened in large part because of the work of
   Hadley, R Ladies, etc.
   
Like much of the world, it is hardly clear cut. Undoubtedly, there
have been massive benefits to R and the community from the
tidyverse. Many people are using R that would not otherwise, and the
world is better off having more people embrace a code-based,
reproducible system. 

But have these benefits come at a cost? Is RStudio and the tidyverse
eroding R's long-term prospects, or have they saved R from obscurity?
Unfortunately, I think only time will tell.


  
