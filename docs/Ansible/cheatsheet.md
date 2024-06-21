### Gather and save facts from all hosts

???+ example

    ```bash
    ansible all -m ansible.builtin.gather_facts --tree /tmp/facts
    ```

### Ansible Galaxy

> Create and manage Ansible roles.
> More information: <https://docs.ansible.com/ansible/latest/cli/ansible-galaxy.html>.

???+ example

    - Create a directory structure for a new role
    
    ```bash
    ansible-galaxy init node_exporter
    ```

    - Install a role:

    ```bash
    ansible-galaxy install username.role_name
    ```

    - Remove a role:

    ```bash
    ansible-galaxy remove username.role_name
    ```

    - List installed roles:

    ```bash
    ansible-galaxy list
    ```

    - Search for a given role:

    ```bash
    ansible-galaxy search role_name
    ```

    - Get information about a user role:

    ```bash
    ansible-galaxy role info username.role_name
    ```

    - Get information about a collection:

    ```bash
    ansible-galaxy collection info username.collection_name
    ```
