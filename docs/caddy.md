## Log files permissions for Promtail

```bash
chmod +X /var/log/caddy/
chmod 644 /var/log/caddy/*
setfacl -R -m u::rwX,g::rwX,o:rX /var/log/caddy/
setfacl -R -d -m u::rwX,g::rwX,o:rX /var/log/caddy/
setfacl -Rdm u:promtail:rX /var/log/caddy
setfacl -Rm u:promtail:rX /var/log/caddy
```
