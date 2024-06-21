
???+ note
    Following files can also be viewed to check if a restart is needed.

    ```bash 
    cat /var/run/reboot-required /run/reboot-required
    cat /var/run/reboot-required.pkgs /run/reboot-required.pkgs
    ```

### needrestart

> Check which daemons need to be restarted after library upgrades.
> More information: <https://github.com/liske/needrestart>.

??? example
    - List outdated processes:

    ```bash
    needrestart
    ```

    - Interactively restart services:

    ```bash
    sudo needrestart
    ```

    - List outdated processes in [v]erbose or [q]uiet mode:

    ```bash
    needrestart -v|q
    ```

    - Check if the [k]ernel is outdated:

    ```bash
    needrestart -k
    ```

    - Check if the CPU microcode is outdated:

    ```bash
    needrestart -w
    ```

    - List outdated processes in [b]atch mode:

    ```bash
    needrestart -b
    ```

    - List outdated processed using a specific [c]onfiguration file:

    ```bash
    needrestart -c path/to/config
    ```

    - Display help:

    ```bash
    needrestart --help
    ```
