---
date: "2018-04-19T11:47:40-06:00"
title: "Dynamic documents (part 2)"
authors: ["mespe"]
categories:
  - R
  - data science
tags:
  - R
  - data science
draft: false
toc: true
---

*Wow - long delay on this one. There are good reasons, which will be explained in coming posts. Stay tuned.*

# A brief intro to dynamic documents in R (cont.)

Last time I outlined why you might want to use a dynamic document, and some of the popular formates that are commonly used.
There are many great resources for the syntax used to actually create dynamic documents (e.g., for Rmarkdown: [Rstudio](https://rmarkdown.rstudio.com/), [R-bloggers](https://www.r-bloggers.com/r-markdown-and-knitr-tutorial-part-1/), etc.)
Rather, today I will outline some tips and tricks I have picked up while creating publications using Rmarkdown/Sweave.

## Make your document easy to version control

One of the main benefits of dynamic documents is that they can be easy to put under version control. 
*Can be.* But often we will be tempted to make it more difficult. How?

  + **Word-wrap entire paragraphs:** Rstudio and other editors can wrap long text lines, so they appear to be similar to what happens in MS Word. 
  Unfortunately, this can create real havok when the doc is put under version control systems like `Git`.
  `Git` was built for version control of code, hence it records changes at the text line level.
  If you create an entire paragraph on a single line, now any edit, even just a single character, is recorded as a change to that entire paragraph.
  When looking at the `git diff`, it can become difficult to tell what actually changed.
  Therefore, it is best to keep each sentence on its own line, or at least put only a few sentences on each line.
  
  + **Rely on complicated formatting in MS Word:** Many of us become dependent on making our document look fancy using MS Word's tools.
  MS Word's "what you see is what you get" model can be great for some word processing tasks.
  However, this model requires someone behind the screen to manipulate controls, something not captured by version control on the markdown document.
  Therefore, this workflow generally does not fit with the goal of automatic generation of dynamic documents. 
  Therefore, it is best to try to stick to simple markup for essential formatting, such as *emphasis*, **bold**, or section headings.
  Often, you will find you don't need the more complex formatting.

  + **Make supporting documents, data, or other items not easily accessible:** 
  Often, important data, figures, and supporting code are kept separate from the document, either in on a local drive, Dropbox/Google Drive/Box, etc., or in a private repository.
  When working with some datasets which cannot be shared, such as medical records or propriatory data, this can be unavoidable.
  But many times this is not the case, and we just are in the habit of keeping the data in certain locations.
  By doing this, the benefit of rebuilding dynamic documents is lost to anyone but yourself.

## Convert when necessary

  + **Collaboration necessitates conversion:** Unless you are lucky or work alone, not all of your collaborators will be familar or comfortable working with a dynamic document.
  A co-author might want to edit for content, and not care about how the document was generated.
  In these cases, conversion to PDF or docx files can be the easiest way to allow them to contribute.
  You gain nothing by being a dynamic document purist. 
  
## Keep your document under version control as long as you can

  + **docx/PDFs are not easy to version control:**
  Once you start getting feedback on converted versions of the document, it can be really tempting to then work on these converted documents rather than the original.
  Try to hold off as long as possible.
  Even if you think you are on the final revisions, you likely aren't, and you can create a massive headache for yourself by ditching the dynamic document framework too early.
  All the work put into creating a dynamic document under version control can be lost if the final ten drafts are neither dynamic nor version controlled.
  Try to make sure even small changes get pushed back to the dynamic version.
  
## Avoid cutting corners

  + **Easy now is a headache later:**
  Sure, it can be a pain to type `\Sexpr{round(coef(mod)[2], 1)}` rather than 2.1, and you will be tempted to just type a few of these little numbers or other elements. 
  But doing this now makes part of the document static rather than dynamic, and arguably you have a larger headache than just creating a static document.
  Sometimes little changes by hand are unavoidable, for example the locations of labels on a figure. 
  It can be easier to just use `locator()` to select points that do not interfer with eachother.
  But even in these cases, the locations on the plot can be saved as data.
  This enables you to update the plot data and allow the plot generation to remain dynamic.
  
  + **Assume the data/analysis/code will change:**
  If you always assume that the inputs to your document will change, you will create a document robust to changes.
  This forces you to use better practices, and in almost all cases will save time and energy later.
  I experienced this first-hand where the "final drafts" of a paper spanned 6 months and 30 commits.
  Don't bet on your dynamic document being static.
