---
title: Backup of Directories - Kopia
author: Marlon Mejia
contributors: Mejia
tested_with: 9.3
tags:
  - backup
  - rsnapshot
---

# Snapshots of Directories - _kopia_

## Prerequisites

- Know how to install additional repositories
- Know about mounting filesystems external to your machine (external drive, remote filesystem, and so on.)
- Minor Knowledge of scripting
- Know how to change the crontab for the root user
- Knowledge of client and server interaction as you may need to create a server repository for Kopia.

## Introduction

*Kopia* is an utility to create archival backups of your directories based on Repositories that can have an unlimited amount of Policies. Repositories allow you to encrypt and define files you may want to back up.

*Kopia* is very flexible as it can backup to multiple storage types making it easy to create an off-site backup solution to a cloud solution. [*List of Storage Locations*](https://kopia.io/docs/repositories/)

*Kopia* creates snapshots of the files and directories you designate, then encrypts these snapshots before they leave your computer, and finally uploads these encrypted snapshots to cloud/network/local storage called a repository. Snapshots are maintained as a set of historical point-in-time records based on policies that you define.

## Installing Kopia on Rocky Linux 9

Import the Public GPG Key from Kopias server

```bash
rpm --import https://kopia.io/signing-key
```

Create the Repository file for `dnf` or `yum`:

```bash
sudo tee /etc/yum.repos.d/kopia.repo <<EOF
[Kopia]
name=Kopia
baseurl=http://packages.kopia.io/rpm/stable/\$basearch/
gpgcheck=1
enabled=1
gpgkey=https://kopia.io/signing-key
EOF
```

You will now be able to install *Kopia*

```bash
dnf install kopia
```

???+ note "Kopia UI"

    Kopia has a UI Client that you can utilize, you are free to download it if you feel more comfortable with UI Clients, but we will not be using it on this guide.
    `install kopia-ui`


### Enabling auto-completion for Kopia

Using `kopia --completion-script-bash`

```bash title="Install bash-completions"
dnf install bash-completion
```

```bash
echo 'eval "$(kopia --completion-script-bash)"'\
| tee /etc/bash_completion.d/kopia-completion.sh
```

Source the file to add to your current session

```bash
source /etc/bash_completion.d/kopia-completion.sh
```

Autocompleted command options will be shown when you double ++tab+tab++ after typing Kopia

```bash
[root@rockytest rocky]# kopia
benchmark   diff        help        list        mount       policy      repository  restore     server      show        snapshot
```

## Creating a local repository

Create a folder to use with the repository

```bash
mkdir /.kopia_backups
```

Create the repository for your local filesystem

```bash
kopia repository create filesystem --path /.kopia_backups
```

When first creating a repository you will need to provide a password, you will receive the policy retention defined for your repository, these are defaults and derived from the global policy.

??? note

    Kopia cannot use multiple passwords, if you are currently using this guide for production make certain this password is stored on another entity other than yourself.


```bash
[root@rockytest .kopia_backups]# kopia repository create filesystem --path /.kopia_backups
Enter password to create new repository:
Re-enter password for verification:
Initializing repository with:
  block hash:          BLAKE2B-256-128
  encryption:          AES256-GCM-HMAC-SHA256
  splitter:            DYNAMIC-4M-BUZHASH
Connected to repository.

NOTICE: Kopia will check for updates on GitHub every 7 days, starting 24 hours after first use.
To disable this behavior, set environment variable KOPIA_CHECK_FOR_UPDATES=false
Alternatively you can remove the file "/root/.config/kopia/repository.config.update-info.json".

Retention:
  Annual snapshots:                 3   (defined for this target)
  Monthly snapshots:               24   (defined for this target)
  Weekly snapshots:                 4   (defined for this target)
  Daily snapshots:                  7   (defined for this target)
  Hourly snapshots:                48   (defined for this target)
  Latest snapshots:                10   (defined for this target)
  Ignore identical snapshots:   false   (defined for this target)
Compression disabled.

To find more information about default policy run 'kopia policy get'.
To change the policy use 'kopia policy set' command.

NOTE: Kopia will perform quick maintenance of the repository automatically every 1h0m0s
and full maintenance every 24h0m0s when running as root@rockytest.

See https://kopia.io/docs/advanced/maintenance/ for more information.

NOTE: To validate that your provider is compatible with Kopia, please run:

$ kopia repository validate-provider
```

## Your initinal snapshot and incremental snapshots

Creating a snapshot is as simple as a `kopia snapshot create`:

```bash title="Verify you are using the correct repository"
kopia repository status
```

```bash title="Create a snapshot for the current users home folder"
kopia snapshot create $HOME
```

Below is the output of the created snapshot:

???+ info "Information Layout"

    _**{user}**@**{hostname}**:**/{directory}**_<br>
    **{CREATIONDATE}** - **{{SNAPSHOT ID}}** - **{{SIZE}}** - **{{PERMISSIONS}}** - **{{RETENTION}}

```bash title="output"
[root@rockytest ~]# kopia snapshot ls
root@rockytest:/root
2024-07-15 18:57:22 EDT ke68996eddc0f04fd44419874c1624925 28.5 MB dr-xr-x--- files:78 dirs:106 (latest-1,hourly-1,daily-1,weekly-1,monthly-1,annual-1)
```

```bash title="Create an incremental backup of the backup."
mkdir -p $HOME/folder/{1..100}
kopia snapshot create $HOME
```

```bash title="output"
[root@rockytest ~]# kopia snapshot ls
root@rockytest:/root
2024-07-15 18:57:22 EDT ke68996eddc0f04fd44419874c1624925 28.5 MB dr-xr-x--- files:78 dirs:106 (latest-2,hourly-2)
2024-07-15 19:11:57 EDT k2e8cc246eeeef5d3f051e67fa513c7b0 28.7 MB dr-xr-x--- files:110 dirs:208 (latest-1,hourly-1,daily-1,weekly-1,monthly-1,annual-1)
```

Viewing the contents of the snapshot with `kopia list`:

```bash
kopia list  k2e8cc246eeeef5d3f051e67fa513c7b0 -r -l
```

```bash title=output
drwxr-xr-x            0 2024-07-15 19:11:57 EDT kf652ba8cf935331461506477ecd8bf3a  folder/2/
```

List a specific folder content and files by the object ID

```bash
kopia list kf652ba8cf935331461506477ecd8bf3a -l
```

```bash title="Json Format"
kopia content show  kf652ba8cf935331461506477ecd8bf3a -j
```

## Configuring your repository policy

Before diving into configuring your repository policies, you must first understand the policy settings and how they can be applied.

Policy settings can be applied to the following targets:

| Targets     | Example     |
| ------------- | ------------- |
| user@host | root@rockytest |
| @host | @rockytest |
| user@host:path | root@rockytest:/root |
| local path | /.kopia_backups |
| --global | global configuration |

```bash
[root@rockytest ~]# kopia policy show /.kopia_backups
Policy for root@rockytest:/.kopia_backups:

Retention:
  Annual snapshots:                     3   inherited from (global)
  Monthly snapshots:                   24   inherited from (global)
  Weekly snapshots:                     4   inherited from (global)
  Daily snapshots:                      7   inherited from (global)
  Hourly snapshots:                    48   inherited from (global)
  Latest snapshots:                    10   inherited from (global)
  Ignore identical snapshots:       false   inherited from (global)

Files policy:
  Ignore cache directories:          true   inherited from (global)
  No ignore rules:
  Read ignore rules from files:             inherited from (global)
    .kopiaignore
  Scan one filesystem only:         false   inherited from (global)

Error handling policy:
  Ignore file read errors:          false   inherited from (global)
  Ignore directory read errors:     false   inherited from (global)
  Ignore unknown types:              true   inherited from (global)

Scheduling policy:
  Scheduled snapshots:
    None.
  Manual snapshot:                  false   inherited from (global)

Uploads:
  Max parallel snapshots (server/UI):   1   inherited from (global)
  Max parallel file reads:              -   inherited from (global)
  Parallel upload above size:      2.1 GB   inherited from (global)

Compression disabled.

No actions defined.

OS-level snapshot support:
  Volume Shadow Copy:               never   inherited from (global)

Logging details (0-none, 10-maximum):
  Directory snapshotted:                5   inherited from (global)
  Directory ignored:                    5   inherited from (global)
  Entry snapshotted:                    0   inherited from (global)
  Entry ignored:                        5   inherited from (global)
  Entry cache hit:                      0   inherited from (global)
  Entry cache miss:                     0   inherited from (global)
```

Applying a compression setting to a repository via a policy

??? note

    To view a list of compression algorithms please see https://kopia.io/docs/advanced/compression/

```bash
[root@rockytest ~]# kopia policy set --compression zstd /.kopia_backups
Setting policy for root@rockytest:/.kopia_backups
 - setting compression algorithm to zstd
```

```bash title="output"
[root@rockytest ~]# kopia policy show /.kopia_backups | grep -i Compres
Compression:
  Compressor:                        zstd   (defined for this target)
  Compress files regardless of extensions.
  Compress files of all sizes.
```

```bash title="Remove specific file extensions from compression"
# Disable .7z .7Z and .gz .Gz .GZ file extensions
kopia policy set --add-never-compress=.tar.* --add-never-compress=.7[zZ] --add-never-compress=.[gG][zZ] /.kopia_backups
```

### Retention

Understanding backup rentetion policies

```bash
  --keep-latest=N            Number of most recent backups to keep per source (or 'inherit')                                                --keep-hourly=N            Number of most-recent hourly backups to keep per source (or 'inherit')                                         --keep-daily=N             Number of most-recent daily backups to keep per source (or 'inherit')                                          --keep-weekly=N            Number of most-recent weekly backups to keep per source (or 'inherit')
  --keep-monthly=N           Number of most-recent monthly backups to keep per source (or 'inherit')
  --keep-annual=N            Number of most-recent annual backups to keep per source (or 'inherit')
```
