## Example

* Caddy

```bash
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
