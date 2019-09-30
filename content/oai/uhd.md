+++
title = "Changing the UHD Version"
date = 2019-02-05T07:30:00-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "UHD Versions"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
# Define a parent ID if this page is a parent.
#name = "YourParentID"
  
# Reference a parent ID if this page is a child.
parent = "Other Stuff"
  
# Order that this page appears in the menu.
weight = 1
+++

I've found the UHD drivers to be a point of instability in OAI. Recently, the UHD drivers updated and broke my OAI setup.

** BAD = **  UHD 3.13.1.0-release 


These are the steps I needed to take to revert my UHD version.

## Purge Stuff:
```bash 
sudo apt purge uhd*
```

## Install Stuff:
On Ubuntu 16.04 systems, run:
```bash
sudo apt-get -y install git swig cmake doxygen build-essential libboost-all-dev libtool libusb-1.0-0 libusb-1.0-0-dev libudev-dev libncurses5-dev libfftw3-bin libfftw3-dev libfftw3-doc libcppunit-1.13-0v5 libcppunit-dev libcppunit-doc ncurses-bin cpufrequtils python-numpy python-numpy-doc python-numpy-dbg python-scipy python-docutils qt4-bin-dbg qt4-default qt4-doc libqt4-dev libqt4-dev-bin python-qt4 python-qt4-dbg python-qt4-dev python-qt4-doc python-qt4-doc libqwt6abi1 libfftw3-bin libfftw3-dev libfftw3-doc ncurses-bin libncurses5 libncurses5-dev libncurses5-dbg libfontconfig1-dev libxrender-dev libpulse-dev swig g++ automake autoconf libtool python-dev libfftw3-dev libcppunit-dev libboost-all-dev libusb-dev libusb-1.0-0-dev fort77 libsdl1.2-dev python-wxgtk3.0 git-core libqt4-dev python-numpy ccache python-opengl libgsl-dev python-cheetah python-mako python-lxml doxygen qt4-default qt4-dev-tools libusb-1.0-0-dev libqwt5-qt4-dev libqwtplot3d-qt4-dev pyqt4-dev-tools python-qwt5-qt4 cmake git-core wget libxi-dev gtk2-engines-pixbuf r-base-dev python-tk liborc-0.4-0 liborc-0.4-dev libasound2-dev python-gtk2 libzmq-dev libzmq1 python-requests python-sphinx libcomedi-dev python-zmq python-setuptools
```

## Clone Stuff:
```bash
git clone https://github.com/EttusResearch/uhd
```

## Checkout Stuff:
Next, checkout the desired UHD version. You can get a full listing of tagged releases by running the command:
```bash
cd uhd
git tag -l
```

Example: For UHD 3.13.0.1-release:
```bash
git checkout v3.13.0.1
```
 
## Build Stuff:
```
cd host
mkdir build
cd build
cmake ..
make -j 4 
```
I use 4 cores in the make command. If you have less than 4 cores (or more) change it so things can go fast!

## Install Stuff:

Next, install UHD, using the default install prefix, which will install UHD under the /usr/local/lib folder. You need to run this as root due to the permissions on that folder.
```bash
sudo make install
```

Next, update the system's shared library cache.
```bash
sudo ldconfig
```

Finally, make sure that the LD_LIBRARY_PATH environment variable is defined and includes the folder under which UHD was installed. Most commonly, you can add the line below to the end of your $HOME/.bashrc file:

```bash
export LD_LIBRARY_PATH=/usr/local/lib
```

The above step is very important, if you don't do the above step, the usrp_library.so file will not be linked with lte-softmodem and you will keep getting "Undefined symbol error" so please double check to make sure you have the above env variable on the path for library linking. 
 
## Download Stuff:
Locate this file "uhd_images_downloader.py​" using the below command and then
run it. It will download all the fpga images require to run the SDR.
```bash
python uhd_images_downloader.py
```

## Run OAI:
Run OAI and verify that it loads the new 
