

# Networking CLI Tools


## Linux

## Basic Tools
- [Networking CLI Tools](#networking-cli-tools)
  - [Linux](#linux)
  - [Basic Tools](#basic-tools)
    - [ping](#ping)
    - [traceroute (tracert on Windows)](#traceroute-tracert-on-windows)
    - [nslookup](#nslookup)
    - [dig](#dig)
    - [netstat](#netstat)
    - [ip](#ip)
    - [nmap](#nmap)
    - [tcpdump](#tcpdump)
    - [curl](#curl)
    - [ssh](#ssh)
    - [telnet](#telnet)
    - [mtr](#mtr)
    - [Exercises to get familiar with the command:](#exercises-to-get-familiar-with-the-command)

---

### ping
Sends ICMP ECHO_REQUEST packets to network hosts.  
It's commonly used to check if a host is reachable across an IP network.  

Example:

`````````bash
ping google.com
`````````
This command sends packets to google.com and prints the response times.  


### traceroute (tracert on Windows)
Shows the route packets take to reach a network host.  
It can identify the path and measure transit delays of packets across an IP network.  

Example:
`````````bash
traceroute google.com
`````````
This command traces the path from your machine to google.com.  


### nslookup
Queries the Domain Name System to obtain domain or IP address mapping.  
It's useful for finding out information about domain names and their corresponding IP addresses.  
Example:
`````````bash
nslookup example.com
`````````
This command retrieves DNS records for example.com.  


### dig

> DNS lookup utility.
> More information: <https://manned.org/dig>.

- Lookup the IP(s) associated with a hostname (A records):

```bash
dig +short example.com
```

- Get a detailed answer for a given domain (A records):

```bash
dig +noall +answer example.com
```

- Query a specific DNS record type associated with a given domain name:

```bash
dig +short example.com A|MX|TXT|CNAME|NS
```

- Specify an alternate DNS server to query and optionally use DNS over TLS (DoT):

```bash
dig +tls @1.1.1.1|8.8.8.8|9.9.9.9|... example.com
```

- Perform a reverse DNS lookup on an IP address (PTR record):

```bash
dig -x 8.8.8.8
```

- Find authoritative name servers for the zone and display SOA records:

```bash
dig +nssearch example.com
```

- Perform iterative queries and display the entire trace path to resolve a domain name:

```bash
dig +trace example.com
```

- Query a DNS server over a non-standard [p]ort using the TCP protocol:

```bash
dig +tcp -p port @dns_server_ip example.com
```



### netstat
Displays network connections, routing tables, interface statistics, masquerade connections, and multicast memberships.  
Example:
`````````bash
netstat -tuln
`````````
This command lists all TCP and UDP listening ports along with their addresses and port numbers.  

### ip

> Show/manipulate routing, devices, policy routing and tunnels.
> Some subcommands such as ```ip address``` have their own usage documentation.
> More information: <https://www.man7.org/linux/man-pages/man8/ip.8.html>.

- List interfaces with detailed info:

```bash
ip address
```

- List interfaces with brief network layer info:

```bash
ip -brief address
```


- List interfaces with brief link layer info:

```bash
ip -brief link
```


- Display the routing table:

```bash
ip route
```


- Show neighbors (ARP table):

```bash
ip neighbour
```


- Make an interface up/down:

```bash
ip link set interface up|down
```

- Add/Delete an IP address to an interface:

```bash
ip addr add/del ip/mask dev interface
```

- Add a default route:

```bash
ip route add default via ip dev interface
```



### nmap
Network exploration tool and security scanner.  
It's used to discover hosts and services on a computer network, thus building a "map" of the network.  
Example:
`````````bash
nmap -p 22,80,443 192.168.1.1
`````````
This command scans ports 22, 80, and 443 on the host 192.168.1.1.  


### tcpdump
A network traffic analyzer or sniffer.  
It captures packets off a network interface and interprets them for you.  
Example:
`````````bash
tcpdump -i eth0 port 80
`````````
This captures all traffic on the eth0 interface to or from port 80.  


### curl
Command-line utilities for downloading files from the web.  
curl is also capable of uploading data and supports a wide variety of protocols.  
Example with curl:
`````````bash
curl -o example.html http://example.com
`````````
This downloads the webpage from example.com and saves it as example.html.  


### ssh
Secure Shell is a protocol used to securely log onto remote systems.  
It's the command-line tool for accessing remote machines.  
Example:
`````````bash
ssh user@192.168.1.10
`````````
This command logs you into the remote machine with IP 192.168.1.10 as user.  


### telnet
A network protocol used on the Internet or local area networks to provide
 a bidirectional interactive text-oriented communication facility using a virtual terminal connection.  
Not secure compared to SSH but useful for troubleshooting.  
Example:
`````````bash
telnet example.com 80
`````````
This connects to example.com on port 80, often to test web server connectivity.  


### mtr
Combines the functionality of traceroute and ping into one diagnostic tool.  
It displays the route and measures each hop's transit delays.  
Example:
`````````bash
    mtr google.com
`````````
Shows the path and network latency to google.com with real-time updates.  
Each of these tools has multiple flags and options you can use to refine
 your commands according to your specific needs.  


### Exercises to get familiar with the command:
* Try using ping to check the connectivity to a local and remote server.  
* Use traceroute to see the path taken to an external website.  
* Run nslookup to find out the authoritative DNS servers for a domain.  
* Explore nmap by scanning your local network for open ports.  
* Capturing HTTP traffic with tcpdump.  

Remember that there are manual pages accessible with ```man <cmd>``` (e.g., ```man ping```).  
It is a good practice to check the manual page for detailed information about available flags and their uses.  

As for constructive criticism, remember that while learning these tools, it's important to use them responsibly.  
Many of these tools can be misused for probing networks without permission, which could be illegal and unethical.  
Always have proper authorization before scanning networks and hosts.  

Now, acting as a student, a question one might have: "How can you use nmap to scan for a specific service running on a range of IP addresses?"

To answer: You can scan for a specific service with nmap by specifying the service's port number and a CIDR notation to denote the IP range.  
For example, if you want to scan for SSH servers running on the IP range 192.168.1.0/24, you could use nmap -p 22 192.168.1.0/24.  
This command will only scan port 22 (the default port for SSH) on all IP addresses from 192.168.1.0 to 192.168.1.255.  
