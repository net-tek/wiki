<!-- TITLE: Cisco Lan To Lan I Psec Tunnel Interface Mode -->


```text
crypto isakmp policy 1
encr 3des
authentication pre-share
group 2
crypto isakmp key Cisco12345 address 0.0.0.0 0.0.0.0
crypto IPsec transform-set T1 esp-3des esp-sha-hmac
 mode transport (or tunnel?)
crypto IPsec profile P1
set transform-set T1

interface Tunnel0
 ip address 10.0.51.203 255.255.255.0
 ip ospf mtu-ignore
 load-interval 30
 tunnel source 10.0.149.203
 tunnel destination 10.0.149.217
 tunnel protection IPsec profile P1

interface Ethernet3/0
 ip address 10.0.149.203 255.255.255.0
 duplex full

interface Ethernet3/3
 ip address 10.0.35.203 255.255.255.0
 duplex full

ip classless
ip route 10.0.36.0 255.255.255.0 Tunnel0
```
