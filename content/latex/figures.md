+++
title = "Figures and Tikz"
date = 2019-07-23T21:00:27-05:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
# linktitle = "Example"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.latex]
  # Define a parent ID if this page is a parent.
  name = "Figs"
  
  # Reference a parent ID if this page is a child.
  # parent = "YourParentID"
  
  # Order that this page appears in the menu.
  weight = 2
+++

Most figures I see in papers are bad. 
Some of my figures are also bad. 
On this page I try to collect some guidlines on how to make figures better. 

# Style
## Vector Graphics
The number one tip for making figures instantly better is to use vector graphics. Formats such as pdf, svg, and eps are capabale of this though using these formats are not a guarantee.
[Inkscape](https://inkscape.org/) is a great, opensource tool to edit vector graphics files. 

Alternatively (and preferably) the figures can be generated in the LaTeX using Tikz where you define nodes and their connections. The figure is then made whenever you compile your document. I have a whole separate page devoted to Tikz. 


## Colors
Figures should have pleasant colors that help illustrate a point.
[Colorbrewer](http://colorbrewer2.org) is a great tool that can help create a color palate for a plot. 

### Consistency. 
If some data is represented across multiple plots, make the colors for like elements identical. This helps the reader connect data across the plots. 

## References to Figs
When you reference a figure, always do it as "Fig. 1" as opposed to "Figure 1." The "Fig." is the default label to show up in the caption so you should mirror this in the text. Moreover, as a general rule when doing technical writing, you should always condense your work to save space. In this case, "Fig." saves two characters! 

Never hardcode your reference to a figure. In the figure, define a label. In the text, reference that label. This looks like:
```latex
Fig. \ref{fig:cool_plot} shows an example of good color choices. 
```
By doing this, the reference will always grab the correct index for a figure, even if things get moved around.

## Captions
Captions should be sort, but also as self contained as possible. When people read papers, they often read the abstract and intro, look at the tables and figures, read the conclusion, and then, if they feel its worth their time, will read in more depth. 
With this in mind, it is best to give detailed captions that can quickly help the reader understand what they are looking at and why it is significant. 

# Content 
Content is more important than style. But most of what I feel like I have to offer is related to creating a styleguide for authors. 

## Don't make a plot without a point. 
When doing research, we often collect a lot of data and will make many graphs to represent the data. Don't through in a plot unless it is part of a story. All papers should tell a story. The plot is part of the plot of that story (pun intended).

Make sure the main insight from the plot is apparent in the graphic and stated in the caption. The full, detailed discussion of the significance should be in the body of the paper. 


