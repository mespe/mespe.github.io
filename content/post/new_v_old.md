+++
Categories = ["Stats", "Research"]
Description = ""
Tags = ["stats", "research"]
date = "2017-06-30T19:35:44-07:00"
menu = ""
title = "Moving past p-values"

+++

I had a marathon review/revision session with my adviser last week
regarding a manuscript I am working on - overall took 2 hours!

We were going round and round in circles, and finally 1.25 hours in we
got to the piece of my paper that was really bugging my advisor: I did
not have a single p-value in the entire paper. What followed was a
really interesting exchange that I think highlights some of the issues
that are behind scientific research, especially in agriculture, using
out-of-date statistics.

## The issue

My adviser was trained in the worst of the frequentist school of
statistical thought, namely:

  + p-values are an indication of whether or not an effect is
    "real". If the calculated p-value is < 0.05, the effect is "real"
    and if > 0.05 it is not "real".
	
  + An analysis is made more trust-worthy by excluding data that might
    not be representative, reliable, or both. This exclusion can
    happen after the first round of analysis. Rather than 
	
  + You do not present/discuss non-significant results.
  
I say this is the worst of the frequentist school, though it is not
really frequentist in nature - rather it is a result of the
Null-Hypothesis Significance Testing framework
(See [Andrew Gelman's excellent blog](andrewgelman.com) for many
discussions and examples of the perils of NHST). Similar evils can are
possible under the Bayesian framework. 

By contrast, my manuscript introduces two well documented,
well-supported by theory dynamics/effects, and measures each using a
multileveling framework that averages over uncertainty introduced by
many factors. The estimates and credible intervals are
presented and the implications of each are discussed. There is no
discussion of whether or not an effect is "real", rather the paper
assumes each effect is "real" and measures them. 

It makes sense why my adviser would struggle with this: Under his
framework, 0 is of critical importance because NS results are not
discussed. Under mine, 0 is just another number. 

## Next steps

This is fine and all, but most of the above re-hashes the conversations
on Andrew Gelman's blog as well as countless others
(e.g.,
[Blinding Us to the Obvious?](http://www.blakemcshane.com/Papers/mgmtsci_pvalue.pdf)) -
it is not really new except that it is new to me. For me, what was
before an academic discussion is now very real to me, and I need to
figure out how to proceed. The following are a few observations from
my conversations with my adviser.

  1. **Principles cannot replace rules:** Applied scientists are busy
	 folks, and they need\* a set of rules to guide their use of statistics
	 as a shortcut to learning statistics from first principles. The
	 rules above have worked because there was sufficient weight behind
	 them: Experts said this is how you should operate to find the
	 Truth. By contrast, my arguments that relied on principles did not
	 make much headway. While I originally was quite frustrated by this,
	 the more I think about it, the more understandable it is. As Andrew
	 Gelman has said, applied statistics is all about defaults, and we
	 just need to shift the defaults.
  
  2. **Science moves slowly:** Ken Cassman once told me that science
     moves forward on the backs of grad students - they are the ones
     trying new things, learning the cutting-edge methods, and
     ultimately doing most of the actual research. PIs steady the ship
     (and pay for it) and keep grad students from getting too far into
     the weeds. This is all fine, except that PIs hold the keys to
     publication, and publication is the key to advancement. So there
     is this tension between what the PIs/publications will accept,
     and what is actually modern best practice.

  3. **We need some guidelines to adapt practice:** So there are
	 (understandably) brakes keeping things from moving too quickly, but
	 there is also a need to move science towards using more robust
	 methods. Towards that goal, I think we need some *rules* on how to
	 evaluate new methods. Anyone want to write this?
  
\* "need" might be a bit strong - appreciate?
