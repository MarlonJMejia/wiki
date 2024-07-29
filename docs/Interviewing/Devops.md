# Linux & Networking Screen Interview

Many interview processes for DevOps/Infrastructure/Cloud/SRE engineers have an initial screen interview (often carried out by a recruiter) consisting of short networking and Linux questions. These questions are supposed to be basic, answers are short so they can be easily checked and candidates need to answer correctly most of them to pass to the next phase.

Here are some of the most popular networking and Linux questions we’ve seen in these screen interviews. Note that the answers here are succinct and you want to study further if you are not familiar with a topic. This list is of course not exhaustive but it will give you an idea of what to expect in most initial interviews.

## Networking

- **What’s the three way handshake in TCP?**
  - Client sends SYN, server responds with SYN + ACK, client replies with ACK. SYN synchronizes the Sequence Number. ACK acknowledges receipt of all prior bytes.
  
- **What’s the difference between UDP and TCP? Give examples.**
  - TCP is connection-based, guarantees data delivery, used in HTTP, SSH, SMTP. UDP is faster, supports broadcasting, used in DNS, TFTP, VoIP.
  
- **Name some TCP flags**
  - SYN (Synchronization), ACK (Acknowledgement Number), Window, FIN, RST.

- **What’s the DNS record that maps a hostname to an IP address?**
  - “A” record. Others: CNAME, MX, PTR, TXT.

- **What’s the size of an IPv6 address?, what’s the name of its A record, how is “localhost” represented?**
  - IPv6 is 16 bytes. “A” record in IPv6 is “AAAA”. “localhost” in IPv6 is represented by: ::1.

- **How does traceroute work?**
  - Traceroute is an ICMP tool using the TTL field to trace the path of an IP packet.

- **What’s the port number for (common protocols) DNS, SSH, HTTP(S)?. How many TCP/UDP ports are there?**
  - DNS (53), HTTP (80), HTTPS (443). There are 65,535 ports.

- **How can we check in Linux what ports are open?**
  - Use ss or netstat, also lsof and nmap.

- **What’s the HTTP response code for “success”?**
  - 200

## Linux File System

- **What’s an inode?**
  - A data structure describing a file or directory. Stores metadata and disk block locations.

- **File permissions**
  - Read (4), write (2), execute (1). Use chmod to change them. Special permissions: SUID, SGID, sticky bit. SELinux context (.)

- **How do you find the disk usage?**
  - Use df or du.

- **How do you find the type of file systems present?**
  - Use df -T, lsblk -f, or mount.

## Linux Processes

- **Process management**
  - Process states, life cycle, zombie processes.
  
- **Uptime -> load**

- **Signals**
  - Signals you cannot ignore, signals to reload config.

- **Yes | apt-get install blah, how does it stop**

- **Strace**

## Other Linux Questions

- **Explain what happens: ls *.txt**
- **Find version of Linux kernel uname**
- **How to load kernel module**
- **User limits ulimit**
- **Kernel params sysctl**

For more details, visit [SadServers](https://docs.sadservers.com/docs/interviews/linux-networking-filter-interview/).
