# General Commands

## vmstat

> Report information about processes, memory, paging, block IO, traps, disks and CPU activity.
> More information: <https://manned.org/vmstat>.

???+ example
    - Display virtual memory statistics:

    ```bash
    vmstat
    ```

    - Display reports every 2 seconds for 5 times:

    ```bash
    vmstat 2 5
    ```

## mpstat

> Report CPU statistics.
> More information: <https://manned.org/mpstat>.

???+ example
    - Display CPU statistics every 2 seconds:

    ```
    mpstat 2
    ```

    - Display 5 reports, one by one, at 2 second intervals:

    ```
    mpstat 2 5
    ```

    - Display 5 reports, one by one, from a given processor, at 2 second intervals:

    ```
    mpstat -P 0 2 5
    ```

## pidstat

> Show system resource usage, including CPU, memory, IO etc.
> More information: <https://manned.org/pidstat>.

???+ example
    - Show CPU statistics at a 2 second interval for 10 times:

    ```bash
    pidstat 2 10
    ```

    - Show page faults and memory utilization:

    ```bash
    pidstat -r
    ```

    - Show input/output usage per process ID:

    ```bash
    pidstat -d
    ```

    - Show information on a specific PID:

    ```bash
    pidstat -p PID
    ```

    - Show memory statistics for all processes whose command name include "fox" or "bird":

    ```bash
    pidstat -C "fox|bird" -r -p ALL
    ```

## iostat

> Report statistics for devices and partitions.
> More information: <https://manned.org/iostat>.

???+ example
    - Display a report of CPU and disk statistics since system startup:

    ```bash
    iostat
    ```

    - Display a report of CPU and disk statistics with units converted to megabytes:

    ```bash
    iostat -m
    ```

    - Display CPU statistics:

    ```bash
    iostat -c
    ```

    - Display disk statistics with disk names (including LVM):

    ```bash
    iostat -N
    ```

    - Display extended disk statistics with disk names for device "sda":

    ```bash
    iostat -xN sda
    ```

    - Display incremental reports of CPU and disk statistics every 2 seconds:

    ```bash
    iostat 2
    ```
