<!-- TITLE: Squid 3 Config Example -->


```text
#
# Recommended minimum configuration:
#

# Example rule allowing access from your local networks.
# Adapt to list your (internal) IP networks from where browsing
# should be allowed

#logformat squid      %ts.%03tu %6tr %>a %Ss/%03>Hs %<st %rm %ru %[un %Sh/%<a %mt
logformat squid      %{%Y/%m/%d.%H:%M:%S}tl %6tr %>a %Ss/%03Hs %<st %rm %ru %un %Sh/%<A %mt

acl auth proxy_auth REQUIRED

acl anim-mtl src 10.140.0.0/20
acl vfx-mtl src 10.130.0.0/20
acl dev-mtl src 10.135.1.0/24
acl aws src 10.195.0.0/16
acl nimble src 10.129.6.60
acl nimble src 10.129.6.61
acl nimble src 10.129.6.62
acl vc01 src 10.129.0.23

acl whitelist dstdomain "/etc/squid/sites.whitelist.txt"
acl whitelist_aws dstdomain "/etc/squid/sites.whitelist.aws.txt"
acl whitelist_nimble dstdomain "/etc/squid/sites.whitelist.nimble.txt"
acl whitelist_vc01 dstdomain "/etc/squid/sites.whitelist.vc01.txt"
acl whitelist_pypi dstdomain "/etc/squid/sites.whitelist.pypi.txt"

acl SSL_ports port 443 3443 4443
acl nimble_ports port 443 80 2222
acl Safe_ports port 80		# http
acl Safe_ports port 443		# https
acl Safe_ports port 3443	# https
acl Safe_ports port 4443	# https
acl Safe_ports port 96		# https
acl CONNECT method CONNECT

#
# Recommended minimum Access Permission configuration:
#
# Deny requests to certain unsafe ports
http_access deny !Safe_ports

# Deny CONNECT to other than secure SSL ports
http_access deny CONNECT !SSL_ports

# Only allow cachemgr access from localhost
http_access allow localhost manager
http_access deny manager

# We strongly recommend the following be uncommented to protect innocent
# web applications running on the proxy server who think the only
# one who can access services on "localhost" is a local user
#http_access deny to_localhost

#
# INSERT YOUR OWN RULE(S) HERE TO ALLOW ACCESS FROM YOUR CLIENTS
#

# Example rule allowing access from your local networks.
# Adapt localnet in the ACL section to list your (internal) IP networks
# from where browsing should be allowed
http_access allow Safe_ports vfx-mtl whitelist
http_access allow Safe_ports anim-mtl whitelist
http_access allow Safe_ports nimble whitelist_nimble
http_access allow Safe_ports vc01 whitelist_vc01
http_access allow Safe_ports dev-mtl whitelist_pypi
http_access allow SSL_ports aws whitelist_aws
http_access allow localhost

# And finally deny all other access to this proxy
http_access deny all

# Squid normally listens to port 3128
http_port 3128
#https_port 3129 ssl-bump cert=/etc/squid/cert.crt key=/etc/squid/private.key

# Uncomment and adjust the following to add a disk cache directory.
#cache_dir ufs /var/spool/squid 100 16 256

# Leave coredumps in the first cache dir
coredump_dir /var/spool/squid

#
# Add any of your own refresh_pattern entries above these.
#
refresh_pattern ^ftp:		1440	20%	10080
refresh_pattern ^gopher:	1440	0%	1440
refresh_pattern -i (/cgi-bin/|\?) 0	0%	0
refresh_pattern .		0	20%	4320
```
