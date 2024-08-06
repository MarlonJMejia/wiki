# Notifications

## Configuation files

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
    "content": "```\n{{range . -}}{{$alert := . -}}{{range .Decisions -}}- {{.Value}} will get **{{.Type}}** for the next '{{.Duration}}' for triggering '{{.Scenario}}'\n{{end -}}{{end -}}\n```"
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
