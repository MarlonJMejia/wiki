---
title: Kernel
description: 
tags: linux/kernel linux/grubby linux/grub
date: 2024-08-17
time: 10:33
---

* Current Kernel information

```shell
grubby --info="/boot/vmlinuz-$(uname -r)"
```

* Displaying all grub2 kernel options and their index

```shell
grubby --info=ALL | grep -E "^kernel|^index"
```

* Set a specific kernel to load at boot
  
```shell
# via index
grubby --set-default-index=2
```

```shell
# via vmlinuz file
grubby --set-default="/boot/vmlinuz-4.18.0-193.1.2.el8_2.x86_64"
```

*  Create a new boot entry

```shell
grubby --grub2 --add-kernel=/boot/vmlinuz-4.18.0-193.el8.x86_64 --title="Red Hat Enterprise 8 Test" --initrd=/boot/initramfs-4.18.0-193.el8.x86_64.img --copy-default
```

* Adding new options/arguments to the kernel boot entry

```shell
grubby --update-kernel=/boot/vmlinuz-$(uname -r) --remove-args="quiet" --args="console=ttsy0"
```
  
  