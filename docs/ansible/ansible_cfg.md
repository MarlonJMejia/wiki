
# Cache Facts

???+ info "ansible.cfg"

    ```sh
    [defaults]
    fact_caching = jsonfile
    gathering = smart
    fact_caching_connection = ./cached_facts
    fact_caching_timeout = 86400
    ssh_args = "-C -o ControlMaster=auto -o ControlPersist=1800s -o PreferredAuthentications=publickey"
    forks = 50
    host_key_checking = False
    inventory = ./inventory.yml
    remote_user = root
    roles_path = ./roles
    ```