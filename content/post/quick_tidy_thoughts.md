---
date: "2019-07-24T15:38:18-06:00"
title: "Quick tidyverse thoughts"
authors: ["mespe"]
categories:
  - R
tags:
  - R
draft: false
---

There has been a lot of chatter about R and the tidyverse as of late -
some of it prompted by Norm Matloff's posts
[here](https://github.com/matloff/TidyverseSkeptic) and
[here](https://github.com/matloff/R-vs.-Python-for-Data-Science).

Below you will find some quick thoughts on the topic. These are
half-baked at best, and probably not fit for general consumption. This
is mostly a way for me to get my thoughts down on the page.

1. The tidyverse has increased the approachability and accessibility of
   R to a large audience. Many claim this is due to more "natural"
   syntax (something I, frankly, don't understand). There are many
   people who either would never have tried R or gave up on base, only
   to be brought back into the fold by the tidyverse.
   
2. Part of #1 is due to social aspects of the tidyverse. The tidyverse
   is heavily promoted on social media (e.g. Twitter), at social
   gatherings (e.g. UseR), etc. The tidyverse crowd have been known to
   be open to new-comers from all walks of life. This is a huge
   benefit to the overall R community.
   
3. An aspect of the tidyverse's dominance of the social landscape has
   been helped by backing from a for-profit organization (RStudio),
   which has staff to promote their work over that of base R.
   
4. The "old guard" of R are not active on social media and do not
   heavily advertise their work. They appear to prefer to let their work
   speak for itself. Therefore, in many online communities, there is
   no voice advocating for base R.
   
5. Many people feel that the traditional means of learning R was not
   accessible, hostile to newcomers, etc. The message boards (R-help,
   R-devel, and StackOverflow to a lesser degree) are seen as not
   suitable to anyone but expert users. It is perceived, prehaps
   rightfully, that beginners get mocked, harassed, etc. on these
   platforms (e.g. "search before posting", "read the docs",
   etc.). Whether true or not, this perception has actively deterred
   newcomers from learning base R.
   
5. People love the tidyverse for these reasons - it opened (and
   welcomed) them into the world of R and data science. Furthermore,
   the tidyverse represents not only a computational tool, but a
   community to many. This can be seen in R Ladies, etc.
   
6. Due to the above, people using the tidyverse are understandably
   defensive of it, especially from perceived attacks from the "old
   guard". An attack on the tidyverse is seen as going beyond an attack on the
   computational model, extending to be seen as an attack on
   inclusivity, accessibility, and the community.
   
7. However, the tidyverse, at its core, limits users in some
   significant ways. It dictates that the data must be in a
   data.frame, that it be ammenable to the "tidy" workflow. It also
   introduces a separate evaluation scheme which makes writing custom
   functions more difficult. This makes users relient on the functions
   provided by the tidyverse. 
   
8. (My opinion) Well-meaning though it may be, the tidyverse is
   patronizing. A central tenant of the tidyverse appears to be
   "users cannot be bothered with learning detail X" - in fact Hadley
   has said as much in
   [interviews](https://www.propublica.org/nerds/hadley-wickham-your-default-position-should-be-skepticism-and-other-advice-for-data-journalists#). By
   contrast, base R tells users "learn X, and then R is your
   playground". While I believe this to be unintended, a side-effect
   of this is that the tidyverse developers have become the
   gate-keepers of what is possible in R. Any functionality missing in
   dplyr, etc. is seen by its users as not possible. 
   
9. This gives rise to the paradox that the tidyverse increased R's
   impact, in part by limiting its users' power in exchange for
   convienence. However, since this is tied to the social aspects of
   the tidyverse, it becames difficult to objectively assess whether
   this trade-off is worth it.
   

   
   
   
