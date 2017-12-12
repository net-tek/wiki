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
