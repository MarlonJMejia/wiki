
# Cache Facts

???+ info "ansible.cfg"

    ```sh
    [defaults]
    fact_caching = jsonfile 
    gathering = smart
    fact_caching_connection = ./cached_facts # Store facts in this directory
    fact_caching_timeout = 86400 # Use cache fact for a single day, before new ones are gathered.
    ssh_args = "-C -o ControlMaster=auto -o ControlPersist=1800s -o PreferredAuthentications=publickey"
    forks = 50  # Increase amount of host connections
    host_key_checking = False # Disable host_key_verification
    inventory = ./inventory.yml
    remote_user = root
    roles_path = ./roles
    ```