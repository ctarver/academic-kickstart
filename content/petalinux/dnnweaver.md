+++
title = "Dnnweaver"
date = 2019-04-25T09:32:55-05:00
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
  name = "DNNWeaver"
  
  # Reference a parent ID if this page is a child.
  # parent = "YourParentID"
  
  # Order that this page appears in the menu.
  weight = 1
+++
I am using version 1 of this project here: https://bitbucket.org/hsharma35/dnnweaver.public/overview

# Dependencies
I installed all the same versions of Vivado and petalinux as that guide, 2016.2.

# Download

```
git clone https://bitbucket.org/hsharma35/dnnweaver.public.git
```

# Modifications 
## TCL Script
I am using a Zedboard. To add support for this to the tcl script, I modified the fpga/tcl/vivado_2016.2.tcl on line 69.
Below shows  the diff. 
```
-set_property "board_part" "xilinx.com:zc702:part0:1.2" $obj
+set_property "board_part" "em.avnet.com:zed:part0:1.3" $obj
```

## dnn_accelerator.v
For unknown reasons, I get an erorr complaining about a ROM_ADDR_W param whenever I try to make the project. To overcome this, I had to explicitly add this parameter to the /fpga/hardware/source/dnn_accelerator/dnn_accelerator.v file. I made the following addition at line 22.
```
parameter integer ROM_ADDR_W = 3,
```

## Downgrade Libssl
I was getting an error when I'd run petalinux-build. The error is discussed here: https://forums.xilinx.com/t5/Embedded-Linux/U-Boot-compile-error-dereferencing-pointer/td-p/794782
To fix this, I had to downgrade libssl.
```
sudo apt install libssl1.0-dev
```

# Compile the project

# Make the Petalinux project
In the ```synthesis-output``` directory, create the project.
```
petalinux-create -t project --template zynq --name petalinux_project
```

This will create a new directory called petalinux_project.

## Configure the petalinux
cd into the petalinux_project. 
Run this to configure it by pointing it to the synthesis-output directory where the hdf for the project should be.
```
petalinux-config --get-hw-description=".." 
```

When the config window popped up, I just saved it. This leaves everything as the default which idk if that's ok or not.

## petalinux-build
Run ```petalinux-build```.

My console output is shown below:
```
chance@chance-XPS13-9333:~/Desktop/dnnweaver.public/fpga/synthesis-output/petalinux_project$ petalinux-build 
INFO: Checking component...
INFO: Generating make files and build linux
INFO: Generating make files for the subcomponents of linux
INFO: Building linux
[INFO ] pre-build linux/rootfs/fwupgrade
[INFO ] pre-build linux/rootfs/peekpoke
[INFO ] build linux/kernel
[INFO ] generate linux/u-boot configuration files
[INFO ] update linux/u-boot source
[INFO ] build linux/u-boot
[INFO ] build zynq_fsbl
[INFO ] building /home/chance/Desktop/dnnweaver.public/fpga/synthesis-output/petalinux_project/build/linux/rootfs/packages-repo/pkgs_sysroot_list
[INFO ] do_gen_sysroot
[INFO ] build linux/rootfs/fwupgrade
[INFO ] build linux/rootfs/peekpoke
[INFO ] build kernel in-tree modules
[INFO ] modules linux/kernel
[INFO ] post-build linux/rootfs/fwupgrade
[INFO ] post-build linux/rootfs/peekpoke
[INFO ] pre-install linux/rootfs/fwupgrade
[INFO ] pre-install linux/rootfs/peekpoke
[INFO ] install device trees
[INFO ] install linux/kernel
[INFO ] generate linux/u-boot configuration files
[INFO ] update linux/u-boot source
[INFO ] build linux/u-boot
[INFO ] install linux/u-boot
[INFO ] do_gen_baserootfs
[INFO ] install sys_init
[INFO ] install linux/rootfs/fwupgrade
[INFO ] install linux/rootfs/peekpoke
[INFO ] install kernel in-tree modules
[INFO ] modules_install linux/kernel
[INFO ] post-install linux/rootfs/fwupgrade
[INFO ] post-install linux/rootfs/peekpoke
[INFO ] package rootfs.cpio to /home/chance/Desktop/dnnweaver.public/fpga/synthesis-output/petalinux_project/images/linux
[INFO ] Update and install vmlinux image
[INFO ] vmlinux linux/kernel
[INFO ] install linux/kernel
[INFO ] package zImage
[INFO ] zImage linux/kernel
[INFO ] install linux/kernel
[INFO ] Package HDF bitstream
[INFO ] Failed to copy images to TFTPBOOT /tftpboot
```

