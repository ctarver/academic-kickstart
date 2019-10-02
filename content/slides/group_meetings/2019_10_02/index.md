---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "2019_10_02"
summary: ""
authors: []
tags: []
categories: []
date: 2019-10-02T09:37:04-05:00
diagram: true
slides:
  # Choose a theme from https://github.com/hakimel/reveal.js#theming
  theme: white
  # Choose a code highlighting style (if highlighting enabled in `params.toml`)
  #   Light style: github. Dark style: dracula (default).
  highlight_style: dracula
---

# Group Meeting

Chance Tarver\\
October 2nd, 2019

---

## Current Projects

* Trip to Taiwan
* SIPS Presentation
* GPU Based DPD for Massive MIMO (40%)
* Asilomar: DPD with NNs
* LDPC GPU for 5G NR
* PhD Proposal

---

## Trip to Taiwan
* Leave on Saturday morning.
* Land in Taiwan Sunday Evening. 
* Bringing 2 Skylark Boards, 1 USRP. 

---
## SIPS Presentation
* Updated:
   * Fewer curves in plots. 
   * Clearer labels
   * Condensed some slides
   

---
## GPU-Based DPD for M.MIMO
* Motivation:
   * PA Nonlinearities force backoff forcing less efficient operation
   * DPD can be computationally complex, especially for large BWs in NR.
   * PA efficiency and computational complexity problems compund in Massive MIMO
   * GPUs can easily perform parallel computation to apply to many anntenas 
   
---

## PAE Example
{{< figure src="pae.png" lightbox="false" >}}

---

## Main Goal
* The DPD on GPU will take some power. 
* The DPD will allow for more efficient PA operation
* Figure out the tradeoff for DPD computation on GPU vs. # of PAs to net save power
  
 ---
## Old GPU Arch
{{< figure src="Kaipeng_GPU.PNG">}}

---
## New Idea

{{< figure src="board.jpg">}}

---
## New Idea
* 1024 IQ elements per block. Each gets a thread.
* Each PA will go onto new block
* Thread computes $|x(n)|^2$. Reuses result to calculate each polynomial order. $x(n)|x(n)|^4, ... $
* Store all of these results in a matrix in shared memory.
* Perform filtering and accumulation in combined matrix/vector multiplication step.

---
## Asilomar TODOs

* Make poster
* Compare NNs to Memory Polynomial

---
##  LDPC GPU for 5G NR
* Added support for BG2
* Need to make flexible by putting everything in constant memory if possible.

---
## PhD Proposal
* Started a DPD background / lit review section
