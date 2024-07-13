# Hardening SSH with Authorized Keys

## Overview

I found multiple attempts to login to my server via SSH.  

All of these attempts failed due to wrong
passwords or non-existent usernames.

So even though no one succeeded in getting in to my server, I still want to make sure 
no one can get in unless I want them to.

To do this, I changed the SSH server (`sshd`) configuration to only allow those with 
authorized SSH keys to connect.

- "client" refers to the Local Machine
- "server" refers to the Remote Machine

## Generating an SSH Key

If you don't already have an SSH key, you'll need to generate one.  

On the client, generate an SSH key with:
```bash
ssh-keygen -C mycomment -f /tmp/somekey -t ed25519
```
* Optionally you can add a comment to the key with `-C "comment"`.  

Next, grab the public key (NOT THE PRIVATE KEY!) from `~/.ssh/id_ed25519.pub`.
Public keys will always end with `.pub`.
```bash
cat ~/.ssh/id_ed25519.pub
```

### Authorizing The User
On the Server, add the contents of the public key file to `~/.ssh/authorized_keys`.

To  add a public_key to all users, you can predefined a directory, with a key for the users of your system.

```bash
AuthorizedKeysFile  .ssh/authorized_keys /etc/ssh/authorized_keys
```

### Server Configuration options

https://man.openbsd.org/sshd_config.5
Configuration file is located under `/etc/ssh/sshd_config`

Allow or Deny Entity Type by Precendence:

`DenyUsers > AllowUsers > DenyGroups > AllowGroups`

```bash title="Deny the user illegal from access"
DenyUsers illegal
AllowUsers illegal
```
Deny authentication methods:

```config
PermitRootLogin [no, prohibit-password]
PubkeyAuthentication [yes, no]
PasswordAuthentication [yes, no]
AuthenticationMethods [pubkey, password, etc...]
```

# Verification

Verify users can only log in via public key authentication only

```bash
ssh -o PubkeyAuthentication=no -o PreferredAuthentications=password proxmox.inside.lan
```

Verify sshd configuration

```bash
sshd -t -f $file
```
