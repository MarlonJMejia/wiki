# Create a partition

```yaml title="Create /dev/sdb1 with xfs and LVM flag with 50% of the block size"
- name: Create a new ext4 primary partition
  community.general.parted:
    device: /dev/sdb
    number: 1
    flags: [lvm]
    state: present
    part_start: 0%
    part_end: 50%
    fs_type: xfs

# Typically waiting a few seconds for the system to register the device is advice
- pause:
    seconds: 3
```

## Create an LVM Volume

```yaml
- name: Create a volume group on top of /dev/sda1
  lvg:
    vg: vg_database
    pvs: /dev/sdb1
    state: present

- name: Create a logical volume of 512g.
  lvol:
    vg: vg_database
    lv: lv_mysql
    size: 512M
    state: present

- name: Create a xfs filesystem on the logival volume
  filesystem:
    fstype: xfs
    dev: /dev/vg_database/lv_mysq

- name: Create a file on remote systems
  file:
    path: /mnt/mysql_backups
    state: directory

- name: Mount up device by label
  mount:
    path: /mnt/mysql_backups
    src: /dev/vg_database/lv_mysql
    fstype: xfs
    state: mounted
```
