# Access individual hosts facts

```bash
{% for host in ansible_play_hosts_all %}
{{hostvars[host].ansible_default_ipv4.address}} {{hostvars[host].ansible_hostname}} {{hostvars[host].ansible_fqdn}}
{%endfor%}
```
