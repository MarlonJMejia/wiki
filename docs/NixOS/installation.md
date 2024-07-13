## DiskIO
> Automatically prepare the disk partitions for the disks

[Examples](https://github.com/nix-community/disko/tree/master/example)

```bash
sudo nix --experimental-features "nix-command flakes" run github:nix-community/disko -- --mode disko /tmp/disk-config.nix
```

## Install Nixos

```bash
nixos-generate-config --root /mnt && nixos-install
```
