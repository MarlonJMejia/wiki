---
title: Creating Swap
description: 
tags: swap linux/swap swappiness
date: 2024-08-15
time: 20:37
---

```
Note  that  a  swap  file  must  not contain any holes.  Using cp(1) to
create the file is not acceptable.  Neither is use of  fallocate(1)  on
file  systems  that support preallocated files, such as XFS or ext4, or
on copy-on-write filesystems like btrfs.   It  is  recommended  to  use
dd(1)  and  /dev/zero in these cases.  Please read notes from swapon(8)
before adding a swap file to copy-on-write filesystems.
```

* Create a file for swap, alternative a disk can be use but would be too inefficient. 

```shell
# Create a 2GiB swapfile
dd if=/dev/zero of=/swapfile bs=500MiB count=4 status=progress
```

* Enable the swapfile

```shell
chmod 0600 /swapfile
mkswap /swapfile
swapon /swapfile
swapon --show
```

* Editing /etc/fstab

```shell
/swapfile none swap defaults 0 0
```

> [!warning]  After updating /etc/fstab you need to `systemctl daemon-reload`

# Swappiness

When memory usage reaches a certain threshold, the kernel starts looking at active memory and seeing what it can free up. File data can be written out to the file system (if changed), unloaded and re-loaded later; other data must be written to swap before it can be unloaded.

To check the current swappiness value:

```shell
sysctl vm.swappiness
```

Alternatively, the file `/proc/sys/vm/swappiness` can be read in order to obtain the raw integer value.

To temporarily set the swappiness value:

```shell
sysctl -w vm.swappiness=35
```

To set the swappiness value permanently, create a [sysctl.d(5)](https://man.archlinux.org/man/sysctl.d.5) configuration file. For example:

```conf
/etc/sysctl.d/99-swappiness.conf
```