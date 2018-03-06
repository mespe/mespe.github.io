---
title: "Dynamic Docs in R (part 1)"
authors: []
categories:
  - R
  - data science
tags:
  - data science
  - R
date: 2018-03-05T11:14:04-07:00
draft: false
---

# A brief intro to dynamic documents in R

I was asked to give a short overview of dynamic documents in R during
Dr. Cini Brown's lab meeting at Colorado State University last week,
and I thought I would post some of the material here as well.

## What is a dynamic document?

Dynamic documents, or living documents, are documents which can be
continuously updated, and are often self-contained. This means that
the document itself contains the framework to generate and update its
content. Pretty cool, huh?

In R, a dynamic document is a document which contains code to generate
tables, figures, and in-text content. Dynamic documents have a few
benefits by provide a means to:

- Document the process behind the content: the code itself shows how
  the content was generated
- Reproduce the results: related to the above, the document can be
  re-created with the original data, which allows an explicit means
  to reproduce not only the analysis but also the document itself.
- Update the document based on new analysis or data: revisions are
  relatively easy because one change propagates through the entire
  document.
- Typically easy to version control: most dynamic documents are
  simple text files, which can be put under version control using
  git, svn, etc.
	
That said, there are also some drawbacks:

- More complicated to collaborate: Unless your collaborators are
  comfortable working outside of MS Word, it can be a real hassle
  to collaborate. There will always be a disconnect between a revised
  or edited version in Word and the original dynamic document.
- Added complexity: Dynamic documents require additional pieces of
  software which creates additional stages where things can (and
  will) go wrong. 
- Require investment to learn: time spent learning
  Rmarkdown/Sweave/etc. is time not spent writing or researching.
  
So is it worth it? I generally think it is. The little bit of time
invested in learning these technologies pays off when you need to make
just a small change to a research paper's methods or data. What would
otherwise be a day's work turns into minutes with a dynamic document.

## Types of dynamic documents

- Rmarkdown: In general, when the R community talks about dynamic documents, they
  are primarily talking about Rmarkdown. Rmarkdown is an extension of
  Markdown, a simple syntax for marking up documents with basic
  formatting (headings, bold font, etc.). Rmarkdown is so prominent
  because it is well-supported by Rstudio, the environment that most R
  users are working in. Additionally, once converted to plain Markdown
  files, multiple formats can be created (e.g., HTML, PDF, MS Word, etc.)
  from the same file. The biggest benefits of Rmarkdown are this
  support, adaptibility and simplicity in use. 
- Sweave/R-no-web: Older technology for dynamic documents, Sweave
  allows embedding R code into LaTeX documents. The syntax is more
  complicated than Rmarkdown, but in return you gain additional
  control and fuller range of customization options.\*
- Org-mode: Similar to Rmarkdown, this is a simple syntax, but has the
  added benefit of being in Org-mode, and supporting almost any
  language you can think of. 
- XML: For those crazy enough (DTL), you can also write in XML. This
  allows even more control compared to Sweave, with the added benefit
  of being about to query the document using XPath.


\* *Some people will point out that you can use LaTeX commands in
Rmarkdown documents, and while this is true, in my experience it is
quite clunky and prone to errors. This is because the LaTeX commands
do not always play nice with the LaTeX template used by knitr/pandoc.*




