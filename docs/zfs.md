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
# Viewing Basics

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

---
# Handle Drives and Pools

ZFS Pools vdevs that can contain datasets, if a vdev fails the pool is considered failed.

Each RAID-Z vdev can have single, double or triple parity (called raidz/raidz1, raidz2 and raidz3), implying that it can withstand one, two or three disks failing at the same time, so generally speaking you would use higher parity the more disks your vdev is comprised of.


| Feature                | Stripe   | Mirror             | RAIDZ  | RAIDZ2 | RAIDZ3 | Stripe+Mirror                                  |
|------------------------|----------|--------------------|--------|--------|--------|------------------------------------------------|
| Min number of disks    | 1        | 2                  | 2      | 4      | 5      | 4                                              |
| Fault tolerance        | None     | (N-1) disk          | 1 disk | 2 disks| 3 disks| (N-1) disk in each N-disk mirror               |
| Disk space overhead    | None     | (N-1)/N            | 1 disk | 2 disks| 3 disks| (N-1)*P for P stripe over N-disk mirrors       |
| Read speed             | Fast     | Fast               | Slow, see below | Fast   |        |                                                |
| Write speed            | Fast     | Fair               | Slow, see below | Fair   |        |                                                |
| Hardware cost          | Cheap    | High to highest    | High   | Very high | Very High (disks) | High to highest                                |

* Craete a zfs pool

```bash
zfs create poolname raidlevel drives
```

* Attach a drive into a mirror pool

```bash
zpool attach poolname driveinpool newdrive
```

* Deatch a drive into a mirror pool

```bash
zpool detach poolname drive
```

* Replace a drive in a zfs pool

```bash
zpool replace poolname drive newdrive
```
