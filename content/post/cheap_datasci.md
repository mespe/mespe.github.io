---
categories:
  - data science
title: "Data science on the cheap"
Description: "Can you be a data scientist without dropping $$$ on a computer?"
date: 2017-09-12T19:46:04-07:00
draft: false
Tags: ["data science"]
toc: true
---

One thing you will notice pretty quick if you hang around data-sci
folks is they tend to pack pretty spendy hardware. During workshops,
it is not uncommon for our classroom at the [UC Davis Data Science
Initiative](dsi.ucdavis.edu) to be packed full of Macbook Pros and
other $1K+ laptops.

Recently my daily driver ([Lenovo
T460s](https://www.notebookcheck.net/Lenovo-ThinkPad-T460s-Core-i7-WQHD-Ultrabook-Review.161028.0.html))
needed to go into the shop for a little work, which gave me a chance
to play with my backup - a [Lenovo Yoga 11e
Chromebook](https://www.notebookcheck.net/Lenovo-ThinkPad-Yoga-11e-Chromebook-Review.124664.0.html),
which I picked up for $160 shipped. This little guy is (in my opinion)
one of the better chromebooks out there - nice bright screen, good
keyboard, great battery life, folds to tablet-mode, etc. It's not
perfect (BayTrail cpu, a bit heavy), but I have been happy with it as
something to cruise the web around the house.

But with the daily-driver away, the question was: Would I be able to
use it for my day to day data-sci work, or do you need to drop some $$
to play the data-sci game?

# Background

Growing up, my mom always told me we didn't have much, but we always
had enough. Not much meant we never had fancy computers or other
electronics. Our first family computer was a big deal, and we used
that same computer for 9 years.

One of the things I love the most about data science is that it is
empowering - most tools and knowledge are open source, and there is a
strong ethos of tackling problems using whatever is at your disposal.

However, you need a computer of some sort for data science.

There is a second concern - I have been traveling a lot lately, and
traveling with an more expensive machine makes me nervous. There is
something attractive about traveling only with things you can easily
replace (that was my rule in China and NZ). Hence, the Chromebook
default of strong disk encryption and the ability to remote wipe the
disk is an additional bonus.

# My requirements

My day to day work is primarily done in R via Emacs-ESS, so at a bare
minimum I needed access to these. Additionally, the data sets I am
currently working with are all >1.5GB, so I needed some space, both in
memory and disk. The more "exotic" work I do involves spatial data sets
often tens of GB large, and these are out of reach for the Chromebook
for sure.

All my work is hosted on Gtihub/Bitbucket, and some depends on forked
C/C++ libraries, so some basic build tools (gcc, git, autconf, make,
etc.) will be needed.

# The setup

There are a few routes commonly recommended for programming on a
Chromebook:

  - [crouton](https://github.com/dnschneid/crouton): a tool that
    allows you to run Linux in a chroot environment alongside the
    normal Chrome browser.
	
	This works (mostly), and has the advantage of giving you access to
    a full Linux desktop experience and the repos. But it also is a
    bit more resource intensive (not good with limited resources), and
    can be pretty buggy when switching between windows. Due to the
    limited resources of the 11e, I had to run the XFCE desktop, and
    even then had issues.
	
  - Native development: This involves running everything inside the
    Chrome shell and building tools in Chrome. For this,
    [Chromebrew](https://github.com/skycocker/chromebrew) allows easy
    setup of the basic build tools, and then everything else can be
    built from source (more on this later).
	
	This actually worked pretty well. Instead of switching from Chrome
    to Linux, you switch between tabs in Chrome. All the tools work as
    expected.
	
	The disadvantages, though started to pill up early... Not having
    access to repositories means manually dealing with dependencies
    for every build. Some pieces ended up being a huge pain (e.g., X11
    and plotting capabilities for R). Emacs was fairly easy to setup,
    but then some key-binds were masked by ChromeOS (C-w was the most
    annoying). The Chromebook feels snappy
    cruising the web, but compiling software takes ages.  
	
  - SSH terminal: This path involved only setting up SSH for Chrome,
    which is [incredibility
    easy](https://chrome.google.com/webstore/detail/secure-shell/pnhechapfaindjhompbnflcldabbghjo?hl=en),
    and then having another machine to SSH into (e.g., personal
    workstation, AWS, Google Cloud Compute). While not necessarily
    "cheap", it might be more cost efficient than buying and
    maintaining your own machine.
	
	This might be the easiest and best case for working on a
    Chromebook - it leverages the strengths of the Chromebook (strong
    network, good battery life, etc.) while not trying to push it into
    a full development machine. Some annoyances persist (keybindings
    from ChromeOS), but it is by far the least painful (if you are
    comfortable working over SSH).
	
# Conclusion 

All in all, the Chromebook is a great machine to use solely as
terminal to SSH into a bigger machine. If you are working over SSH
anyway, you get some added benefits of security. However, without
internet, you are dead in the water - but this is not always a bad
thing (I am using flights to catch up on reading). This will probably
be my traveling setup going forward.

However, I was frustrated early and often when trying to use it as a
stand-alone development machine. It constantly felt like I was trying
to push the Chromebook into being something it was not. If I needed a
stand-alone machine, I would not go this route. Instead, picking up a
used X220 or X230 and running coreboot and Linux would give a far more
usable machine for about the same money.

