---
title: Benchmark
description: 
tags: benchmark linux/benchmark
date: 2024-08-13
time: 06:51
---

# I/O Performance

## Benchmarking Block Device

> [!info]-  DD
> `bs=` indicates the file size to write each time. 1024k is 1 Megabyte, meaning we are testing throughput with 1 megabyte.
> `count=` copies only this amount of of blocks, DD keeps on going forever by default.
> > (bs\*count)
> > dd if=/dev/zero of=file bs=100MiB count=8 status=progress
> > 838860800 bytes (839 MB, 800 MiB) copied, 16.8348 s, 49.8 MB/s

* Writing test

```bash
for i in {1..5}; do dd if=/dev/zero of="file${i}" bs=100MiB count=5 status=progress; done
```

>[!info]- Average write on Oracle OCI
>524288000 bytes (524 MB, 500 MiB) copied, 10 s, 54.3 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 9.64985 s, 54.3 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 12 s, 43.7 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 12.0004 s, 43.7 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 11 s, 47.9 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 10.9358 s, 47.9 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 11 s, 49.8 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 10.5266 s, 49.8 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 11 s, 45.8 MB/s
>524288000 bytes (524 MB, 500 MiB) copied, 11.4364 s, 45.8 MB
>```bash
>#Add all the MB/s and Divide
>awk '/MB\/s/  {sum += $10; count++} END {if (count > 0) print sum / count " MB/s"}'
>```
> * `48.3 MB/s` is the Average speed write

* Reading Test

```bash
for i in {1..5}; do echo "Testing Read Disk with file ${i}"; dd if="file${i}" of=/dev/null; done
```

| System   | Disk Write | Disk write 100MiB byte blocks | Disk Read  | Disk Rebalancing |
| -------- | ---------- | ----------------------------- | ---------- | ---------------- |
| OCI      | 48.3 MB/s  | 15s - 10GB                    | 53.14 MB/s | 5min - 100GB     |
| Hellcat5 |            |                               |            |                  |

---
## Network Speed

### Iperf3

> [!warning]
> User iperf3 v3.16 or higher for multiple threads support.

*  Creating an iperf server to verify network throughput

```bash
iperf3 --server 0.0.0.0/0
```

* Connecting to the iperf server via another machine (client)

```
iperf3 --connect publicip --port 5201
```

#### Options

* `-P` =  Number of streams. iperf3 will spawn off a separate thread for each test stream. Using multiple streams may result in higher throughput than a single stream.
* `--bidir` = Test in both directions (normal and reverse), with both the client and server sending and receiving data simultaneously
* `-i, --interval n` pause n seconds between periodic throughput reports; default is 1, use 0 to disable
* `-t, --time n` time in seconds to transmit for (default 10 secs)

```bash
iperf3 --client host -p 5201 -P 4 --bidir -t 20
```

### ntttcp 

> [!info]
> Created by microsoft mainly for windows, but there is a version for linux.