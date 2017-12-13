<!-- TITLE: Lsyncd Configuration -->
<!-- SUBTITLE: A quick summary of Lsyncd Configuration -->

## /etc/lsyncd.conf

```lua
----
-- User configuration file for lsyncd.
--
-- Simple example for default rsync, but executing moves through on the target.
--
-- For more examples, see /usr/share/doc/lsyncd*/examples/
-- 
-- sync{default.rsyncssh, source="/var/www/html", host="localhost", targetdir="/tmp/htmlcopy/"}

local confdir = '/etc/lsyncd.d/'
local entries = readdir( confdir )
for name, isdir in pairs( entries ) do
    if not isdir then
        dofile( confdir .. name )
    end
end
settings {
logfile = "/var/log/lsyncd/lsyncd.log",
statusFile = "/var/log/lsyncd/lsyncd-status.log",
statusInterval = 2
}
```

## /etc/lsyncd.d/*.lua

```lua
sync {
default.rsync,
source="/etc/nginx/conf.d",
target="root@remote_host_here:/etc/nginx/conf.d/",
delay = 0,
rsync = {
archive = true,
perms = true,
owner = true,
compress = false,
acls = true,
verbose = true,
rsh = "/usr/bin/ssh -p 22 -o StrictHostKeyChecking=no" }
}
```

