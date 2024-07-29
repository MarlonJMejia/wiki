---
tags:
  - Drives
  - Disks
  - File Systems
---

# File systems

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Category:Filesystems>
    * <https://wiki.gentoo.org/wiki/Filesystem>
    * <https://wiki.archlinux.org/index.php/File_systems>
    * <https://www.phoronix.com/scan.php?page=news_item&px=Linux-5.14-File-Systems>


---
## Table of contents

<!-- vim-markdown-toc GitLab -->

- [File systems](#file-systems)
  - [Table of contents](#table-of-contents)
  - [FUSE](#fuse)
  - [Ext4](#ext4)
  - [Btrfs](#btrfs)
  - [XFS](#xfs)
  - [ZFS](#zfs)
  - [ReiserFS](#reiserfs)
  - [MergerFS](#mergerfs)
  - [F2FS](#f2fs)
  - [HFS/HFS+](#hfshfs)
  - [JFS](#jfs)
  - [FAT](#fat)
  - [ExFAT](#exfat)
  - [NTFS](#ntfs)
  - [NFS](#nfs)
  - [SSHFS](#sshfs)
  - [`sysfs`](#sysfs)
  - [`tmpfs`](#tmpfs)
  - [`procfs`](#procfs)

<!-- vim-markdown-toc -->

---
## FUSE

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Filesystem_in_Userspace>
    * <https://wiki.archlinux.org/index.php/FUSE>
    * <https://en.wikipedia.org/wiki/Filesystem_in_Userspace>

FUSE is a software interface for Unix and Unix like computer operating systems that lets
non privileged users create their own file systems without editing kernel code.  This is achieved
by running file system code in user space while the FUSE module provides only a "bridge" to the
actual kernel interfaces.


---
## Ext4

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Ext4>
    * <https://wiki.archlinux.org/index.php/Ext4>
    * <https://en.wikipedia.org/wiki/Ext4>

The ext4 journaling file system or fourth extended file system is a journaling file system for
Linux, developed as the successor to ext3.


---
## Btrfs

See [Btrfs](./btrfs.md)


---
## XFS

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/XFS>
    * <https://wiki.archlinux.org/index.php/XFS>
    * <https://en.wikipedia.org/wiki/XFS>

XFS is a high performance 64-bit journaling file system created by Silicon Graphics Inc in 1993.
XFS was ported to the Linux kernel in 2001; as of June 2014, XFS is supported by most Linux
distributions, some of which use it as the default file system.


---
## ZFS

See [ZFS](./zfs.md)


---
## ReiserFS

???+ Note "Reference(s)"
    * <https://en.wikipedia.org/wiki/ReiserFS>

ReiserFS is a general-purpose, journaled computer file system initially designed and implemented by
a team led by Hans Reiser.


---
## MergerFS

???+ Note "Reference(s)"
    * <https://github.com/trapexit/mergerfs>

MergerFS is a union file system geared towards simplifying storage and management of files across
numerous commodity storage devices. It is similar to MHDDFS, UnionFS, and AUFS.


---
## F2FS

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/F2FS>
    * <https://wiki.archlinux.org/index.php/F2FS>
    * <https://en.wikipedia.org/wiki/F2FS>

F2FS is a flash file system initially developed by Samsung Electronics for the Linux kernel.


---
## HFS/HFS+

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/HFS>
    * <https://wiki.gentoo.org/wiki/HFS%2B>
    * <https://en.wikipedia.org/wiki/Hierarchical_File_System>
    * <https://en.wikipedia.org/wiki/HFS_Plus>

HFS is a proprietary file system developed by Apple Inc. for use in computer systems running Mac
OS.

HFS Plus or HFS+ is a journaling file system developed by Apple Inc. It replaced the HFS as the
primary file system of Apple computers with the 1998 release of Mac OS 8.1.


---
## JFS

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/JFS>
    * <https://wiki.archlinux.org/index.php/JFS>
    * <https://en.wikipedia.org/wiki/JFS_%28file_system%29>

JFS is a journaling file system that was open sourced by IBM in 1999 and support for which has been
available in the Linux kernel since 2002.


---
## FAT

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/FAT>
    * <https://wiki.archlinux.org/index.php/FAT>
    * <https://en.wikipedia.org/wiki/File_Allocation_Table>

FAT is a computer file system architecture and a family of industry standard file systems utilizing
it. The FAT file system is a continuing standard which borrows source code from the original,
legacy file system and proves to be simple and robust.


---
## ExFAT

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/ExFAT>
    * <https://en.wikipedia.org/wiki/ExFAT>

ExFAT is a file system introduced by Microsoft in 2006 and optimized for flash memory such as USB
flash drives and SD cards. ExFAT is proprietary, and Microsoft owns patents on several elements of
its design.


---
## NTFS

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/NTFS>
    * <https://wiki.archlinux.org/index.php/NTFS-3G>
    * <https://en.wikipedia.org/wiki/NTFS>

NTFS is a proprietary disk file system by Microsoft for Windows and Windows based operating systems.


---
## NFS

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Nfs-utils>
    * <https://wiki.archlinux.org/index.php/NFS>
    * <https://wiki.archlinux.org/index.php/NFS/Troubleshooting_>
    * <https://en.wikipedia.org/wiki/Network_File_System>

NFS is a distributed file system protocol originally developed by Sun Microsystems in 1984,
allowing a user on a client computer to access files over a computer network much like local
storage is accessed.


---
## SSHFS

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/SSHFS>
    * <https://wiki.archlinux.org/index.php/SSHFS>
    * <https://en.wikipedia.org/wiki/SSHFS>

SSHFS is a FUSE based file system client for mounting remote directories over a Secure Shell
connection.


---
## `sysfs`

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Sysfs>
    * <https://en.wikipedia.org/wiki/Sysfs>

`sysfs` is a pseudo file system provided by the Linux kernel that exports information about various
kernel subsystems, hardware devices, and associated device drivers from the kernel's device model
to user space through virtual files. In addition to providing information about various devices and
kernel subsystems, exported virtual files are also used for their configuration.


---
## `tmpfs`

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Tmpfs>
    * <https://wiki.archlinux.org/index.php/Tmpfs>
    * <https://en.wikipedia.org/wiki/Tmpfs>

`tmpfs` is a temporary file storage paradigm implemented in many Unix like operating systems. It is
intended to appear as a mounted file system, but data is stored in volatile memory instead of a
persistent storage device. A similar construction is a RAM disk, which appears as a virtual disk
drive and hosts a disk file system.


---
## `procfs`

???+ Note "Reference(s)"
    * <https://wiki.gentoo.org/wiki/Procfs>
    * <https://wiki.archlinux.org/index.php/Procfs>
    * <https://en.wikipedia.org/wiki/Procfs>

`procfs` is a special file system in Unix like operating systems that presents information about
processes and other system information in a hierarchical file like structure, providing a more
convenient and standardized method for dynamically accessing process data held in the kernel than
traditional tracing methods or direct access to kernel memory.

Typically, it is mapped to a mount point named `/proc` at boot time. `procfs` acts as an interface
to internal data structures in the kernel. It can be used to obtain information about the system
and to change certain kernel parameters at runtime (`sysctl`).
