+++
title = "Saving your log files so that they are pretty"
date = 2019-02-12T11:00:00-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "Pretty Logs"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Other Stuff"
  
  # Order that this page appears in the menu.
  weight = 1
+++

I've been posting logs on my website to share them with other OAI users. However, they are always stdout/stderr ugly outputs with no colors.
However, when OAI is actually running, there are nice ANSI colors that help focus your attention to useful info. 

If you are using ```tee`` like I was, you'd probably like a better way to save colorful log outputs.

## Install ANSI2HTML
```bash
sudo apt install kbtin
```

## Run stuff with pretty outputs!
```bash
./cmake_targets/lte_build_oai/buile/lte_softmodem -O CONF_FILE |& ansi2html | tee enb_pretty.html
```

## See my example output
[Here](../enb_pretty.html)
