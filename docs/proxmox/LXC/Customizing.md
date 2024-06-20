---
title: Customizing a Template
description: 
published: true
date: 2024-06-10T01:35:16.695Z
tags: lxc, proxmox, containers, linux
editor: markdown
dateCreated: 2024-06-10T01:35:16.695Z
---

# Creating and Customizing a Template

1. Upgrade or Installl any applications on the template. Ref: *[Downloading a LXC Template](/home/LXC)*

    ```shell
    pct enter <lxcid>
    ```

    ```shell
    apt upgrade && apt install -y vim ncurses git vim jq curl
    ```

2. Shutdown the container and create a template from the updated container.

    ```shell
    vzdump <lxc-id> --mode stop --compress gzip --dumpdir /var/lib/vz/template/cache/
    ```

