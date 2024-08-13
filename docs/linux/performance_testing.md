---
title: Benchmark
description: 
tags: benchmark linux/benchmark benchmark/ipef3 benchmark/fio
date: 2024-08-13
time: 06:51
---

## I/O Performance

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

`-P` =  Number of streams. iperf3 will spawn off a separate thread for each test stream. Using multiple streams may result in higher throughput than a single stream.
`--bidir` = Test in both directions (normal and reverse), with both the client and server sending and receiving data simultaneously
`-i, --interval n` pause n seconds between periodic throughput reports; default is 1, use 0 to disable
`-t, --time n` time in seconds to transmit for (default 10 secs)

```bash
iperf3 --client host -p 5201 -P 4 --bidir -t 10
```

Average Mbps for an Oracle Instance

| ID  | Role | Interval    | Transfer  | Bitrate     | Retr | Type    |
|-----|------|-------------|-----------|-------------|------|---------|
| 5   | TX-C | 0.00-10.00 sec | 16.5 MBytes | 13.8 Mbits/sec | 467  | sender  |
| 5   | TX-C | 0.00-10.01 sec | 13.5 MBytes | 11.3 Mbits/sec |      | receiver|
| 7   | TX-C | 0.00-10.00 sec | 18.2 MBytes | 15.3 Mbits/sec | 521  | sender  |
| 7   | TX-C | 0.00-10.01 sec | 15.2 MBytes | 12.8 Mbits/sec |      | receiver|
| 9   | TX-C | 0.00-10.00 sec | 17.5 MBytes | 14.7 Mbits/sec | 443  | sender  |
| 9   | TX-C | 0.00-10.01 sec | 14.1 MBytes | 11.8 Mbits/sec |      | receiver|
| 11  | TX-C | 0.00-10.00 sec | 18.6 MBytes | 15.6 Mbits/sec | 621  | sender  |
| 11  | TX-C | 0.00-10.01 sec | 16.0 MBytes | 13.4 Mbits/sec |      | receiver|
| SUM | TX-C | 0.00-10.00 sec | 70.9 MBytes | 59.5 Mbits/sec | 2052 | sender  |
| SUM | TX-C | 0.00-10.01 sec | 58.9 MBytes | 49.3 Mbits/sec |      | receiver|
| 13  | RX-C | 0.00-10.00 sec | 18.6 MBytes | 15.6 Mbits/sec | 1277 | sender  |
| 13  | RX-C | 0.00-10.01 sec | 16.6 MBytes | 13.9 Mbits/sec |      | receiver|
| 15  | RX-C | 0.00-10.00 sec | 18.2 MBytes | 15.3 Mbits/sec | 1131 | sender  |
| 15  | RX-C | 0.00-10.01 sec | 15.5 MBytes | 13.0 Mbits/sec |      | receiver|
| 17  | RX-C | 0.00-10.00 sec | 16.4 MBytes | 13.7 Mbits/sec | 904  | sender  |
| 17  | RX-C | 0.00-10.01 sec | 14.8 MBytes | 12.4 Mbits/sec |      | receiver|
| 19  | RX-C | 0.00-10.00 sec | 15.0 MBytes | 12.6 Mbits/sec | 602  | sender  |
| 19  | RX-C | 0.00-10.01 sec | 13.0 MBytes | 10.9 Mbits/sec |      | receiver|
| SUM | RX-C | 0.00-10.00 sec | 68.2 MBytes | 57.2 Mbits/sec | 3914 | sender  |
| SUM | RX-C | 0.00-10.01 sec | 59.9 MBytes | 50.2 Mbits/sec |      | receiver|


`Retr` = packets retransmitted
`ID/SUM` = SUM of speed if you are using `-P`

### ntttcp

> [!info]
> Created by microsoft mainly for windows, but there is a version for linux.
