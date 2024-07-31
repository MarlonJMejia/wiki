**Reverse SSH tunneling** is a powerful tool that can be used to securely forward ports to or from remote servers to your local machine. It is especially useful when you don’t have direct access to a remote server, such as in a cloud environment. By creating a secure tunnel between the remote server and your local machine, you can access services on the remote server as if they were running on your own machine.

There are two ways you can use SSH tunneling, in this post I will describe both and how they can be useful. For this post, I assume you have access to a remote host running an SSH server with no firewall, as well as an SSH client on your local machine. I also assume there are no conflicts with ports already in use on the system, and you have the proper privileges to open ports on the system.


# Local port forwarding

Personally, I use local port forwarding the most to access ports on a headless server, for example to access a database on my local machine.

```bash
ssh -L 3306:localhost:3306 user@server
```

After this command, you can now access `port 3306` as if MySQL was running on your local machine and use any tools like you normally would!
Remote port forwarding #

Remote port forwarding is useful for bypassing firewalls or forwarding traffic from a remote server to your local machine to for example show a work in progress web project.

```bash
ssh -R 0.0.0.0:8080:localhost:80 user@server
```

This command will forward traffic from any IP (listening address 0.0.0.0) at port 8080 to your local machine on port 80. So someone could access server:8080 and see what is on your laptop on port 80.

# Using a Systemd Service

You can use a systemd service to auto start a remote port forwarding session, this can be useful to forward port 22 of your laptop to a server you own so you always have ssh access remotely.

```bash
[Unit]
Description=Port forwarding to server
After=network.target

[Service]
ExecStart=/usr/bin/ssh -NT -o ServerAliveInterval=60 -o ExitOnForwardFailure=yes -R 2222:localhost:22 user@server
RestartSec=5
Restart=always

[Install]
WantedBy=multi-user.target
```

Because we didn’t specify a listening address, it defaults to localhost access only. If you now login to the server over ssh and then ssh to localhost:2222 you should be able to access your laptop, neat!

# Bonus: Using SSH as SOCKS proxy

You can also use SSH as a proxy. This can be useful to access multiple control panel websites hosted on the server that normally are only available on localhost.

For this, execute the following command:

```bash
ssh -C -D 1080 user@server
```

Now you can setup a SOCKS proxy in your browser to localhost on port 1080 to forward traffic. Now you should be able to access the different sites hosted locally or bypass IP restrictions.
