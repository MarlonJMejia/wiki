Additional Documentation can be found in `ansible-doc mount` and `ansible-doc parted`

# Create a partition

```yaml title="Create partition by Filesystem"
- name: Create a new ext4 primary partition
  community.general.parted:
    device: /dev/sdb
    number: 1
    state: present
    fs_type: xfs
```

## Create a LVM Volume from partition