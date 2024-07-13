---
title: Cloud-Init Template Creation
description: 
published: 1
date: 2024-06-19T04:03:31.376Z
editor: markdown
dateCreated: 2024-06-10T04:21:31.688Z
---

Location for qcwo2 files are under `/var/lib/vz/template/iso/` on proxmox | [Repository for Alma Linux images](https://repo.almalinux.org/almalinux/9/cloud/x86_64/images/)

???+ info "Packages Required"
    - libguestfs-tools

Preparing and modidying the cloud image

```bash
# Enable a service by running a command
virt-customize -a AlmaLinux-9-GenericCloud-9.3-20231113.x86_64.qcow2 --run-command 'systemctl enable ssh.service'

# Change timezone 
virt-customize -a AlmaLinux-9-GenericCloud-9.3-20231113.x86_64.qcow2 --timezone UTC

# Install qemu-guest-agent
virt-customize -a AlmaLinux-9-GenericCloud-9.3-20231113.x86_64.qcow2 --install qemu-guest-agent 
```

Virtual machine configuration and creation

???+ info
    `--tablet=0` Disable pointer graphics to decreases CPU consumption

```bash
qm create 8000 --name almalinux-9-3-init --ostype l26 --tablet 0
qm set 8000 --net0 virtio,bridge=vmbr1,tag=30
qm set 8000 --memory 2064 --cores 2 --cpu host
qm set 8000 --scsi0 local-lvm:0,import-from="/var/lib/vz/template/iso/AlmaLinux-9-GenericCloud-9.3-20231113.x86\_64.qcow2",discard=on,ssd=1
qm set 8000 --boot order=scsi0 --scsihw virtio-scsi-single
qm set 8000 --agent enabled=1,fstrim_cloned_disks=1
qm disk resize 8000 scsi0 15G
```

Apply cloud-init settings

```bash
qm set 8000 --ide2 local-lvm:cloudinit
qm set 8000 --ipconfig0 "ip=dhcp"
qm set 8000 --sshkeys ~/.ssh/id_rsa.pub
qm set 8000 --ciuser root
qm set 8000 --cipassword hidden
```

Create the template

```bash
qm template 8000
```
