+++
title = "CQL Cheat Sheet"
date = 2018-12-17T13:02:19-06:00
draft = false
type = "docs"  # Do not modify.

# Show table of contents? true/false
toc = true

# Add menu entry to sidebar.

# Uncomment to customize menu title, e.g. to abbreviate page title.
linktitle = "CQL Cheat Sheet"

# Substitute `tutorial` with the name of your tutorials folder.
[menu.oai]
  # Define a parent ID if this page is a parent.
  #name = "YourParentID"
  
  # Reference a parent ID if this page is a child.
  parent = "Other Stuff"
  
  # Order that this page appears in the menu.
  weight = 6
+++

The hss uses Cassandra as the database. I do not use SQL/CQL or other databases every day, so this is a cheat sheet of CQL commands for use with the hss.

## Launch Command Prompt

```bash
cqlsh
```

## See all details

```sql
DESCRIBE vhss;
```

## Switch to VHSS database
```sql
USE vhss;
```

## Print all user info 

```sql
SELECT * FROM users_imsi;
```

## Print the info for a specific user
```sql
SELECT * FROM users_imsi WHERE imsi='208920100001101';
```

## Update an OPc for a specific user
```sql
UPDATE users_imsi SET OPc='4a136435f62f0c9de71875bcf245a0a8' WHERE imsi='208920100001101';
```

## See the current SQN for a specific user
```sql
SELECT sqn FROM users_imsi WHERE imsi='208920100001101';
```

## Update SQN for a specific user
```sql
UPDATE users_imsi SET sqn=YOURINTHERE WHERE imsi='208920100001101';
```
