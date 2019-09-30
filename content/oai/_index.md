+++
title = "OAI Tutorial"
date = 2018-12-10T08:24:55-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Main Guide"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  name = "Main Guide"
  
  # Reference a parent ID if this page is a child.
  # parent = "YourParentID"
  
  # Order that this page appears in the menu.
  weight = 1
+++

This is a tutorial for setting up my current OAI testbed. OpenAirInterface is a completely opensource software-based LTE and 5G NR framework. 

## Overview
This tutorial is split into two pages: the [core network installation]({{< ref "oai/cn.md" >}}) and the [eNB installation]({{< ref "oai/enb.md" >}}). This page serves as an overview showing how I am connecting the two.


## Hardware Used
In my setup, I am using three laptops. Two are acting as eNBs while the third is acting as the core network. I have all of these machines connected to a router. I prefer to do this so that I have better control over the network.

* 3 Computers running Ubuntu 16.04. We have ThinkPad P50s. 
* 2 USRP B210s. Maybe a 3rd if you want 2 eNBs and a UE.
* A COTS, android phone. Doesn't really matter what (I think. I'm using a Nexus 5X and Pixel 1). Being rooted may be helpful for debugging since you can get extra info from the phone using something like [MobileInsight](http://mobileinsight.net/).
* [Sim cards and sim card programmer.](https://open-cells.com/index.php/sim-cards/)
* A router. I like to do all my experiments on their own network. It gives me more control over IP addresses. 

## Overall Network Guide. 
Each computer gets assigned a 192.168.1.x IP address by my router. The core network runs a virtual machine containing multiple network interfaces bridged to the network via the host interface. Below is a diagram of this:

{{< figure src="../Network.PNG" title="OAI Network Setup" >}}
