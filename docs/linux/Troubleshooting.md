# General Commands

What type of system? 

```bash
dmidecode -s system-manufacturer
systemd-detect-virt
```

Biggest file?

```bash
du -mxS / |sort -n|tail -10
dua i
ncdu
```

Does the system needs to restarted?

```bash
cat /var/run/reboot-required /run/reboot-required
cat /var/run/reboot-required.pkgs /run/reboot-required.pkgs
needrestart
```

### vmstat

> Report information about processes, memory, paging, block IO, traps, disks and CPU activity.
> More information: <https://manned.org/vmstat>.

???+ example
    - Display virtual memory statistics:

    ```bash
    vmstat
    ```

    - Display reports every 2 seconds for 5 times:

    ```bash
    vmstat 2 5
    ```

# CPU & PROCESSES

### mpstat

> Report CPU statistics.
> More information: <https://manned.org/mpstat>.

???+ example
    - Display CPU statistics every 2 seconds:

    ```
    mpstat 2
    ```

    - Display 5 reports, one by one, at 2 second intervals:

    ```
    mpstat 2 5
    ```

    - Display 5 reports, one by one, from a given processor, at 2 second intervals:

    ```
    mpstat -P 0 2 5
    ```

### pidstat

> Show system resource usage, including CPU, memory, IO etc.
> More information: <https://manned.org/pidstat>.

??? example
    - Show CPU statistics at a 2 second interval for 10 times:

    ```bash
    pidstat 2 10
    ```

    - Show page faults and memory utilization:

    ```bash
    pidstat -r
    ```

    - Show input/output usage per process ID:

    ```bash
    pidstat -d
    ```

    - Show information on a specific PID:

    ```bash
    pidstat -p PID
    ```

    - Show memory statistics for all processes whose command name include "fox" or "bird":

    ```bash
    pidstat -C "fox|bird" -r -p ALL
    ```

### iostat

> Report statistics for devices and partitions.
> More information: <https://manned.org/iostat>.

??? example
    - Display a report of CPU and disk statistics since system startup:

    ```bash
    iostat
    ```

    - Display a report of CPU and disk statistics with units converted to megabytes:

    ```bash
    iostat -m
    ```

    - Display CPU statistics:

    ```bash
    iostat -c
    ```

    - Display disk statistics with disk names (including LVM):

    ```bash
    iostat -N
    ```

    - Display extended disk statistics with disk names for device "sda":

    ```bash
    iostat -xN sda
    ```

    - Display incremental reports of CPU and disk statistics every 2 seconds:

    ```bash
    iostat 2
    ```

# Networking

### sar

> Monitor performance of various Linux subsystems.
> More information: <https://manned.org/sar>.

??? example
    - Report I/O and transfer rate issued to physical devices, one per second (press CTRL+C to quit):

    ```bash
    sar -b {{1}}
    ```

    - Report a total of 10 network device statistics, one per 2 seconds:

    ```bash
    sar -n DEV {{2}} {{10}}
    ```

    - Report CPU utilization, one per 2 seconds:

    ```bash
    sar -u ALL {{2}}\
    ```

    - Report a total of 20 memory utilization statistics, one per second:

    ```bash
    sar -r ALL {{1}} {{20}}
    ```

    - Report the run queue length and load averages, one per second:

    ```bash
    sar -q {{1}}
    ```

    - Report paging statistics, one per 5 seconds:

    ```bash
    sar -B {{5}}
    ```

### ss

> Utility to investigate sockets.
> More information: <https://manned.org/ss.8>.

??? example
    - Show all TCP/UDP/RAW/UNIX sockets:

    ```bash
    ss -a -t|-u|-w|-x
    ```

    - Filter TCP sockets by states, only/exclude:

    ```bash
    ss state/exclude bucket/big/connected/synchronized/...
    ```

    - Show all TCP sockets connected to the local HTTPS port (443):

    ```bash
    ss -t src :443
    ```

    - Show all TCP sockets listening on the local 8080 port:

    ```bash
    ss -lt src :8080
    ```

    - Show all TCP sockets along with processes connected to a remote SSH port:

    ```bash
    ss -pt dst :ssh
    ```

    - Show all UDP sockets connected on specific source and destination ports:

    ```bash
    ss -u 'sport == :source_port and dport == :destination_port'
    ```

    - Show all TCP IPv4 sockets locally connected on the subnet 192.168.0.0/16:

    ```bash
    ss -4t src 192.168/16
    ```

    - Kill IPv4 or IPv6 Socket Connection with destination IP 192.168.1.17 and destination port 8080:

    ```bash
    ss --kill dst 192.168.1.17 dport = 8080
    ```

### tcpdump

> Dump traffic on a network.
> More information: <https://www.tcpdump.org>.
> 
??? example
    - List available network interfaces:

    ```bash
    tcpdump -D
    ```

    - Capture the traffic of a specific interface:

    ```bash
    tcpdump -i eth0
    ```

    - Capture all TCP traffic showing contents (ASCII) in console:

    ```bash
    tcpdump -A tcp
    ```

    - Capture the traffic from or to a host:

    ```bash
    tcpdump host www.example.com
    ```

    - Capture the traffic from a specific interface, source, destination and destination port:

    ```bash
    tcpdump -i eth0 src 192.168.1.1 and dst 192.168.1.2 and dst port 80
    ```

    - Capture the traffic of a network:

    ```bash
    tcpdump net 192.168.1.0/24
    ```

    - Capture all traffic except traffic over port 22 and save to a dump file:

    ```bash
    tcpdump -w dumpfile.pcap port not 22
    ```

    - Read from a given dump file:

    ```bash
    tcpdump -r dumpfile.pcap
    ```

# Systemd and Kernel

### dmesg

> Write the kernel messages to `stdout`. More information: <https://manned.org/dmesg>.

???+ example

    - Show kernel error messages:

    ```bash
    dmesg --level err
    ```

    - Show kernel messages and keep reading new ones, similar to `tail -f` (available in kernels 3.5.0 and newer):

    ```bash
    dmesg -w
    ```

    - Show how much physical memory is available on this system:

    ```bash
    dmesg | grep -i memory
    ```

    - Show kernel messages with a timestamp(-T), human-readable(-H) and colorized(-L) output (available in kernels 3.5.0 and newer):

    ```bash
    dmesg -THL
    ```

### systemd-analyze

> Analyze and debug system manager.
> Show timing details about the boot process of units (services, mount points, devices, sockets).
> More information: <https://www.freedesktop.org/software/systemd/man/systemd-analyze.html>.
??? example

    - List all running units, ordered by the time they took to initialize:

    ```bash
    systemd-analyze blame
    ```

    - Print a tree of the time-critical chain of units:

    ```bash
    systemd-analyze critical-chain
    systemd-analyze critical-chain ssh.service
    ```

    - Create an SVG file showing when each system service started, highlighting the time that they spent on initialization:

    ```bash
    systemd-analyze plot > path/to/file.svg
    ```

    - Plot a dependency graph and convert it to an SVG file:

    ```bash
    systemd-analyze dot | dot -T svg > path/to/file.svg
    ```

    - Show security scores of running units:

    ```bash
    systemd-analyze security
    ```
