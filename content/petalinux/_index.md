+++
title = "Petalinux"
date = 2019-04-24T12:51:24-05:00
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
  name = "Main"
  
  # Reference a parent ID if this page is a child.
  # parent = "YourParentID"
  
  # Order that this page appears in the menu.
  weight = 1
+++
# Installation

## Dependencies


## Download
Download petalinux SDK from [Xilinx](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html). I downloaded 2018.3. 

## Installation. 
The petalinux file from Xilinx is a tar like ```petalinux-v2018.3-open_components.tar.gz```. Untar this somewhere. Make the petalinux-v2018.3-final-installer.run file executable by running ```chmod 777 petalinux-v2018.3-final-installer.run```.

I created a directory in my home folder for the installation. 
```bash
bash
cd 
mkdir peta_linux_2018
cd WHEREVER YOUR .RUN IS
mv petalinux-v2018.3-final-installer.run ~/peta_linux_2018/
cd ~/peta_linux_2018
./petalinux-v2018.3-final-installer.run
```

## Source the Files
After everything installs, you should have a settings.64 in the directory. To get the petalinux commands in your terminal, source this:
```bash
source settings.sh
```

## Create a blank project
Download a bsp for your board. Xilinx hosts some on their [downloads page](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-design-tools.html). I am downloading the "ZED BSP."
This gets a avnet-digilent-zedboard-v2018.3-final.bsp.


```bash
petalinux-create -t project --template zynq --name from_bsp -s ~/Desktop/avnet-digilent-zedboard-v2018.3-final.bsp 
cd /from_bsp
petalinux-config
```
This gives a configuration window like what is shown below:
{{< figure src="petalinux-config.png" title="Petalinux-config configuration window." >}}

{{%alert warning %}}
This is where I'm currently breaking. After I exit the above, I get:
```
[INFO] generating Kconfig for project
[INFO] menuconfig project


*** End of the configuration.
*** Execute 'make' to start the build or try 'make help'.

[INFO] sourcing bitbake
ERROR: Failed to source bitbake
ERROR: Failed to config project.
```
{{% /alert %}}



```bash
petalinux-create -t apps --name myapp --enable --template autoconf
petalinux-build -c kernel
petalinux-build -c rootfs
petalinux-build -c device-tree
petalinux-build -c u-boot
petalinux-build
petalinux-build -x package
petalinux-package --boot --fsbl ./images/linux/zynq_fsbl.elf  --fpga ./images/linux/design_1_wrapper.bit --uboot
```
## Create a project from an SDK hardware export

```bash
>> petalinx-create -t project --template zynq --name my_proj
cd /my_proj
petalinux-config --get-hw-desciption=../../Xilinx_Proj/project_ex/project_ex.sdk/
petalinux-create -t apps --name myapp --enable --template autoconf
petalinux-build -c kernel
petalinux-build -c rootfs
petalinux-build -c device-tree
petalinux-build -c u-boot
petalinux-build
petalinux-build -x package
petalinux-package --boot --fsbl ./images/linux/zynq_fsbl.elf  --fpga ./images/linux/design_1_wrapper.bit --uboot
```


## Other guides and links:

* [Xilinx Petalinux Getting Started](https://xilinx-wiki.atlassian.net/wiki/spaces/A/pages/18841618/PetaLinux+Getting+Started)
* [Instructables Getting Started with Petalinux](https://www.instructables.com/id/Getting-Started-With-PetaLinux/)
* [Salam!'s Beginner's Guide: Build a Petalinux Project for Zynq](https://assil.me/2017/10/24/building-a-petalinux-project.html)
