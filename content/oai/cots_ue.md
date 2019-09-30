+++
title = "Setting up the COTS UE"
date = 2018-12-17T16:02:01-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "COTS UE"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 3
+++

There are a few steps to get the COTS UE to work with the network. We need to program the SIM card then properly set up the APN.

## Program the SIM CARD
Using the uicc tool from opencells we can program the SIM card. Replace each of the parameters below with those appropriate for your network.

```bash
sudo ./program_uicc --adm 68954302 --authenticate --key 6874736969202073796d4b2079650a73 --opc 504f20634f6320504f50206363500a4f --spn ChanceWireless
```

One of the outputs will be the next SQN number. Save this to put in your HSS.


## Setting up the APN
On the phone, go to Settings > Network and Internet > Mobile Network > Advanced > Access Point Names.

Add an APN. Set the name to whatever you like, the APN to "default," the APN protocol to IPv4, and the Bearer to "LTE." Save this by clicking the three dots in the top-right corner and then clicking "Save."

## Connecting to the Network
With the core network and the eNB running, you can proceed to connect to your network. I find it is not reliable in automatically connecting. I usually turn off the automatic connection setting and then proceed to choose the network myself. The network shows up as "20892."

Once the phone registers, you should have working 4G data to the COTS UE.

