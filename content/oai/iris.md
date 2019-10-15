+++
title = "Using an Iris as the SDR"
date = 2019-02-05T15:00:00-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Iris"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Other Stuff"
  
  # Order that this page appears in the menu.
  weight = 1
+++

At Rice, we have a strong relationship with [Skylark Wireless](http://www.skylarkwireless.com/) through the [RENEW Project](http://renew.rice.edu/).

See more information about [Renew and IRIS here](https://obejarano.gitlab.io/renew-documentation/getting-started/hardware/).

They have recently pushed support for their Iris SDR to OAI so I am going to test it.

# Installation:
I'm installing the eNB the usual ways:
```bash
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git
git checkout develop
cd openairinterface5g
./cmake_targets/build_oai -I -w IRIS
./cmake_targets/build_oai --eNB -w IRIS
```

The ```-I -w IRIS``` command may give some messages saying that some packages related to soapysdr aren't found. Don't worry about it; it'll actually download and install them from source. 

I'm running this using the latest master branch, version 1.1, commit 7af841272fbe9954e5b92015c0411e1634b0381d. 

## Connect the board. 
Connect the IRIS board to the NIC on your machine. My MME is on my LAN, so I used a USB to ethernet adapter for connecting to the LAN.

The IRIS is set up to get its IP via DHCP, so we need to set up our interface to assign an IP to the IRIS. 

Check to see that it is discovered.
```bash
SoapySDRUtil --find
```
This command should discover any IRIS that is connected. I found that the discovery wasn't reliable, and I needed to run it a few times for it to show up. When it does, a serial number is presented. You will need this serial number for the config file.

You should also be able to ssh to the IRIS. The default username and pwd are `sklk`.


# Config File:
In the current master branch, there are a variety of config files in the folder "openairinterface/targets/PROJECTS/GENERIC-LTE-EPX/CONF/." 

I like to copy my config to the root of the repository:
```bash
cp targets/PROJECTS/GENERIC-LTE-EPC/CONF/enb.band7.tm1.25PRB.iris030.conf .
```
I needed to change was the IP address of the MME on line 174, chnage the eNB addresses and interfaces to match my LAN connection on the machine, and replace the serial number on line 231 with the one shown in the ```SoapySDRUtil --find``` command.

# Run!
With the IRIS on, start OAI:
```bash
sudo bash
./cmake_targets/lte_build_oai/build/lte-softmodem -O enb.band7.tm1.25PRB.iris030.conf
```

# Error:
It looks like OAI can't find my IRIS. The full log file is available [here](../iris.log)
