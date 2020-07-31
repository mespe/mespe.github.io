---
date: "2020-05-20T07:31:15-06:00"
title: "Extract a number from a string with base R"
authors: ["mespe"]
categories:
  - R
tags:
  - R
draft: false
---

This problem came across my desk yesterday, and I thought it was
interesting and straightforward.

The problem is extracting a "group" number from the middle of a
string. The strings looked like:

```r
strings = 
c("Group_1.Pooled", "Group_1.Sample_1", "Group_1.Sample_2", "Group_1.Sample_3", 
"Group_2.Pooled", "Group_2.Sample_1", "Group_2.Sample_2", "Group_2.Sample_3", 
"Group_3.Pooled", "Group_3.Sample_1", "Group_3.Sample_2", "Group_3.Sample_3", 
"Group_4.Pooled", "Group_4.Sample_1", "Group_4.Sample_2", "Group_4.Sample_3", 
"Group_5.Pooled", "Group_5.Sample_1", "Group_5.Sample_2", "Group_5.Sample_3"
)
```

So, what is the "base R" way to do this? Turns out there are many...

# substring

Since the strings all start with the same bit, and our number of
interest is always in the same position (position #7), we can use `substring()`

```r
substring(strings, 7, 7)
```
```
 [1] "1" "1" "1" "1" "2" "2" "2" "2" "3" "3" "3" "3" "4" "4" "4" "4" "5" "5" "5"
[20] "5"
```

# strsplit

The strings all have separators surrounding the number, so we can
split the string, and then extract the part we are interested in

```r
sapply(strsplit(strings, "_|\\."), "[[", 2)
```
Notice that we wanted to split on a literal `.`, so we escaped it with
`\\`.

# regmatches()

The two above rely on the strings being in a set pattern. If the
string were not so consistent, we can use `regmatches()`. Since the
number we are interested in is always the first number in the string,
we can use `regexpr()` to find its position (verses `gregexpr()` which
would find all the matches, not just the first one).

```r
regmatches(strings, regexpr("[0-9]", strings))
```

# gsub()

We can just gsub away all the parts we are not interested in, i.e.,
the part before and including the `_`, and the part after the literal
`.` to the end,

```r
gsub("^.+_|\\..+$", "", strings)
```

# gsub() w/capture group

We can also utilize capture groups in gsub to extract the part that we
want,

```r
gsub(".*_([0-9])\\..*$", "\\1", strings)
```

Notice that I specify that the number is surrounded by `_` and a
literal `.` - this might be overkill. However, this is the solution I
offered because knowing about capture groups together with `gsub()` is
crazy powerful.

# So why are there so many different ways to do this? 

Can't base R get its act together and just provide a single function?
Preferrably with a [verb name](http://r-pkgs.had.co.nz/style.html).

Actually, each of the above are doing slightly different things, and
one might be better suited than another, depending on the situation.

Also, I find it kinda fun to know that I can manipulate stings in so
many ways. If one does not work, I can use another. 

