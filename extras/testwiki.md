---
title: Test Page
description: 
published: true
date: 2021-08-30T11:52:14.754Z
tags: 
editor: markdown
dateCreated: 2021-08-30T11:52:14.754Z
---



# Snap

## Removing Snap

1. List all snaps

```prolog
snap list
```

2. Remove the snaps manually

```bash
sudo snap remove *
```

3. Unmount the `/snap`

**Ubuntu 20.04 LTS**

You'll need to replace the xxxx with the actual ID inside the core directory on your system, which you can find out by running df

```bash
sudo umount /snap/core/xxxx
```

**Ubuntu 20.10**

In Ubuntu 20.10, I found that this is now under /var/snap. Simply run:

```bash
sudo umount /var/snap
```


4. Remove and purge snap

```bash
sudo apt purge snapd
```


5. Remove the deadweight

```bash
rm -rf ~/snap
sudo rm -rf /snap
sudo rm -rf /var/snap
sudo rm -rf /var/lib/snapd
```



:::info
You would have to remove the directories for different users manually (eg. wantguns, root, etc.)

:::


### References

<https://www.kevin-custer.com/blog/disabling-snaps-in-ubuntu-20-04/>

