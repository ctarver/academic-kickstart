+++
title = "Using CLION To Debug OAI"
date = 2019-07-31T08:41:25-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "CLION"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Main Guide"
  
  # Order that this page appears in the menu.
  weight = 1
+++

[CLION](https://www.jetbrains.com/clion/) is a great IDE for working with any C project. I find it to be easier to use than Ecclipse. As I've worked with OAI, I've mostly just worked in VIM and on the command line. There is nothing wrong with VIM or other text based methods, but a full featrued IDE is much easier. 


## Pre Reqs
###Install CLION. 
There is a free version which will work though if you are a student, you can get a free licence for the full version. 
### Download OAI:
```
git clone https://gitlab.eurecom.fr/oai/openairinterface5g.git
```
### Install dependencies:
```
cd openairinterface5g
./cmake_targets/build_oai -I --eNB
```

## Main Setup
### Open CLion.
You should be able to launch this from the Ubuntu app launcher. 

### Create new CMAKE project from sources. 
Click "File >> New Cmake Project from Sources."
Navigate to the root of the openairinterface5g project. Click the checkboxes to include all files.

### Tell it which CMAKE to use. 
CLion will make its own CMakeLists.txt file in the root of the project. We don't want this. Right click on it and delete it. In the CMake tab on the bottom window, it will now complain that a CMakeLists.txt was not found. It will give an option to select one. Click it and select one of the targets such as cmake_targets/lte_simulators/CMakeLists.txt (I tried this with the main CMake_Lists.txt but had issues). You just need to select different CMmakeLists.txt depending on what you want to do. 

### Set up environment vars.
CLion will try to compile from the CMake and will fail. This is because it needs an environment variable. 
In the CMake tab, click the gear on the left hand toolbar. Click "CMake Settings." Then add this to the Environment field: "OPENAIR_DIR=/home/YOURUSERNAME/openairinterface5g"

### Change project root.
After you setup envirionment vars, CMake will recompile and fail. A little box will come up saying that some source files are outside the CMakeLists.txt directory. It will give an option to change the project root. Do that. Make it the actual root of the project, openairinterface5g. 

## Build!
All of the targets will be in the upper right hand dropdown box. Select one such as `dlsim.` 

