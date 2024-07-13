### Crowdsec OPNSense Plugin integration with Caddy

# General

Ban an IP Address for s,m,h,d of time.

    cscli decisions add --ip 192.168.1.10 --duration 1m

# Register a server and client

Server:

```bash
cscli bouncers add dmz
```

Client:

```bash
cscli lapi register -u http://10.0.0.1:8080
```

Server should have a new machine added to the list:

```bash
cscli machines list
```
??? example

    root@OPNsense:~ # sudo cscli machines list
    
    Name                                              IP Address  Last Update           Status  Version                  Auth Type  Last Heartbeat
    
    localhost                                         10.0.0.1    2024-07-13T12:00:05Z  ✔️      v1.6.2-16bfab86-freebsd  password   1m21s
    3923f4ccdd84438581a2397ba488a6a454dpH1qIzAz4gKBg  10.10.10.2  2024-07-13T12:00:05Z  🚫                               password   58s
    

```bash
cscli machines validate 3923f4ccdd84438581a2397ba488a6a454dpH1qIzAz4gKBg
```
??? example

    
    3923f4ccdd84438581a2397ba488a6a454dpH1qIzAz4gKBg  10.10.10.2  2024-07-13T12:32:12Z  ✔️      v1.6.2-debian-pragmatic-amd64-16bfab86-linux  password   26s    