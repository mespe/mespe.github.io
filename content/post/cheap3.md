---
date: "2017-10-28T16:33:23-07:00"
title: "Data science on the cheap - pt. 3"
categories:
  - data science
tags:
  - data science
draft: false
---

This is the last part of the series, and if you have not read the
other two parts([pt. 1](/post/cheap_datasci/),[pt. 2](/post/cheap2/)),
it might be helpful to glance back to see how I got here.

In the last part, I explained the thinking that led me to build my own
workstation using reclaimed parts. Here is brief overview of what I
did:

*Note: this is not intended to be a build guide. If you have no
experience assembling a computer system, please find a beginner's
guide for the basics.*

# Components

Before you run off to EBay to source components, it is worth taking a
moment to evaluate your expected workload, specifically to decide if
it will be CPU, memory, or I/O intensive. In an (overly) simple breakdown:

  - CPU: These operations benefit from faster calculations, and (to
    over simplify) benefit from higher clock speeds on the CPU. You
    want to look for faster GHz ratings on the CPU. 
	
  - Memory: These operations involve moving or altering lots of data,
    and benefit from getting that data to and from the CPU
    efficiently. You want to look for larger cache (L3) and more
    memory bandwidth, more memory channels, etc.
	
  - I/O: These operations involve reading, writing, or moving data
    around on the disk. You want to look for a system that can mount
    several drives in RAID or over faster interfaces (e.g., PCIE). 
	
Again, there are tons of details in each of these, but taking a basic
inventory can help you focus your parts search.

In my case, modeling with Stan is heavily influenced by memory
bandwidth, but the models themselves do not typically use much memory
total. Therefore, I looked for CPUs with large L3 cache, quad-channel
memory, but was not terribly concerned about CPU clock speeds, core
count, or storage. 

# My build

I focused on a dual socket build, using LGA 2011 Xeon parts. Right
now, you can pick up some older CPUs off EBay for around $30 each,
each with 25 MB of L3 cache. Used ECC RAM costs around $1/GB, so for
around $100 you can easily get 32 cores, 32 GB RAM. Given the cost, I
figure if one part dies, it can be replaced for a low sum.

However, the difficulty is in the rest of the build. Server parts are
intended to be installed in servers, not workstations. Therefore,
finding a case, motherboard, PSU ended up being much more of a pain. I
found a dual socket LGA 2011 Intel workstation board for $200
refurbished, a case that could fit the EAXT (extended ATX) motherboard
for $80, and a power supply with the necessary connections to drive 2
CPUs (Seasonic 650 Gold unit) for an additional $90. You read that
right, almost all the other components cost as much as the CPU and
RAM, which are typically what people focus on. Additional parts that
were needed were CPU coolers and a SSD for the OS.

The total cost ended around $600.

# Was it worth it?

For this comparison, I have my daily-driver laptop (Lenovo T460s,
i5-6200U, 20GB RAM) which I purchased for ~$1k vs. the Chromebook
(Lenovo 11e) and workstation combo which cost $770.

## Computing power

Both the T460s and workstation are running Linux, hence UnixBench is
the nature benchmark to compare them.

### Single core

First, the laptop:

<pre>
System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0   30919412.5   2649.5
Double-Precision Whetstone                       55.0       3944.4    717.2
Execl Throughput                                 43.0       3795.4    882.6
File Copy 1024 bufsize 2000 maxblocks          3960.0     979550.8   2473.6
File Copy 256 bufsize 500 maxblocks            1655.0     285555.7   1725.4
File Copy 4096 bufsize 8000 maxblocks          5800.0    2351617.0   4054.5
Pipe Throughput                               12440.0    1990267.8   1599.9
Pipe-based Context Switching                   4000.0     214263.3    535.7
Process Creation                                126.0      12805.6   1016.3
Shell Scripts (1 concurrent)                     42.4       4485.4   1057.9
Shell Scripts (8 concurrent)                      6.0       1337.2   2228.6
System Call Overhead                          15000.0    3544217.3   2362.8
                                                                   ========
System Benchmarks Index Score                                        1510.3
</pre>

Not terrible. How did the $600 workstation with old parts do?

