---
date: "2020-11-10T14:27:08-07:00"
title: "Workstation Update"
authors: []
categories:
  - data science
  - computers
tags:
  - data science
  - computers
draft: true
---

Way back in 2017, I built a cheap data science rig for $600. You can
read about the build [here](../cheap3/). Since that post,
I added a mid-tier NVIDIA GPU for machine learning, and some
hard drives for more storage. Otherwise, it's been pretty bullet-proof
for the last 3 years.

While it was a great solution for what needed at the time, eventually
it's shortcomings were increasingly obvious. Those were:

- Efficiency (or lack there of): 16 cores of 2012-circa Xeon
  processors pulled roughly 200W on their own. Add in the other
  components, and I measured 500-600W draw from the wall when it was at
  100%. While not terrible, it would definitely heat up the room. Nice
  in winter, but a bummer in summer. When the computer
  lived on campus in a room with industrial climate control, it was not
  noticeable. In my 100 sq ft office, it was very noticeable. When I
  had weeks of work to run on it, I saw it in our electricity bill. 

- Noise: The motherboard I used was meant for a Intel workstation with
  chassis sensors to monitor temperatures. Without these present, it
  used a sensible-ish default of running the CPU and system fans at
  100%. The workstation sounded like a small airplane, even at zero
  load. While fine initially and great for thermals, it got old.
  
- Size: To house the dual socket E-ATX motherboard, I needed a large
  case. Very large. Approximately 60L large. On campus, I had tons of
  room, so it was never an issue. In my small home office, I was
  acutely aware of just how big it was.
  
- Performance: The Xeon processors I used were no slouches for 2012,
  and for a time CPU performance gains were minimal so there was no
  real incentive to update. Even when I bought them for $30 in 2017,
  their performance still held up. Then, AMD got their Zen processors to
  market, and everything shifted. The Xeons started to show their age
  with heavily multi-threaded workloads. Benchmarks showed my 16 Xeon
  cores below AMD's 6-core Ryzen 5 3600 in both single and multi-threaded
  workloads, all while using 140W less.
  
- COVID-19: Wait, you might be wondering, how did COVID affect your
  workstation? In the before times, I highly valued mobility - I often
  would work from coffee shops, the library, etc. Most of my work was
  done on a laptop, and I only used the workstation for big jobs. With
  COVID, suddenly I was spending all my time at home. I have a dock
  for my laptop, but then I got the worst points of laptops
  (relatively poor performance) without the benefits (mobility). I
  yearned for working at the desktop. However, the above points made
  it less pleasant if not outright difficult.

So, I decided to update the workstation. The goals are a smaller,
quieter, more efficient, more powerful workstation. More details
soon. 
