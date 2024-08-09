# Acquisitions

By default when CrowdSec is installed it will attempt to detect the running services and acquire the appropriate log sources and Collections.

```bash
cscli metrics show acquisition
```

* Caddy

```yaml
---
filenames:
 - /var/log/caddy/*.log
labels:
  type: caddy
```

## Verifying

* Verify crowdsec configuration

```bash
crowdsec -t
```

* Log file has been found

```bash
grep '/path/to/your/file.log' /var/log/crowdsec.log
```

* 

```bash
tail -n 10 /var/log/caddy/access.log | cscli explain -f- --type caddy -v
```

# Notifications

* linux
    /etc/crowdsec/notifcations

* FreeBSD
    /usr/local/etc/crowdsec/notifcations

```yaml title="/etc/crowdsec/notifications/discord.yaml"
type: http
name: discord
log_level: info
format: >-
  {
  "content": ">>>{{range . -}} {{$alert := . -}} {{range .Decisions -}}  {{if $alert.Source.Cn -}} {{$alert.Source.Cn}}: [whois {{.Value}}](https://www.whois.com/whois/{{.Value}}) \n Type: {{.Type}} \n Duration: {{.Duration}} \n Scenario: {{.Scenario}} on machine '{{$alert.MachineID}}'. [Shodan](https://www.shodan.io/host/{{.Value}}){{end}} {{if not $alert.Source.Cn -}} :pirate_flag: [whois {{.Value}}](https://www.whois.com/whois/{{.Value}})\n Type: {{.Type}} \n Duration: {{.Duration}} \n Scenario: {{.Scenario}} on machine '{{$alert.MachineID}}'. \n [View on Shodan](<https://www.shodan.io/host/{{.Value}}>){{end}} {{end -}} {{end -}}"
  }
url: https://discord.com/api/webhooks/<webhook id>/<webhook token>
#                                     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
#                                          Your ID+Token Here
method: POST
headers:
  Content-Type: application/json
```

`profiles.yaml` also needs to be edited to include the new alert.
Remember the filename has to match the notification name on the file.

```yaml title='Example'
name: default_ip_remediation
filters:
  - Alert.Remediation == true && Alert.GetScope() == "Ip"
decisions:
  - type: ban
    duration: 4h
notifications:
  # trigger discord.yaml notification
  - discord
on_success: break
```

# OPNSense

## Crowdsec OPNSense Plugin integration with Caddy

## General

Ban an IP Address for s,m,h,d of time.

    cscli decisions add --ip 192.168.1.10 --duration 1m

## Register a server and client

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