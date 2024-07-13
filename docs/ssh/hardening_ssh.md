# Generating an SSH Key

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

# Authorizing The User
On the Server, add the contents of the public key file to `~/.ssh/authorized_keys`.

To  add a public_key to all users, you can predefined a directory, with a key for the users of your system.

```bash
AuthorizedKeysFile  .ssh/authorized_keys /etc/ssh/authorized_keys
```
____
# Server Configuration options

[https://man.openbsd.org/sshd_config.5](https://man.openbsd.org/sshd_config.5)<br>
Configuration file is located under `/etc/ssh/sshd_config`

Allow or Deny Entity Type by Precendence:

`DenyUsers > AllowUsers > DenyGroups > AllowGroups`

```bash title="Deny the user illegal from access"
DenyUsers illegal
AllowUsers illegal
```
Deny authentication methods:

```ini
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
