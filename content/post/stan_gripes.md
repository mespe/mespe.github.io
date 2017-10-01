+++
title= "A few things I don't love about Stan"
date= "2017-08-24T20:51:22-07:00"
Description = "My gripes with my favorite stats program"
Tags = ["Stan", "stats"]
Categories = ["data science"]
menu = ""
toc = true
+++

First off, I love [Stan](mcstan.org). If you have not heard of Stan, I
highly recommend you check it out. I use Stan for almost all of my
statistics these days, and have published several papers using it. 

That said, there are a fair number of things I don't like about it:

# The model trade-offs

In my experience, when you are developing a model in Stan, you are
often faced with trade-offs between 1) flexibility, 2) readability,
and 3) development time.

To illustrate with a silly example, say I had a basic model:


```stan
data{
  int N;
  vector[N] y;
}
parameters{
  real mu;
}
model{
  target += normal_lpdf(y | mu, 1);
}
```


This model is simple and easy to read, but it is not flexible. To make
this model take a number of predictors, I have to modify the code:

```stan
data{
  int N;
  vector[N] predictor;
  vector[N] y;
}
parameters{
  real beta;
  real mu;
}
model{
  target += normal_lpdf(y | mu + beta * predictor, 1);
}
```

Creating this little change would mean sitting through compiling the
model again. Doing this once or twice is not so bad, but when I am
working with a model this could be up to 20 or 30 times. That really
adds up. 

Second, each time I edit the model, there is the potential to
introduce errors. Therefore, in addition to the compiling time, there
is time spent debugging, etc.

So, instead of all of this, I could have coded the model as:

```stan
data{
  int N;
  int num_pred; //number of predictors
  matrix[N, num_pred] X; //model matrix
  vector[N]y;
}
parameters{
  vector[num_pred] beta;
}
model{
  target += normal_lpdf(y | beta * X, 1);
}
```

This model is actually the same as both of the models above. Compared
to those, it is much more flexible. This model can accept countless
number of predictors without changing the Stan code at all. All that
compiler time is gone, as is the time spent debugging and checking the
model. 

So, you might say this is clearly the winner, right?

Well, I actually prefer the first way because:

1. The model code is expressive of what happening in the analysis. Though it
   is more inefficient on several fronts, this code allows someone to see
   exactly what is happening. Looking at the more efficient model, a
   reader will not have a clue how many predictors were used, what
   their types were, etc. An extreme example of this is the model code
   for
   the
   [rstanarm package](https://github.com/stan-dev/rstanarm/tree/master/inst/chunks) for
   R. These models are extremely flexible and highly efficient, but
   almost impossible to tell on first glance what is going on.

2. The inefficiency can be a feature. More than once I have been
   forced to actually spell out the math in the models I am creating,
   rather than just relying on the "throw it in a model matrix and it just
   runs" approach. For me, forcing me to slow down and think
   deliberately about my model is helpful.
   
3. Verbose models are often easier for people with little statistics
   experience to approach. As I think more and more about reproducible
   research, the more I think I would happily trade some wall time for
   a model that more readers can understand and even potentially
   modify.
   
   
# So what's the issue?

Those compile and testing times are killing me. Developing models as
part of my analysis is slow. When a project comes up that needs an
answer ASAP, I will often resort to rstanarm or even lme4. But for
something I am going to publish alongside a manuscript, I prefer the
slower approach.

My wish list then is:

1. A testing framework that will work for custom written models to
   quickly evaluate that they are performing as intended.
   
2. Custom Bayesian models that have the speed of compiled code, but without
   the lengthy compile times. I would trade a little wall time for
   reduced compile times.
   
Which makes me wonder what potential exists in 1) Nimble compiled with
LLVM, 2) Julia, or 3) Go. Go is the most intriguing to me - it is
almost as fast as C++, but compiles ridiculously quickly. Hmmm...
