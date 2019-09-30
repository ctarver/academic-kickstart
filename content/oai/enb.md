+++
title = "Installing the eNB"
date = 2018-12-10T08:41:19-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "eNB"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 2
+++

There are many recent changes to openairinterface5g that make many current guides to installing the repository difficult to follow. I will document the steps here for how I am building it.

We will use the latest commit of the [develop branch of the openairinterface-5g repository on Gitlab](https://gitlab.eurecom.fr/oai/openairinterface5g/tree/develop) which is commit b805cfc62862a1509645ad9cb16a53c5e6d7fa60. The most relevant guides that I used as a basis are the [official one from the OAI wiki](https://gitlab.eurecom.fr/oai/openairinterface5g/wikis/HowToConnectCOTSUEwithOAIeNBNew) and the [opencells guide](https://open-cells.com/index.php/2017/08/22/all-in-one-openairinterface-august-22nd/).

## Computer Setup
Setup a computer with Ubuntu. I am currently using [Ubuntu 16.04](http://releases.ubuntu.com/16.04/). The opencells guide uses 17.04, but this is more problematic since it is not a LTS; all the ppas are out of date so it's a pain to update anything. 

### Download the repo
```
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git
cd openairinterface5g
git checkout develop
```

## Install
```bash
cd openairinterface5g
./cmake_targets/build_oai -I -w USRP # Install dependencies
./cmake_targets/build_oai --eNB -w USRP # compile the enb
```

## Run
```
sudo bash
cd openairinterface5g; source oaienv
./cmake_targets/lte_build_oai/build/lte-softmodem -O <your config file>
```


