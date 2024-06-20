---
title: Downloading a Template
description: 
published: true
date: 2024-06-10T01:38:37.441Z
tags: 
editor: markdown
dateCreated: 2024-06-10T01:38:37.441Z
---

# Downloading a Template

1. Update list of available template and their version.

    ```bash
    pveam update
    ```

2. List available templates.

    ```bash
    pveam available
    ```

3. Download container template.

    ```bash
    pveam download <images storage> <container template name>
    ```

    ```bash
    pveam download local-zfs_images debian-12-standard_12.0-1_amd64.tar.zst
    ```

4. Get list of downloaded templates.

    ```bash
    pveam list <images storage>
    ```

    ```bash
    pveam list local-zfs_images
    ```