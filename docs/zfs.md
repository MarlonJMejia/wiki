---
tags:
  - Drives
  - Disks
  - File Systems
  - RAID
---

# ZFS

ZFS is a combined file system and logical volume manager (designed by Sun Microsystems). ZFS is
scalable, and includes extensive protection against data corruption, support for high storage
capacities, efficient data compression, integration of the concepts of file system and volume
management, snapshots and CoW clones, continuous integrity checking and automatic repair, RAID Z,
native `NFSv4 ACLs`, and can be very precisely configured.


???+ Note "Reference(s)"
    * ⭐️ <https://openzfs.github.io/openzfs-docs/Getting%20Started/index.html>
    * ⭐️ <https://web.archive.org/web/20231110075538/https://blog.mikesulsenti.com/zfs-cheat-sheet-and-guide/>
    * <https://wiki.gentoo.org/wiki/ZFS>
    * <https://wiki.archlinux.org/index.php/ZFS>
    * <https://en.wikipedia.org/wiki/ZFS>

---
## Table of contents

<!-- vim-markdown-toc GitLab -->

* [Install](#install)
* [Config](#config)
* [Use](#use)
    * [sanoid](#sanoid)
    * [RAIDZ expansion](#raidz-expansion)

<!-- vim-markdown-toc -->

---
## Install

TODO

---
## Config

TODO

---
## Use

TODO

* Show disk space utilization info:
    ```console
    $ zfs list
    ```

* Show all properties for <POOLNAME> or <DATASET_NAME>:
    ```console
    $ zfs get all <POOLNAME>
    $ zfs get all <DATASET_NAME>
    ```

* Check zpool status of all pools with extra verbose information:
    ```console
    $ zpool status -v
    ```

* Check zpool status of specific pool <POOLNAME> with extra verbose information:
    ```console
    $ zpool status -v <POOLNAME>
    ```

* Show verbose information about pools filesystem statistics:
    ```console
    $ zpool list -v
    ```

* Show verbose IO statistics for all pools:
    ```console
    $ zpool iostat -v
    ```

* Show verbose IO statistics for a specific pool <POOLNAME>:
    ```console
    $ zpool iostat -v <POOLNAME>
    ```

* Show useful and advanced information on how ZFS's ARC Cache is being used:
    ```console
    $ arcstat
    
    $ arc_summary
    ```