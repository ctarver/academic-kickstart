+++
title = "Debugging my Setup"
date = 2018-12-12T09:14:00-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Debugging Status"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 3
+++

## What Works
* eNB
* MME
* HSS
* SPGW
* UE Attachment

## What doesn't Work
* Data Connection on the Phone

## Summary of main issue
Currently, I run the core network on a virtual machine as shown in the [core network installation guide]({{< ref "oai/cn.md" >}}). The HSS, MME, and SPGW all launch and seem to work properly.

The COTS UE, a Pixel XL, will attach to the network. However, currently there is no data. We have a 4G connection without being able to access the internet.

## Interesting things from the log files:
The mme shows that there is no bearer:
```
[0m[0;32m001474 00206:494709 7F9712FFD700 DEBUG MME-AP -cn/src/mme_app/mme_app_bearer.c:1272         Received S11_MODIFY_BEARER_RESPONSE from S+P-GW
[0m[0;36m001475 00206:494743 7F9712FFD700 INFO  MME-AP -cn/src/mme_app/mme_app_bearer.c:1312         We could not find the first bearer with eBI 1 in the set of bearers. Ignoring the MBResp for UE with ueId: bcf53bf3. 
[0m[0;37m001476 00206:494752 7F9712FFD700 TRACE MME-AP -cn/src/mme_app/mme_app_bearer.c:1313      Leaving mme_app_handle_modify_bearer_resp() (rc=0)
[0m[0;32m001477 00210:293491 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0045      ======================================= STATISTICS ============================================

[0m[0;32m001478 00210:293546 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0046                     |   Current Status| Added since last display|  Removed since last display |
[0m[0;32m001479 00210:293558 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0048      Connected eNBs |          1      |              0              |             0               |
[0m[0;32m001480 00210:293569 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0050      Attached UEs   |          1      |              1              |             0               |
[0m[0;32m001481 00210:293579 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0052      Connected UEs  |          1      |              1              |             0               |
[0m[0;32m001482 00210:293587 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0054      Default Bearers|          0      |              0              |             0               |
[0m[0;32m001483 00210:293596 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0056      S1-U Bearers   |          0      |              0              |             0               |

[0m[0;32m001484 00210:293604 7F9712FFD700 DEBUG MME-AP src/mme_app/mme_app_statistics.c:0057      ======================================= STATISTICS ============================================
```

In the packet capture, you can see the UE at 12.1.1.2 trying to do DNS. I think the spotify app was running in the background.
{{< figure src="../UE_DNS.PNG" title="The UE seems to want to use spotify" >}}


### Log files
* [mme.log](../mme.log)
* [hss.log](../hss.log)
* [spgw.log](../spgw.log)
* [enb.log](../enb.log)

From a different run than the above, but showing the same issue:

* [Wireshark Packet Capture](../Capture_on_any_interface_debuging_cn_no_bearers.pcapng)

Not from current run. Needs to be updated:

* [ifconfig output](../ifconfig.txt)

### Config Files
* [cassandra.conf](../cassandra.conf.txt)
* [hss_rel14.conf](../hss_rel14.conf.txt)
* [hss_rel14.json](../hss_rel14.json)
* [mme.conf](../mme.conf.txt)
* [spgw.conf](../spgw.conf.txt)
* [acl.conf](../acl.conf.txt)
* [hss_rel14_fd.conf](../hss_rel14_fd.conf.txt)
* [mme_fd.conf](../mme_fd.conf.txt)

## Old Issues
For old issues and how I fixed them, see the [list of issues I had]({{< ref "oai/fixes.md" >}}).
