# Configuration

* linux
    /etc/crowdsec/notifcations

* FreeBSD
    /usr/local/etc/crowdsec/notifcations

```yaml title="/etc/crowdsec/notifications/discord.yaml"
type: http
name: discord
log_level: info
format: |
  {
  "content": " {{range . -}} {{$alert := . -}} {{range .Decisions -}}  {{if $alert.Source.Cn -}} {{$alert.Source.Cn}}: [WhoIs {{.Value}}](https://www.whois.com/whois/{{.Value}}) \n Type: {{.Type}} \n Duration: {{.Duration}} \n Scenario: {{.Scenario}} on machine '{{$alert.MachineID}}'. [Shodan](https://www.shodan.io/host/{{.Value}}){{end}} {{if not $alert.Source.Cn -}} :pirate_flag: [whois {{.Value}}](https://www.whois.com/whois/{{.Value}})\n Type: {{.Type}} \n Duration: {{.Duration}} \n Scenario: {{.Scenario}} on machine '{{$alert.MachineID}}'. \n [View on Shodan](<https://www.shodan.io/host/{{.Value}}>){{end}} {{end -}} {{end -}}
  "
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

```bash title='Example'
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
