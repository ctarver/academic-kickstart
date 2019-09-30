---
title: "Digital predistortion with low-precision ADCs"
date: 2017-01-01
publishDate: 2019-09-28T19:40:41.172391Z
authors: ["Chance Tarver", "Joseph R Cavallaro"]
publication_types: ["1"]
abstract: "Digital Predistortion (DPD) is a popular technique for linearizing a power amplifier (PA) to help reduce the spurious emissions and spectral regrowth. DPD requires the learning of the inverse PA nonlinearities by training on the output of the PA. In practical systems, the analog output of the PA will have to go through an analog-to-digital converter (ADC) so that training can be done on a digital processor. The quantization degrades signal quality and may limit the performance of a DPD learning algorithm. However, a lower resolution ADC may cost less and allow for less computational complexity in the digital processing. We study this trade-off to try to find how much precision is needed in DPD systems and discover that for a full-band DPD as few as 6 bits can reliably be used. For sub-band DPD, a single bit ADC can be used."
featured: false
publication: "*2017 51st Asilomar Conference on Signals, Systems, and Computers*"
doi: 10.1109/ACSSC.2017.8335381
tags: ["DPD", "WARP"]
url_pdf: "https://github.com/ctarver/Asilomar2017/raw/master/TEX/main.pdf"
url_code: "https://github.com/ctarver/Asilomar2017"
url_poster: "https://github.com/ctarver/Asilomar2017/raw/master/POSTER/main.pdf"
---

