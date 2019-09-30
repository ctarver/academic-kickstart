+++
title = "Profiling OAI"
date = 2019-03-10T12:00:00-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Profiling"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Other Stuff"
  
  # Order that this page appears in the menu.
  weight = 2
+++

I've been studying the OAI architecture. Part of that is profiling the code. 

## GPROF
To use gprof, I modifyed the cmake_targets/CmakeLists.txt file.
I just appended this to the top:
```
set(CMAKE_CXX_FLAGS   ${CMAKE_CXX_FLAGS} "-pg")
```

I then compiled it like normal.
```
./build_oai --eNB -w USRP
```

I ran the lte-softmodem for a few seconds. This generates an output ```gmon.out``` that we can load into gprof. 

```
gprof ./lte_build_oai/build/lte-softmodem gmon.out > ~/profile_results
```

The output is [here](../profile_results.txt).
THIS IS PROBABLY WRONG. IT PROBABLY IS ONLY THE MAIN THREAD.

## Valgrind
Valgrind is way more powerful! 
Install valgrind and a few other packages.
```
sudo apt install valgrind kcachegrind graphviz
```

Compile OAI with gdb flags:
```
./cmake_targerts/build_oai -g --eNB -w USRP
```

Run OAI through valgrind with callgrind on:
```
sudo -E valgrind --tool=callgrind ./cmake_targets/lte_build_oai/build/lte-softmodem -O YOUR.CONF
```

This will generate an output like ```callgrind.out.PID```

Change the permisions of this ```sudo chmod 777 callgrind.out.PID``` since you ran the lte-softmodem as sudo.

Open this in kcachegrind. Just launch kcachegrind in the terminal then click open. My callgrind output is [here](../callgrind.out.20042). Once you open this, you can generate call graphs for any function! 

!()[../init_SI.png]
