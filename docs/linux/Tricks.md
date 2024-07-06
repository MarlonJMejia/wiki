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

???+ example "Backing up"

    - Backing up a root drive

    ```bash
    sudo rsync -aAXHv --stats --exclude={"/dev/*","/swapfile","/raid/*","/proc/*","/sys/*","/tmp/*","/run/*","/mnt/*","/media/*","/lost+found"} / /raid/backups/root-backup-$(date '+%m-%d-%Y')
    ```
