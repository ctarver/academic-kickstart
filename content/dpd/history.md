---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "History"
linktitle: "History"
summary:
date: 2019-10-22T17:10:07+08:00
lastmod: 2019-10-22T17:10:07+08:00
draft: false  # Is this a draft? true/false
toc: true  # Show table of contents? true/false
type: docs  # Do not modify.

# Add menu entry to sidebar.
# - Substitute `example` with the name of your course/documentation folder.
# - name: Declare this menu item as a parent with ID `name`.
# - parent: Reference a parent ID if this page is a child.
# - weight: Position of link in menu.
menu:
  dpd:
    name: History
    parent: Overview
    weight: 1
---


On this page, I present a timeline of the main works on the topic so that we can see the evolution of the field. 

I'd really like to make my summaries sortable so that we could sort the topics by time, citations, or general theme. However, I currently don't have time to implement such a cool thing on my site. 
# 1988
## [A Multilayer Neural Network Controller](https://ieeexplore.ieee.org/document/1868/)
**Authors:**  D. Psaltis ; A. Sideris ; A.A. Yamamura \
**Citations:** 722
**Publication:** IEEE Control Systems Magazine
### Creator of the Indirect Learning Architecture (ILA)!
Creates the ILA for neural network training purposes. Not used for predistortion in this paper. We don't see this used until [1997](#1997).


# 1993
## A Neural Network Approach to Data Predistortion With Memory in Digital Radio Systems

# 1997
## [A New Volterra Predistorter Based on the Indirect Learning Architecture](https://ieeexplore.ieee.org/document/552219)
**Authors:**  Changsoo Eun; E.J. Powers 
**Citations:** 295
## First to use Indirect Learning Architecture (ILA)!
Previous works relied on creating an inverse Volterra model. This proved to be easier to train.

On other interesting thing in this paper is that they conclude that for systems with many coefficients, "neural-network-basedpredistorters, designed using the indirect learning architecture, "


# 2001
## [Digital Predistortion of Wideband Signals Based on Power Amplifier Model with Memory](https://ieeexplore.ieee.org/document/966559)
**Authors:**  J. Kim ; K. Konstantinou
**Citations:** 416
**Publication:**  Electronics Letters 
### Introduces the Memory Polynomial!
$$ y(t) = \sum \sum \alpha_{p,m} x(n) |x(n-m)|^{p-1}$$
At this time, memory effects were being recognized to be important as people start considering larger bandwidths. The memory polynomial is essentially the diagonals of a volterra series, allowing for significantly reduced training and implementation complexity.


# 2004
## [A Robust Digital Baseband Predistorter Constructed Using Memory Polynomials](https://ieeexplore.ieee.org/document/1264205)
**Citations:** 676 \\
**Authors:** Lei Ding ; G.T. Zhou ; D.R. Morgan ; Zhengxiang Ma ; J.S. Kenney ; Jaehyeong Kim ; C.R. Giardina \\
**Tags:** \\
**Publication:** IEEE Transactions on Communications \\
### First to use ILA with Memory Polynomials!

# 2006
## [A Generalized Memory Polynomial Model for Digital Predistortion of RF Power Amplifiers](https://ieeexplore.ieee.org/document/1703853)
**Citations:** 624
**Authors:** D.R. Morgan ; Z. Ma ; J. Kim ; M.G. Zierdt ; J. Pastalan 
### Introduces the Generalized Memory Polynomial (GMP)
The GMP is more expressive than the MP from Kim's [2001 paper](#2001)

# 2007
## Adaptive Digital Feedback Predistortion Technique for Linearizing Power Amplifiers
**Citations:** 79 \\
**Authors:**   Young Yun Woo ; Jangheon Kim ; Jaehyok Yi ; Sungchul Hong ; Ildu Kim ; Junghwan Moon ; Bumman Kim \\
**Tags:** \\
**Publication:** IEEE Transactions on Microwave Theory and Techniques

# 2008
## Open-Loop Digital Predistorter for RF Power Amplifiers Using Dynamic Deviation Reduction-Based Volterra Series
**Citations:** 173 \\
**Authors:** Anding Zhu ; Paul J. Draxler ; Jonmei J. Yan ; Thomas J. Brazil ; Donald F. Kimball ; Peter M. Asbeck \\
**Tags:** \\
**Publication:** IEEE Transactions on Microwave Theory and Techniques

# 2009
## Crossover Digital Predistorter for the Compensation of Crosstalk and Nonlinearity in MIMO Transmitters
**Citations:** 78 \\
**Authors:**  Seyed Aidin Bassam ; Mohamed Helaoui ; Fadhel M. Ghannouchi  \\
**Tags:** MIMO\\
**Publication:** IEEE Transactions on Microwave Theory and Techniques

# 2011
## 2-D Digital Predistortion (2-D-DPD) Architecture for Concurrent Dual-Band Transmitters -- Carrier Aggregation
**Citations:** 184 \\
**Authors:**  Seyed Aidin Bassam ; Mohamed Helaoui ; Fadhel M. Ghannouchi \\
**Tags:** \\
**Publication:** IEEE Transactions on Microwave Theory and Techniques

This work is a critical work. It is one of the first to consider carrier aggregation.

# 2012
## Band-Limited Volterra Series-Based Digital Predistortion for Wideband RF Power Amplifiers
**Citations:** 123 \\
**Authors:**   Chao Yu ; Lei Guan ; Erni Zhu ; Anding Zhu  \\
**Tags:** \\
**Publication:** IEEE Transactions on Microwave Theory and Techniques


# 2014
## Behavioral Modeling and Linearization of Crosstalk and Memory Effects in RF MIMO Transmitters

# 2016 
## [The Evolution of PA  Linearization: From Classic Feedforward and Feedback Through Analog and Digital Predistortion](https://ieeexplore.ieee.org/document/7379110)
**Authors:** Allen Katz, John Wood, Daniel Chokola\
**Publication:** IEEE Microwave Magazine
### A great survey of DPD!

