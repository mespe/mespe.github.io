---
date: "2017-10-22T17:09:12-07:00"
categories:
  - data science
title: "Data science on the cheap - pt. 2"
Description: "Can you be a data scientist without dropping $$$ on a computer?"
draft: false
Tags: ["data science"]
<!-- toc: true -->
tags:
  - data science
---

This is the continuation of [Data Science on the
Cheap](/post/cheap_datasci/), where I discussed the potential to
use a Chromebook as a cheap entry to data science. After trying the
main configurations, I concluded that the easiest use case was to use
the Chromebook as a cheap and portable terminal to access a bigger
machine elsewhere. But that leaves you with only half of the
equation - you still need the other machine.

There are two main options here:

  - Cloud service
  - DIY(ish) Workstation

# Cloud services

Cloud services have several advantages:

  1. Pay for what you need/use: Most of these services have a pricing
	 structure where you are billed by the hour, minute, or (thanks to
	 price wars) the second. 
  
  1. Reduced management, administration, and setup: Most services have
     some ready-made images to deploy that include basic data science
     tools pre-installed. Someone else worries about updating the
     images. An added bonus here is you get the advantage of starting
     with a reproducible computing environment (until you start
     messing with things). 
  
  1. Hardware upgrades are made by requesting a different machine:
     Related to the above, you don't have to worry about upgrading
     aging hardware. 
  
There are many of these out there, but the most popular are Amazon Web
Services and the Google Cloud Compute Engine. Of the two, I like
Google Cloud a bit more do to their command-line tools, but really
either will work just fine.

So, what are the disadvantages of these?

  1. You have no control over what the rest of the machine is doing:
     On these services, other jobs on the same physical server can
     impact performance. I learned this the hard way re-running memory
     intensive models on Google Cloud - the same model with the same
     data had massive variance in timing, from 24 hours to 3+ days. 
	 
  1. You do not have complete control over the machine: While you have
     a lot of control, it is not complete. While I have never had
     issues with a Cloud service not allowing the setup I wanted, I
     have heard of cases.
	 
  1. The costs add up: If you are just running small analyses
     occasionally, the cost of renting time on a machine will be much
     lower. However, once you are running an analysis around the
     clock, the costs add up. Most services has a "sustained use"
     discount, but even with these it can sometimes be cheaper to have
     your own. Which leads to...
	 
# DIY Workstation

Thanks to largely stagnant advances in computing technology over the
last 10 or so years, older generation chips often give decent
performance. Additionally, as large tech companies replace aging
components, older tech can be had for 1c on the $. 

The advantages of going this route are you:

  1. have full control over the system, from OS to hardware
     configuration.
	 
  1. will learn about setting up a compute environment, which is a
     handy skill.
	 
  1. never have to share resources.
  
The disadvantages are you:

  1. are responsible for a lot more than just data science. System's
     administration can be a pain, and hardware upgrades can be a
     cascade that leads to replacing a entire system because the
     standards/interfaces change.
	 
  1. never will work with cutting edge tech.
  
  1. shoulder more cost, which may or may not work to your advantage
     vs. renting time.
	 
So, what did I do? I built a workstation, and I am happy I did. Next,
I will explain my setup and share what I learned along the way.

	 
  
