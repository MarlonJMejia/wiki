# Create a single interface network for Proxmox with OPNSense

# It's hacky!

Create a vlan for your interface `enp1s0` under /etc/network/interfaces.d/hacky:

??? warning

    Your interface(s) must alraedy be declared and automatically loaded under `/etc/network/interfaces`
    ```bash
    auto enp1s0
    iface enp1s0 inet manual
    ```

```bash title="/etc/network/interfaces.d/hacky"
auto enp1s0.5
iface enp1s0.5 inet manual
```

Create a bridge for your `enp1s0.5` interface:

```bash title="/etc/network/interfaces.d/hacky"
auto enp1s0.5
iface enp1s0.5 inet manual

auto vmbr2
iface vmbr2 inet static
        address 10.0.200.10/24
        bridge-ports enp1s0.5
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094
```

Apply your settings:

```bash
ifreload -a
```

# Add the new bridge port to OPNSense

First we need to view the configurationf or the VM

```bash
qm config 100 | grep -i net
```

```bash title="Output"
net0: virtio=AE:5A:81:62:D9:4A,bridge=vmbr1
net1: virtio=1A:A3:AD:32:C1:C2,bridge=vmbr0
```

Add the new bridge
```bash
qm set 100 -net10 model=virtio,bridge=vmbr2
```

Review the configuration once again to verify the interface has been added

```bash
qm config 100 | grep -i net
```
