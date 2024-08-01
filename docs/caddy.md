## Log files permissions for Promtail

```bash
chmod +X /var/log/caddy/
chmod 644 /var/log/caddy/*
setfacl -R -m u::rw,g::rw,o:rX /var/log/caddy/
setfacl -R -d -m u::rw,g::rw,o:rX /var/log/caddy/
```