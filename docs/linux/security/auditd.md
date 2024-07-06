# Installing auditd

Install audit

    sudo dnf install audit

Enable and start auditd 

    systemctl enable --now auditd

[Premade configuration with best practices](https://github.com/Neo23x0/auditd)

```bash title="Download"
curl -L https://raw.githubusercontent.com/Neo23x0/auditd/master/audit.rules \
-o /etc/audit/rules.d/audit_git.rules
```
# auditctl

> Utility to control the behavior, get status and manage rules of the Linux Auditing System.

> More information: <https://manned.org/auditctl>.

??? example

    - Display the [s]tatus of the audit system:

    ```bash
    sudo auditctl -s
    ```

    - [l]ist all currently loaded audit rules:

    ```bash
    sudo auditctl -l
    ```

    - [D]elete all audit rules:

    ```bash
    sudo auditctl -D
    ```

    - [e]nable/disable the audit system:

    ```bash
    sudo auditctl -e 1|0
    ```

    - Watch a file for changes:

    ```bash
    sudo auditctl -a always,exit -F arch=b64 -F path=/path/to/file -F perm=wa
    ```

    - Recursively watch a directory for changes:

    ```bash
    sudo auditctl -a always,exit -F arch=b64 -F dir=/path/to/directory/ -F perm=wa
    ```