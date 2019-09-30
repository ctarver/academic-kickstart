+++
title = "Xilinx DNNDK Workflow Example on IMAGENET"
date = 2019-08-29T09:32:55-05:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
# linktitle = "Example"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.petalinux]
  # Define a parent ID if this page is a parent.
  name = "DNNDK"
  
  # Reference a parent ID if this page is a child.
  # parent = "YourParentID"
  
  # Order that this page appears in the menu.
  weight = 1
+++

Xilinx  Deep Neural Network Development Kit (DNNDK) is their toolflow that can take Caffee or Keras/Tensorflow neural network (NN) designs and convert them to run on Deeplearning Processor Unit (DPU) in the FPGA fabric. 

They have a handful of guides [here](https://github.com/Xilinx/Edge-AI-Platform-Tutorials).
However, they mostly use small datasets like FMNIST or CIFAR10. I wanted to try it on something more impressive like [ImageNet](http://www.image-net.org/). 

## Workflow Overview

1. Train CNN in Keras on GPU-enable PC 
2. Quantize with Xilinx's `decent` tool.
3. Compile for Ultra96 using the `dnnc` tool.
4. Copy to Ultra96
5. Compile C code on Ultra96.
6. Run the CNN Inference!

## Hardware Used
We are using the [Ultra96v1 board](https://www.96boards.org/product/ultra96/) which has a [Xilinx Zynq UltraScale+ MPSoC ZU3EG A484](https://www.xilinx.com/support/documentation/selection-guides/zynq-ultrascale-plus-product-selection-guide.pdf). This Zynq has

* Quad Core ARM
* 7.6 MB of Block RAM
* 360 DSP Slices

## Software Setup.
The official Xilinx guide gives very little guidance on this.
Install a python 3.6 virtual environment. 
With the virtual environment activated, install the requirements.
I have collected a list of all the requirements that need to be install via PyPi [here](https://chancetarver.com/petalinux/requirements.txt). 
Download this as requirements.txt. 
```bash
pip install -r requirements.txt
```

## Helpful Links
* [DNNDK User Guide](https://www.xilinx.com/support/documentation/user_guides/ug1327-dnndk-user-guide.pdf)
* [Edge-AI-Platform-Tutorials on Github](https://github.com/Xilinx/Edge-AI-Platform-Tutorials).
* [Xilinx PetaLinux Tools Documentation](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2018_3/ug1144-petalinux-tools-reference-guide.pdf)
* [Xilinx Zynq-7000 All Programmable SoC: Embedded Design Tutorial](https://www.xilinx.com/support/documentation/sw_manuals/xilinx2018_3/ug1165-zynq-embedded-design-tutorial.pdf)
* [Zedboard Petalinux with Custom Hardware](https://fpgaw0rld.wordpress.com/2016/07/26/zedboard-petalinux-with-custom-hardware/)
* [Zynq design from scratch. Part 16.](http://svenand.blogdrives.com/archive/175.html#.V5crU7h96hc)
