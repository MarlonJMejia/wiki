* Get the current default

???+ note

    This can be overriden via grub2 by appending
    GRUB_CMDLINE_LINUX=`systemd.unit=multi-user.target`

```bash
systemctl get-default
```
This command will output something like graphical.target or multi-user.target, which correspond to the traditional run levels. Here’s a quick reference:

| Runlevel | Systemd Target      |
|----------|---------------------|
| 0        | poweroff.target     |
| 1        | rescue.target       |
| 3        | multi-user.target   |
| 5        | graphical.target    |
| 6        | reboot.target       |

* Switch to a different run level target

```bash
systemctl isolate graphical.target
```

```bash
init 5
```

* Current runlevel

```bash
runlevel
```
