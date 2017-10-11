+++
Categories = ["Blogging", "hugo"]
Description = ""
Tags = ["blogging", "hugo"]
date = "2017-06-14T09:40:09-07:00"
menu = ""
title = "Creating an R blog with Hugo (without using blogdown)"

+++

*UPDATE: The fine folks behind Hugo put together a [nice guild that
covers the steps involved in getting github pages and Hugo setup.](https://gohugo.io/hosting-and-deployment/hosting-on-github/)*

I thought I would write up my experiences starting a blog that uses R
code but that does not
use [blogdown](https://github.com/rstudio/blogdown). Why do I not use
blogdown? The short story is that blogdown does not seem quite ready to use,
as it is missing some elements to make publishing a blog easy. 

The long story is that after
spending a good chunk of time helping a student navigate through using
blogdown, I found the approach below to be both easier to use and
simpler to implement. The
disadvantage of this approach is you have to step out of RStudio (which
I don't use anyway) and into the terminal.

*DISCLAIMER: This is a work in progress - I am not big on writing
blogs, so please forgive me if this is not helpful.*

# Setup

I recommend following the instructions here for simple setup and
 deployment scripts. These have worked well for me.
 
[UPDATE: DEPLOYING HUGO-GENERATED WEBSITES ON PERSONAL GITHUB PAGES](https://hjdskes.github.io/blog/update-deploying-hugo-on-personal-gh-pages/)

# Using R code

This is where things get a little tricky. The biggest obstacle is that
when you knitr or Sweave a `.Rmd` to a `.md` file, the
figures (by default) are put in a folder in the working
directory. However, when hugo builds the static website, it moves
everything around. We need to do two things to prepare our R plots and
\*.md files for hugo:

  1. We need to either create the plots in the `/static/` folder, or
     move them there after knitting the `.Rmd`
  
  2. We need to make the file path in the `.md` file point to where
     hugo will place the figures in the built website, not where they exist now.
	 
I found moving the images easier, though I suspect either will work. The steps are:

  1. Create the `.Rmd` file. Specify `fig.path='img/'` (the name of
     this folder does not matter as long as it is consistent for each
     step). For example:
	 
	 ~~~R
	 ```{r fig.path = "img/"}
  plot(rnorm(100))
  ```
  ~~~

  2. knit the `.Rmd` file. Any figures will end up in a folder named
     `img/` in the current working directory.
  
  3. Copy or move all the contents of `img/` to `{main
     website}/static/img`, where `{main website}` is the top-most
     directory of your website.
  
  4. Change all references in the `.md` file from `img/*` to
     `/img/*`. The tricky thing is you want to do this **after** the
     `.md` file has been programmatically created, not in the `.Rmd`
     source file.
	 
Easy, right? I did not want to do this for every single file I might produce, so I
put the whole thing under gnu make using some makefile magic.

 `Makefile`
~~~makefile
POSTDIR := content/post
OBJS := $(shell find -name '*.Rmd')
OUTFILES := $(patsubst %.Rmd,%.md,$(OBJS))
KNIT = Rscript -e "library(knitr); knit('$<')"

all:$(OUTFILES)

$(OUTFILES):$(OBJS)
	$(KNIT); \
	cp -r img/* ../../static/img/; \
	sed -i 's/img\//\/img\//g' $(OUTFILES)

clean:
	rm $(OUTFILES)
~~~

This finds all the files with the `.Rmd` extension, and converts them
to `.md` files. It then uses `sed` to replace `img/` with `/img/`. You
will need to edit this to reflect your file structure (I have
my `.Rmd` files in `mespe.github.io/content/post/` so I need to
accend 2 directories to get to `static/`).

So, what does the resulting workflow look like?

  1. Author a `.Rmd` file
  2. Navigate to the correct folder and run `make all` in the terminal

That's it - no blogdown required.