<pre>
System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0   30215728.7   2589.2
Double-Precision Whetstone                       55.0       3271.3    594.8
Execl Throughput                                 43.0       2049.2    476.6
File Copy 1024 bufsize 2000 maxblocks          3960.0    1040357.0   2627.2
File Copy 256 bufsize 500 maxblocks            1655.0     285077.5   1722.5
File Copy 4096 bufsize 8000 maxblocks          5800.0    2856997.8   4925.9
Pipe Throughput                               12440.0    2147832.0   1726.6
Pipe-based Context Switching                   4000.0     167883.1    419.7
Process Creation                                126.0       4187.3    332.3
Shell Scripts (1 concurrent)                     42.4       2245.3    529.6
Shell Scripts (8 concurrent)                      6.0       2451.3   4085.5
System Call Overhead                          15000.0    3600961.8   2400.6
                                                                   ========
System Benchmarks Index Score                                        1286.0
</pre>

So, the laptop actually has better single core performance. This is
not terribly surprising, since the laptop CPU is newer and has a
higher clock.

### Multicore

Again, first the laptop:

<pre>
System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0   77912721.6   6676.3
Double-Precision Whetstone                       55.0      13699.1   2490.7
Execl Throughput                                 43.0      15577.6   3622.7
File Copy 1024 bufsize 2000 maxblocks          3960.0     949210.3   2397.0
File Copy 256 bufsize 500 maxblocks            1655.0     264511.2   1598.3
File Copy 4096 bufsize 8000 maxblocks          5800.0    2902041.8   5003.5
Pipe Throughput                               12440.0    5488727.9   4412.2
Pipe-based Context Switching                   4000.0     933827.9   2334.6
Process Creation                                126.0      33959.7   2695.2
Shell Scripts (1 concurrent)                     42.4      10576.1   2494.4
Shell Scripts (8 concurrent)                      6.0       1571.4   2619.0
System Call Overhead                          15000.0    8306589.9   5537.7
                                                                   ========
System Benchmarks Index Score                                        3201.6
</pre>

And now the workstation:

<pre>
System Benchmarks Index Values               BASELINE       RESULT    INDEX
Dhrystone 2 using register variables         116700.0  456986583.3  39159.1
Double-Precision Whetstone                       55.0      76894.4  13980.8
Execl Throughput                                 43.0      35217.6   8190.1
File Copy 1024 bufsize 2000 maxblocks          3960.0     721736.4   1822.6
File Copy 256 bufsize 500 maxblocks            1655.0     203074.8   1227.0
File Copy 4096 bufsize 8000 maxblocks          5800.0    2356368.2   4062.7
Pipe Throughput                               12440.0   33687117.5  27079.7
Pipe-based Context Switching                   4000.0    5822845.2  14557.1
Process Creation                                126.0      84757.5   6726.8
Shell Scripts (1 concurrent)                     42.4      61021.9  14392.0
Shell Scripts (8 concurrent)                      6.0       8808.9  14681.5
System Call Overhead                          15000.0    4232309.4   2821.5
                                                                   ========
System Benchmarks Index Score                                        7956.9
</pre>

This is really where the workstation shines - notice that the
benchmark is only running on 8 cores concurrently.

## Benchmarks vs. real-world use

The workstation crunches on large Stan models without even breaking a
sweat, even across 16 cores. Due to the memory bandwidth, even simple
models are faster on the workstation compared to the laptop. I want to
try some other computations at it (crop models, etc.), and I want to
throw some GPUs in it for Machine Learning (it has 4 PCIE slots!). Until then,
I can confidentially say that it out-performs any other $600 system I
have seen. 

 *One (important) thing to consider - I am not accounting for
 energy use here. The electricity needed to run the workstation and
 cool the room it is in should be taken into consideration.*
 
# So, what is the verdict?

The Chromebook is surprisingly nice for day-to-day cruising the web,
answering emails, etc. When it comes to writing code, it is OK once
set up (though the setup is not always fun). It is not great for
actually running analyses, but the workstation helps patch up that
deficiency. 

I think if I only had ~$800 to spend, I would lean towards the
Chromebook + workstation set up. But the limitations of the Chromebook
when it comes to actually compiling code and/or running an analysis
can be frustrating. While things might change, Chromebooks just are
not intended for development.

Therefore, given everything I have learned over the last 2 years: If I
were starting all over today, I would pick up a used Thinkpad (a X230
weighs approximately the same as the Chromebook, but with much
more storage, processing power, etc.) and build a workstation for more
intensive work.






