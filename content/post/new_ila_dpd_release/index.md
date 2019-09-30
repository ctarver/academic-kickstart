---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "New Version of my DPD Matlab Library Available"
subtitle: ""
summary: ""
authors: []
tags: ["DPD", "MATLAB", "RFWebLab"]
categories: []
date: 2019-09-29T19:30:32-05:00
lastmod: 2019-09-29T19:30:32-05:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "PSD plot of 20 MHz OFDM signal with no DPD, 11th order MP, and 11th order GMP."
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

I released a new verison of my DPD library for Matlab yesterday. You can find the source code [here](https://github.com/ctarver/ILA-DPD).
This software can enable you to easily add DPD to any of your Matlab based wireless prototyping. 

This new version now supports the [Generalized Memory Polynomial (GMP)](https://ieeexplore.ieee.org/document/1703853). This is a more expressive model than the standard memory polynomial. 
The extra memory effects captured in this model are especially helpful for signals with larger bandwidth such as what we see in 5G NR. 

The GMP is shown below.

\begin{align} y_{GMP}(n) = & \sum\_{p \in P_a} \sum\_{m \in M_a} a\_{pm} x(n-m) \left| x(n-m)  \right|^{p-1}  +  \\\\ 
& \sum\_{p \in P_b} \sum\_{m \in M_b} \sum\_{l \in L_b} b\_{pm} x(n-m) \left| x(n-m-l)  \right|^{p-1}  +  \\\\ 
& \sum\_{p \in P_c} \sum\_{m \in M_c} \sum\_{l \in L_c} c\_{pm} x(n-m) \left| x(n-m+l)  \right|^{p-1} \end{align}

Here, the 1st line is the standard memory polynomial. The second two lines are the ones added to create the GMP. 
A delay of $l$ samples is inserted between the signal and the exponential envelope.


An example using this is shown at the top of this page.
Here, the green is an 11th order memory polynomial with 4 memory taps. The blue is an 11th order GMP with 4 memory taps and a lag/lead factor of 1. We see that it offers slightly more surpression of the adjacent channel leakage than the MP. This test was performed using [RFWebLab](http://dpdcompetition.com/rfweblab/) which provides a PA with a Matlab API.

To download the package, see the repository on my GitHub [here](https://github.com/ctarver/ILA-DPD).
This can be used by simply including the [ILA_DPD.m](https://github.com/ctarver/ILA-DPD/blob/master/ILA_DPD.m) file in your project. This provides a DPD class. See the [example.m](https://github.com/ctarver/ILA-DPD/blob/master/example.m) script for an example of using it. 

If you use this package in your work, please cite it. My recommened citation is below:
```
@misc{TarverILADPD,
  author       = {Tarver, Chance},
  title        = {GMP DPD Using Indirect Learning Architecture Matlab Library},
  month        = sep,
  year         = 2019,
  doi          = {10.5281/zenodo.3463608 },
}
```
