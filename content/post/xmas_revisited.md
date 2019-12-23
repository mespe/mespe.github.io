---
date: "2019-12-22T09:47:01-07:00"
title: "Xmas Functions, revisited"
authors: []
categories:
  - R
tags:
  - R
draft: false
---

Years ago, a friend asked me for help writing a function to do her
family's Xmas gift exchange. You can see her post on a collection of
functions for this
[here](https://myfanwy.github.io/Blog/2019/12/17/Christmas-Gifting-With-R.html). The
basic goal here is to have a function that randomizes the names in the
hat, but with the restriction that close family members (spouses)
cannot draw eachother. We want to simulate the process of pulling
names out of a hat where you cannot draw yourself or your partner.

I literally wrote this two years ago, and very quickly. I did not
expect it to be great. Looking at it now, after some time and experience, I
can see many areas to improve it. And I would hate to have a sloppily
written function be the last word from me.

# The original function

Here is what I originally wrote:

```r
# this function requires a column of NAs in the starting dataframe:
pool <- data.frame(
    id = c(1:7),
    person = c("Cam", "Mitchell",
               "Jay", "Gloria",
               "Manny",
               "Claire", "Phil"),
    partner = c(2, 1, 4, 3, 5, 7, 6),
    id2 = NA
)

xmas2 = function(pool_df) {
    # for 1:7, gift draw number is sampled from anyone BUT that person's spouse:
    
    for (i in seq(nrow(pool_df))) {
        pool_df$id2[i] = sample(setdiff(seq(nrow(pool_df)), c(pool_df$partner[i], pool_df$id2, i)),
                               1)
    }
    
    pool_df$p2 = pool_df$person[pool_df$id2] # make a column of who each person drew (p2)
    
    ok = all(!duplicated(pool_df$id2) &
                 pool_df$partner != pool_df$id2) # ok is a df where no one drew themselves and no one drew their spouse
    if (!ok)
        # if the conditions aren't met, return "somthing went wrong"
        stop("No good - try again")
    return(pool_df) # else, return the finished matches
}

xmas2(pool_df = pool) # call until you get a return
```

Yikes. This is tough to look at.

So, before we get to improving it, we should take a moment to catalog
what is not great about it, as well as what might be OK.

## The bad

*1. Formatting:* I find it very difficult to read the above
code. Having the comments off the end of each line of code is messy,
and the spacing is terrible.

*2. Expectations for the input data structure:* The function expects very
specific structure for the input data.frame, e.g. having an empty
column to fill, expecting the "spouses" column to be indices, etc.

*3. No flexibility in calling the function:* The function takes a
single argument, the data.frame. The user can only modify behavior by
modifying this data.frame.

*4. Cannot make multiple restrictions for each person:* For example,
to prevent someone from "drawing" any of the people in their immediate
family rather than just your spouse, e.g. children or parents.

*5. Forces the user to re-run on failure:* It would be better to keep
running until there is a valid output, rather than forcing the user to
re-run the function.

## The good

*1. It seemed to work:* 

*2. `for` loop is probably appropriate for small lists:* If we wanted to
do this with a large set of inputs, we might want to look at
vectorizing the code, but for this it is fine.

# Improving the xmas function

First things first, we are going to get rid of the dependence on
certain columns being present in the input data.frame. Let's do that by
creating some variables. We will automatically populated these from
the data.frame by default:

```r
xmas = function(df, persons = df$persons, exclude = df$partner) {
    
    recipient = rep(NA, length(persons)) 
```

This might not be the long term solution, but it will work for the
time being. It gets us away from needing there to be a column of a
certain name in the original data.frame. The default option does
expect certain columns, but the user can get around this, e.g.

```r
xmas( , persons = my_data$a, exclude = my_other_data$b)
```

Next, let's relax the data types that are acceptable for the
function. For example, it would be nice if the `persons` and `exclude`
(formally `partner`)
could be either factors, characters, or integers. To do this, we need
to change this line of code:

```r
pool_df$id2[i] = sample(setdiff(seq(nrow(pool_df)), c(pool_df$partner[i], pool_df$id
```

Right now, it expects that the `exclude` is an integer. We can replace
it with:

```r
recipient[i] = sample(setdiff(persons[-i], c(exclude[i], recipient)), 1) 
```

This does two things - first, we exclude drawing ourselves with
`persons[-i]`, and second we made the code work irrespective of the
data type of `persons` and `exclude`. But it is not foolproof. So,
what will screw this up? It will fail if the data types of these two
are different from one another - so we should add in a test,

```r
if(class(persons) != class(exclude))
    stop("Please provide the same data type for both 'persons' and 'exclude'")
```

We might even want to go a step further and tell the user what data
types they provided,

```r
    stop(sprintf("Please provide the same data type for both 'persons' and 'exclude'\n 'persons' is class %s, 'exclude is class %s", class(persons), class(exclude)))
```

OK, we have dealt with the "bad" 1-3 above, but now we want to think
about how we would deal with multiple `excludes` per person. How would
a user even specify a variable number of people
to exclude for each `person`? Since we can have a variable number -
some people will only have 1 or maybe 0 restrictions, while others
might have 2 or more. The natural structure to store items of
different lengths in R is a list. So maybe something like, 

```r
exclude = list("Mitchell", "Cam",
            c("Gloria", "Manny"), c("Jay", "Manny"),
               NA,
               "Phil", "Claire")
```

So, how would we need to change the code to allow something like this?
First, we need to index into the list when drawing from `exclude`,

```r
recipient[i] = sample(setdiff(persons[-i], c(exclude[[i]], recipient)), 1) 
```

This will still work when we have a simple vector, since `[[` will
still pull the correct element of a vector. But our error message will
not work. So, we need to expand this to work when `excludes` is a
list,

```r
if(class(persons) != class(exclude) |
   (class(exclude) == "list" & !all(class(unlist(exclude)) == class(persons))))
    stop(sprintf("Please provide the same data types for both 'persons' and 'exclude'\n 'persons' is class %s, 'exclude is class %s", class(persons), unique(class(unlist(exclude)))))
```

It doesn't win any points for readability, but it will work.

Now, for the last bit - only returning a valid result, rather than
having the user have to call the function again. To fix this, we need
to know why the function was failing in the first place. Since we are
using `setdiff`, we cannot select ourselves or anyone on the `exclude`
for that person, so that is not a possible reason to fail. But we can
end up in a situation where there are no valid selections for a
person, i.e. the only "names in the hat" are their name or people on their
exclude list. So, we need to handle that situation.

One way to handle it is as you would when actually drawing names from
a hat - if you select your own name, you put that draw back and try
again. But what if there is only one name in the hat? Then you would
probably switch names with someone else. So, can we turn this in to an
algorithm?

Rather than code two different behaviors, to re-draw and trade, we
will code just one, to trade. This is because our code makes it
impossible to draw ourselves. Therefore, we will only need to handle a single
case - if the number of choices for a person is 0,

```r
choices = setdiff(persons[-i], c(exclude[[i]], recipient))
if(length(choices) == 0){
    #trade code
} else {
    recipient[i] = sample(choices, 1)
}
```

Now, how would we trade? This gets a little tricky - we need to make
sure the trade does not result in any invalid combinations. So, we
would need to check that one of the remaining choices is valid for our
trade. This might take a little effort to get right. Of course,
this might not even be the right approach - it might be better to just
restart the entire drawing process and keep trying until we get a
valid set. What would that look like?

```r
i = 1
while(i < length(persons){
    choices = setdiff(persons[-i], c(exclude[[i]], recipient))
    if(length(choices) == 0){
        i = 1
        choices = setdiff(persons[-i], c(exclude[[i]], recipient))
    }
    recipient[i] = sample(choices, 1)
    
    i = i + 1
}

```

This works, but there is now a risk of it being a infinite loop -
i.e. if there are very few or no valid combos. So, we need some safeguard
to keep it from running too long. We can add a `max_draw` parameter to
get an upper limit on the number of times we try to find a valid set,

```r
xmas = function(df, persons = df$persons, exclude = df$partner,
                max_draws = 1000) {
    draw = 1
    while(i < length(persons)){
        if(draw > max_draws)
            stop("No valid combos found in ", draw, " attempts")

        }

}
```

Putting it all together, our function looks like this:

```r
xmas = function(df, persons = df$persons, exclude = df$partner,
                max_draws = 1000) {

    if(!all(class(unlist(exclude)) == class(persons)))
        stop(sprintf("Please provide the same data types for both 'persons' and 'exclude'\n 'persons' is class %s, 'exclude' is class %s",
                     class(persons), unique(class(unlist(exclude)))))

    recipient = rep(NA, length(persons)) 
    i = 1
    draw = 1

    while(i < (length(persons) + 1)){
        if(draw > max_draws)
            stop("No valid combos found in ", draw, " attempts")
        
        choices = setdiff(persons[-i], c(exclude[[i]], recipient))
        if(length(choices) == 0){
            i = 1
            draw = draw + 1
            choices = setdiff(persons[-i], c(exclude[[i]], recipient))
        }
        
        recipient[i] = sample(choices, 1)
        
        i = i + 1
    }
    
    return(recipient)
}

```

# Testing our function

We will grab the data.frames used by one of the other original
functions to start,

```r
d = data.frame(
    persons = c("Cam", "Mitchell", "Jay",
                "Gloria", "Manny", "Claire", "Phil"),
    partner = c("Mitchell", "Cam", "Gloria",
                "Jay", NA, "Phil", "Claire"),
  stringsAsFactors = FALSE)

```

and use it as an input for our function,

```r
replicate(10, xmas(d))
```
```
     [,1]       [,2]       [,3]       [,4]       [,5]       [,6]      
[1,] "Claire"   "Phil"     "Mitchell" "Mitchell" "Mitchell" "Gloria"  
[2,] "Manny"    "Claire"   "Gloria"   "Jay"      "Claire"   "Claire"  
[3,] "Gloria"   "Gloria"   "Manny"    "Cam"      "Gloria"   "Cam"     
[4,] "Phil"     "Cam"      "Phil"     "Claire"   "Cam"      "Jay"     
[5,] "Mitchell" "Mitchell" "Claire"   "Gloria"   "Phil"     "Phil"    
[6,] "Jay"      "Jay"      "Jay"      "Phil"     "Jay"      "Mitchell"
[7,] "Cam"      "Manny"    "Cam"      "Manny"    "Manny"    "Manny"   
     [,7]       [,8]       [,9]       [,10]     
[1,] "Phil"     "Phil"     "Mitchell" "Mitchell"
[2,] "Gloria"   "Claire"   "Manny"    "Phil"    
[3,] "Cam"      "Manny"    "Phil"     "Gloria"  
[4,] "Claire"   "Jay"      "Claire"   "Jay"     
[5,] "Jay"      "Gloria"   "Cam"      "Claire"  
[6,] "Mitchell" "Cam"      "Gloria"   "Cam"     
[7,] "Manny"    "Mitchell" "Jay"      "Manny" 
```

What about when we use a list?

```r
exclude = list("Mitchell", "Cam",
            c("Gloria", "Manny"), c("Jay", "Manny"),
               NA,
               "Phil", "Claire")
replicate(10, xmas(d, exclude = exclude))

```
```
     [,1]       [,2]       [,3]       [,4]       [,5]       [,6]      
[1,] "Manny"    "Jay"      "Gloria"   "Phil"     "Claire"   "Gloria"  
[2,] "Claire"   "Manny"    "Claire"   "Claire"   "Manny"    "Manny"   
[3,] "Phil"     "Phil"     "Phil"     "Mitchell" "Mitchell" "Claire"  
[4,] "Cam"      "Cam"      "Mitchell" "Cam"      "Phil"     "Mitchell"
[5,] "Gloria"   "Claire"   "Cam"      "Jay"      "Gloria"   "Phil"    
[6,] "Mitchell" "Gloria"   "Manny"    "Gloria"   "Cam"      "Jay"     
[7,] "Jay"      "Mitchell" "Jay"      "Manny"    "Jay"      "Cam"     
     [,7]       [,8]       [,9]       [,10]     
[1,] "Phil"     "Phil"     "Jay"      "Manny"   
[2,] "Gloria"   "Gloria"   "Phil"     "Claire"  
[3,] "Claire"   "Claire"   "Claire"   "Phil"    
[4,] "Mitchell" "Cam"      "Cam"      "Mitchell"
[5,] "Cam"      "Jay"      "Mitchell" "Jay"     
[6,] "Jay"      "Mitchell" "Gloria"   "Cam"     
[7,] "Manny"    "Manny"    "Manny"    "Gloria"  
```

OK, it all seems to be working OK. Let's check our error messages,

```r
xmas(d, exclude = 1:7)
```
```
Error in xmas(d, exclude = 1:7) : 
  Please provide the same data types for both 'persons' and 'exclude'
 'persons' is class factor, 'exclude is class integer
```

Now that we are thinking of it, it might be good to check for some
other things, like if the `persons` and `exclude` are the same
length. This was not a concern when we were pulling from a data.frame,
since columns in a data.frame must be the same length.  But when a
list or elements from other objects are provided, we should check that
they have the same length.  Adding this code to the main function
might make sense, but it will also make it longer and might hurt
readability. We could instead make a new function to check the inputs,

```r
check_input = function(persons, exclude)
{
    if(!all(class(unlist(exclude)) == class(persons)))
        stop(sprintf("Please provide the same data types for both 'persons' and 'exclude'\n 'persons' is class %s, 'exclude' is class %s",
                     class(persons), unique(class(unlist(exclude)))))
    if(length(persons) != length(exclude))
        stop("Persons and exclude of different lengths")
    return(NULL)
}
   
```

OK, so the final form of the `xmas` functions is

```r
xmas = function(df, persons = df$persons, exclude = df$partner,
                max_draws = 1000) {

    check_input(persons, exclude)
    
    recipient = rep(NA, length(persons)) 
    i = 1
    draw = 1

    while(i < (length(persons) + 1)){
        if(draw > max_draws)
            stop("No valid combos found in ", draw, " attempts")
        
        choices = setdiff(persons[-i], c(exclude[[i]], recipient))
        if(length(choices) == 0){
            i = 1
            draw = draw + 1
            choices = setdiff(persons[-i], c(exclude[[i]], recipient))
        }
        
        recipient[i] = sample(choices, 1)
        
        i = i + 1
    }
    
    return(recipient)
}

```

# Closing thoughts

Is this function perfect? Not even close - but it is much better. I am
sure there are some clever things that can be done to make it more
efficient. 

I am looking forward to checking it out in another couple years to see
how I could improve it.
