<!-- TITLE: Keep Your Linux Ssh Session From Disconnecting -->
# Global Configuration
Add the following line to the `/etc/ssh/ssh_config` file:

```sh
ServerAliveInterval 30
```

The number is the amount of seconds before the server with send the no-op code.

# Current User Configuration
Add the following lines to the `~/.ssh/config` file (create if it doesn’t exist)


```sh
Host *
  ServerAliveInterval 30
```

Make sure you indent the second line with a space.

# Per-Host Configuration
If you only want to enable keep alive for a single server, you can add that into the `~/.ssh/config` file with the following syntax:


```sh
Host *hostname.com
   ServerAliveInterval 60
```

Works quite well, hope it helps somebody else out there.

