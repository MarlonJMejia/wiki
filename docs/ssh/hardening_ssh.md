# Generating an SSH Key

??? note "Explanation"
    Public Key - Accessible to everyone, shared with everyone, used on the server(s).

    Private Key - Must be kept secret, not shared with everyone, used by the client.

    A Public Key is the Door Knob

    A Private Key is the Key for the Door Knob

Generate an SSH key with:
```bash
ssh-keygen -C mycomment -f /tmp/somekey -t ed25519
```

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
