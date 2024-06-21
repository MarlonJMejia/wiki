## Files
???+ example "Inspection"

    - Follow a file with grep and tail

    ```bash
    tail -f file | grep --line-buffered my_pattern
    ```

    - When was the system last patched

    ```bash
    grep $(date +%F) /var/log/apt/history.log
    ```