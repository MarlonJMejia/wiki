https://github.com/fatedier/frp?tab=readme-ov-file#access-your-computer-in-a-lan-network-via-ssh

## Example Usage

To begin, download the latest program for your operating system and architecture from the [Release](https://github.com/fatedier/frp/releases) page.

Next, place the `frps` binary and server configuration file on Server A, which has a public IP address.

Finally, place the `frpc` binary and client configuration file on Server B, which is located on a LAN that cannot be directly accessed from the public internet.

Some antiviruses improperly mark frpc as malware and delete it. This is due to frp being a networking tool capable of creating reverse proxies. Antiviruses sometimes flag reverse proxies due to their ability to bypass firewall port restrictions. If you are using antivirus, then you may need to whitelist/exclude frpc in your antivirus settings to avoid accidental quarantine/deletion. See [issue 3637](https://github.com/fatedier/frp/issues/3637) for more details.

### Access your computer in a LAN network via SSH

1. Modify `frps.toml` on server A by setting the `bindPort` for frp clients to connect to:

  ```toml
  # frps.toml
  bindPort = 7000
  ```

2. Start `frps` on server A:

  `./frps -c ./frps.toml`

3. Modify `frpc.toml` on server B and set the `serverAddr` field to the public IP address of your frps server:

  ```toml
  # frpc.toml
  serverAddr = "x.x.x.x"
  serverPort = 7000

  [[proxies]]
  name = "ssh"
  type = "tcp"
  localIP = "127.0.0.1"
  localPort = 22
  remotePort = 6000
  ```

Note that the `localPort` (listened on the client) and `remotePort` (exposed on the server) are used for traffic going in and out of the frp system, while the `serverPort` is used for communication between frps and frpc.

4. Start `frpc` on server B:

  `./frpc -c ./frpc.toml`

5. To access server B from another machine through server A via SSH (assuming the username is `test`), use the following command:

  `ssh -oPort=6000 test@x.x.x.x`

### Multiple SSH services sharing the same port

This example implements multiple SSH services exposed through the same port using a proxy of type tcpmux. Similarly, as long as the client supports the HTTP Connect proxy connection method, port reuse can be achieved in this way.

1. Deploy frps on a machine with a public IP and modify the frps.toml file. Here is a simplified configuration:

  ```toml
  bindPort = 7000
  tcpmuxHTTPConnectPort = 5002
  ```

2. Deploy frpc on the internal machine A with the following configuration:

  ```toml
  serverAddr = "x.x.x.x"
  serverPort = 7000

  [[proxies]]
  name = "ssh1"
  type = "tcpmux"
  multiplexer = "httpconnect"
  customDomains = ["machine-a.example.com"]
  localIP = "127.0.0.1"
  localPort = 22
  ```

3. Deploy another frpc on the internal machine B with the following configuration:

  ```toml
  serverAddr = "x.x.x.x"
  serverPort = 7000

  [[proxies]]
  name = "ssh2"
  type = "tcpmux"
  multiplexer = "httpconnect"
  customDomains = ["machine-b.example.com"]
  localIP = "127.0.0.1"
  localPort = 22
  ```

4. To access internal machine A using SSH ProxyCommand, assuming the username is "test":

  `ssh -o 'proxycommand socat - PROXY:x.x.x.x:%h:%p,proxyport=5002' test@machine-a.example.com`

5. To access internal machine B, the only difference is the domain name, assuming the username is "test":

  `ssh -o 'proxycommand socat - PROXY:x.x.x.x:%h:%p,proxyport=5002' test@machine-b.example.com`