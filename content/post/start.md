+++
Categories = ["Blogging", "hugo"]
Description = ""
Tags = ["blogging", "hugo"]
date = "2017-06-14T09:40:09-07:00"
menu = ""
title = "Creating an R blog (without using blogdown)"

+++

I thought I would write up my experiences starting a blog that uses R
code but that does not use blogdown. Why do I not use blogdown? The
short story is that it does not seem quite ready to use, as it is
missing some elements to make publishing a blog easy.

*DISCLAMER: This is a work in progress - I am not big on writing
blogs, so please forgive me if this is not helpful.*

# Setup

I recommend following the instructions here for simple setup and
 deployment scripts. These have worked well for me.
 
[UPDATE: DEPLOYING HUGO-GENERATED WEBSITES ON PERSONAL GITHUB PAGES](https://hjdskes.github.io/blog/update-deploying-hugo-on-personal-gh-pages/)

# Using R code

This is where things get a little tricky. The biggest obsticle is that
when you knitr or Sweave a `*.Rmd` to a `*.md` file, the
figures (by default) are put in a folder in the working
directory. However, when hugo builds the static website, it moves
everything around. We need to do two things to prepare our R plots and
\*.md files for hugo:

  1. We need to either create the plots in the `/static/` folder, or
     move them there after kniting the `.Rmd`
  
  2. We need to make the file path in the `.md` file point to where
     hugo will place the figures, not where they exist now.
	 
I found moving the images easier, though I suspect either will work. The steps are:

  1. Create the `.Rmd` file. Specify the `fig.path='img/'` (the name of
     this folder does not matter as long as it is consistent for each
     step.

  2. knit the `.Rmd` file. Any figures will end up in a folder 'img/'
  
  3. Copy or move all the contents of `img/` to `{main
     website}/static/img`
  
  4. Change all references in the `.md` file from `img/*` to
     `/img/*`.
	 
Easy, right? I did not want to do this for every single file I might produce, so I
put the whole thing under gnumake using some makefile magic.

~~~bash
POSTDIR := content/post
OBJS := $(shell find -name '*.Rmd')
OUTFILES := $(patsubst %.Rmd,%.md,$(OBJS))
KNIT = Rscript -e "library(knitr); knit('$<')"

all:$(OUTFILES)

$(OUTFILES):$(OBJS)
	$(KNIT); \
	cp -r img/* ../../static/img/; \
	sed -i 's/img\//\/img\//g' $(OUTFILES); \
	cd ../../ && hugo undraft $(POSTDIR)/$(OUTFILES)

clean:
	rm $(OUTFILES)
~~~


