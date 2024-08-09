# Troubleshooting

## Usage | Load

```
# System uptime, and load average in the past 1, 5, 15 minutes.
uptime 

# what it does
netstat -tlpn # package net-tools
ss -tunalp
ps auxf

# memory:
# vmstat
# r: runnable (running or waiting to run in queue)
# b: uninterruptible sleep (D in ps)
vmstat # summary
vmstat 1 5 -w # every 1 sec, print 5 . wide .(first line is summary since reboot)
vmstat -s # summary memory stats

free -m
grep -i oom /var/log/messages ( /var/log/syslog )
dmesg -T | grep -i link

# CPU:
top

# package sysstat:
mpstat -P ALL # cpu balance

lscpu

pidstat
pidstat 1
pidstat -p $pid

# disk:
vmstat -d
df -h
df -i
iostat -xz 1

# biggest files in / :
du -mxS / |sort -n|tail -10

# network:
# package sysstat
sar -n DEV 1 # network throughput
sar -n TCP,ETCP 1 # TCP stats (also: ss -s)

# distro:
cat /etc/debian_version
lsb_release -a # apt-get install lsb-release

# boot
dmesg |tail
last -a
```

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

### Logging

```
journalctl
journalctl -n 20 --no-pager -u nginx # last lines for a specific unit
journalctl --since yesterday --until "1 hour ago" # takes 2022-12-24, 08:00 ...
journalctl -k # kernel messages, dmesg
journalctl -p err # 0, 1, 2, 3 ...
```

```
dmesg | tail
tail /var/log/messages  ( /var/log/syslog /var/log/kern.log )
```


### systemd

```
systemctl # same as systemctl list-units
systemctl cat <service> # shows location and contents of config file for <service>
systemctl list-unit-files # lists if they are masked (won't start, use unmask option)
systemctl reload unit # reload options after changes, install

systemctl --failed 
systemd-analyze # startup time, append 'blame' for breakdown
```

### Filesystems and volumes

```
fdisk -l
df -lT # -l local, -T type
lsblk -f # filesystem
file -s /dev/hda1
blkid /dev/hda1

mount
findmnt
cat /etc/fstab

fsck.ext4 -p /dev/sda1  # check and fix, if dirty

xfs_repair -n /dev/sda  # scan
xfs_repair /dev/sda     # scan and fix
```

### Networking

```
ss -s
netstat -s
netstat -i
ip -s link
ifconfig
lsof -i
sar -n DEV

ip route
netstat -r

iptables -L
iptables -t nat -L # does not show with -L
```

curl options:	

```
curl -v
curl -I # header info
curl -L # follow location
curl -O # download original name
```

nic:

```
/etc/sysconfig/network-scripts/ifcfg-eth0
DEVICE="eth0"
BOOTPROTO="dhcp"
```

### Kernel

```
uname -a
sysctl -a
```

### strace

```
strace -p $pid        # running program
strace -c $program    # run & summary
strace -e trace=write # filter
```

Also info under `/proc/$pid/`

### SSL/TLS

```
openssl x509 -in /path/to/server/certificate -text
openssl s_client -connect example.com:443
	HEAD / HTTP/1.1
	Host: example.com

openssl s_client -connect example.com:443 -servername example.com -showcerts | openssl x509 -text -noout
```

### cgroups

ulimits for current Bash session set at `/etc/security/limits.conf`

`su - username -c 'ulimit -a'`

`cat /proc/cgroups`

### Docker

```
docker ps -a
docker stats --all

docker logs <container>
docker inspect  <container>
docker diff <container>     # files changed
docker top <container>

docker update --help # update memory/cpu settings running container:
docker update -m 10M -c 2 <container>

# override entrypoint or command:
docker run -it --entrypoint /bin/bash <image>
docker run -it <image> /bin/bash 
```

### Kubernetes

```
kubectl cluster-info
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get pods --show-labels -o wide
kubectl top node my-node
kubectl api-resources
kubectl explain pods

kubectl rollout history deployment/frontend
kubectl rollout undo deployment/frontend -to-revision=3
kubectl rollout restart deployment/frontend

kubectl logs mypod --since 2m
kubectl logs mypod --previous

# CrashLoopBackOff: can't pul image, image with bad CMD.
# Deploy image with sleep command

kubectl describe ingress myingress
kubectl port-forward svc/my-service 5000 

kubeval my-invalid.yaml
kubectl diff -f ./my-manifest.yaml

# https://kubernetes.io/docs/tasks/debug-application-cluster/debug-running-pod/

kubectl debug -it yourpod --image=busybox:1.28 --target=yourpod
kubectl debug myapp -it --image=ubuntu --share-processes --copy-to=myapp-debug
kubectl debug myapp -it --copy-to=myapp-debug -- sh
kubectl debug node/mynode -it --image=ubuntu
```

### DNS

`host example.com`

From `dnsutils` package:  

`dig +short example.com` , `dig @ns_ip example.com`

`nslookup example.com` : resolves and tells you what DNS server you are using.

Critical files:

```
/etc/nsswitch.conf # order of resolving
/etc/resolv.conf   # nameservers
/etc/hosts         # hard-coded hostname-ip maps
```

## Applications

### nginx

Test configuration: `nginx -t`
Test and dump config: `nginx -T`

### etcd

```
etcdctl get --prefix --keys-only /
etcdctl get "" --prefix=true --keys-only
etcdctl endpoint status --write-out=table  # json to get version
etcdctl member list --write-out=table 
etcdctl alarm list

curl http://127.0.0.1:2379/health

grep "[CE] |" etcd.log
grep "apply entries took too long" etcd.log

etcdctl compact $version
```

# Cheatsheet

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
