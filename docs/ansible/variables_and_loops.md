## Variables

All default variables can be found under in the [Ansible Documentation](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html)

* ansible_play_hosts

    List of hosts in the current play run, not limited by the serial. Failed/Unreachable hosts are excluded from this list.

* ansible_play_hosts_all

    List of all the hosts that were targeted by the play
    ['10.0.0.1', '10.0.0.2', '10.0.0.2']

## Access individual hosts facts

```bash title='hosts.j2'
{% for host in ansible_play_hosts_all %}
{{hostvars[host].ansible_default_ipv4.address}} {{hostvars[host].ansible_hostname}} {{hostvars[host].ansible_fqdn}}
{%endfor%}
```
