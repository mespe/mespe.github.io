---
date: "2020-07-26T11:38:38-06:00"
title: "Untidy Tuedsay: Cocktails"
authors: ["mespe"]
categories:
  - R
tags:
  - R
draft: false
---

So, one of the [Tidy Tuesday
challenges](https://github.com/rfordatascience/tidytuesday) was
brought to my attention recently due to a [video of Hadley Wickam
taking a swing at it](https://www.youtube.com/watch?v=kHFmtKCI_F4).

In the video, Hadley does what he calls a hacky fix to tackle the data
having fractions in the measure column, e.g.

```
1 1/2 oz
```

Hadley does what many people appear to have done, which is to replace the
fraction with the equivalent decimal using string replacement,

```r
measure = str_replace(measure, " ?1/2", ".5")
```

Unfortunately, this does not scale - you have to manually create a
conversion for each fraction you encounter. Hadley mentions that if
this were a real analysis, he would probably put this into a function,
but does not really explain how to do this. 

I am going to show a quick way to deal with this issue using base
R. We are also going to tackle some issues that Hadley did not during
his stream.

# Getting the fraction

First, we need the data:

```r
d = read.csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2020/2020-05-26/cocktails.csv")
```

We can take a look at it to make sure it is what we expect,

```r
head(d)
```

So, the column we are interested in is the `measure` column. To make
life easy, we are going to pull this out to work with it separate as
we develop a function,

```r
m = d$measure
head(m)
```
```
[1] "1 oz white" "1 oz"       "1/2 oz"     "1/4 oz"     "1/4 oz"    
[6] "1/4 oz"    
```

So, where do we start with this? It seems the first step will be
separating out the number from the text of the unit. We can do that
with a `gsub()`. We just need to think about what regular expression
we would use. We can try to capture the pattern of some number, maybe
preceeded by a space, with maybe some other number and `/` as well. 
Or, we can just get rid of any characters,

```r
num = gsub("[A-z]", "", m)
head(num, 40)
```
```
 [1] "1  "   "1 "    "1/2 "  "1/4 "  "1/4 "  "1/4 "  "1/4 "  "1/4 "  "1/2 " 
[10] "1/4 "  "16 "   "1.5 "  "1/2 "  "1/2 "  "1/2  " "1 "    "1 "    "3 "   
[19] "1 "    "1 "    "1 "    "2 "    "1 "    "1 "    "1/2 "  "1/2 "  "1/2 " 
[28] "1/2  " "1/2  " "1/2 "  "1/3 "  "1/3 "  "1/3 "  "1/2 "  "1/2 "  "1/2 " 
[37] "1/2 "  "1/2 "  "\n"    "1-2 " 
```

So, that worked OK, but we still have some newline characters and
unnecessary white space. We can trim these,

```r
num = gsub("^ +| +$|\n", "", num)
head(num, 40)
```
```
 [1] "1"   "1"   "1/2" "1/4" "1/4" "1/4" "1/4" "1/4" "1/2" "1/4" "16"  "1.5"
[13] "1/2" "1/2" "1/2" "1"   "1"   "3"   "1"   "1"   "1"   "2"   "1"   "1"  
[25] "1/2" "1/2" "1/2" "1/2" "1/2" "1/2" "1/3" "1/3" "1/3" "1/2" "1/2" "1/2"
[37] "1/2" "1/2" ""    "1-2"
```

OK, looking better. We still have two issues we need to deal with in
order to convert this to a numeric column. First, there are some
ranges listed, e.g. `1-2`. Second, the original issue we had with
this - the fractions.

We'll deal with the fractions first. One issue with the hacky solution
that Hadley used was we needed to enumerate every possible
fraction. This is going to be tedious and error prone. If possible, we
would like to be able to handle any possible fraction. But how?

Well, if we could extract just the fraction part of the number and
type it into R, we would get the decimal as the result, e.g.

```r
1/2
```
```
[1] 0.5
```

So, how can we do this with a text string?  We can evaluate it.

Before we can evaluate the string, we need to `parse()` it first. So, if we do,

```r
eval(parse(text = "1/2"))
```
we get

```
[1] 0.5
```

We will go ahead and turn that into a function,

```r
decimal_from_text = function(x)
{
    eval(parse(text = x))
}
```

So, now we have a couple different options. We can try to extract that
fraction, convert to the decimal, put it back into the string, and
then convert the entire string to numeric. This is the approach Hadley took, more or less. 

We can also put a `+` in place of the space between the number and the
fraction. We can put this in place using `gsub()` with capture groups,

```r
gsub("([0-9]+) ([0-9]+/[0-9]+)", "\\1+\\2", num)
```

If we evaluate that using our function above, we will get the right
answer for almost all the strings. 

However, this approach will actually introduce a different issue. Strings
such as `8-10`, will be evaluated as 8 minus 10. This is really
intended to mean "8 to 10". So, we need to handle these before using our
function (or replace the erroneous values afterwards).

Let's look at these values, 

```r
grep("[0-9]+-[0-9]+", num, value = TRUE)
```
```
 [1] "1-2"   "2-3"   "1-3"   "3-4"   "3-6"   "2-3"   "2-3"   "1-2"   "6-8"  
[10] "1-2"   "2-3"   "4-5"   "10-12" "4-6"   "3-4"   "3-4"   "1-2"   "2-3"  
[19] "2-4"   "2-3"   "1-2"   "3-4"   "3-4"   "3-4"   "3-4"   "8-10"  "8-10" 
[28] "4-5"   "3/4-1" "1-2"  
```

OK, `3/4-1` might be a problem, since it contains both a fraction and
a dash. We might need to set that one aside as a special case. 

For now, we'll focus on a solution for just the dash. How should we
handle these?

We can just use the average of the 2 values, which
might be the easiest approach. To do this, we'll actually adapt our
function above,

```r
mean_dashed_items = function(x)
{
    res = sapply(x, function(nn)
        eval(parse(text = gsub("-", "+", nn))))
    res/2
}
```

You can see we actually are using similar approaches; we replace the
dash with a `+`, evaluate the expression, and then divide the result
by 2 to get the average of the two numbers. 

We can test it out,

```r
dashes = grep("[0-9]-[0-9]", num, value = TRUE)
mean_dashed_items(dashes)
```
```
   1-2    2-3    1-3    3-4    3-6    2-3    2-3    1-2    6-8    1-2    2-3 
 1.500  2.500  2.000  3.500  4.500  2.500  2.500  1.500  7.000  1.500  2.500 
   4-5  10-12    4-6    3-4    3-4    1-2    2-3    2-4    2-3    1-2    3-4 
 4.500 11.000  5.000  3.500  3.500  1.500  2.500  3.000  2.500  1.500  3.500 
   3-4    3-4    3-4   8-10   8-10    4-5  3/4-1    1-2 
 3.500  3.500  3.500  9.000  9.000  4.500  0.875  1.500 

```

Glancing over these, it appears to have worked, even in the case of
the `3/4-1`. That's great. 

So, how would we put this all together? We might do something like,

```r
cocktail_measure_value = function(x)
{
    # get just numbers
    num = gsub("[A-z]", "", x)
    # Trim whitespace
    num = gsub("^ +| +$|\n", "", num)
    # replace space between numbers with `+`
    num = gsub("([0-9]+) ([0-9]+/[0-9]+)", "\\1+\\2", num)
    # convert to numeric
    ans = decimal_from_text(num)

    # handle dashed numbers and replace values in answer
    dashes = grep("[0-9]-[0-9]", x)
    ans[dashes] = mean_dashed_items(num[dashes])

    ans
}
```

OK, so, it's not elegant. It needs some work and probably some testing
to ensure it handles all cases. But it should give you an idea for how
to do what Hadley hinted at.

