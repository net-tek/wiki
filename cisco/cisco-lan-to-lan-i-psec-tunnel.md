<!-- TITLE: Cisco Lan To Lan I Psec Tunnel -->



```text
!--- Create an ISAKMP policy for Phase 1 
!--- negotiations for the L2L tunnels.


crypto isakmp policy 10
 hash md5
 authentication pre-share


!--- Specify the pre-shared key and the remote peer address 
!--- to match for the L2L tunnel.


crypto isakmp key vpnuser address 10.0.0.2
!

!--- Create the Phase 2 policy for actual data encryption.


crypto ipsec transform-set myset esp-des esp-md5-hmac
!

!--- Create the actual crypto map. Specify 
!--- the peer IP address, transform 
!--- set, and an access control list (ACL) for the split tunneling.


crypto map mymap 10 ipsec-isakmp
 set peer 10.0.0.2
 set transform-set myset
 match address 100


!--- Apply the crypto map on the outside interface.

interface Serial2/0
 ip address 172.16.1.1 255.255.255.0
 crypto map mymap


!--- Create an ACL for the traffic to 
!--- be encrypted. In this example, 
!--- the traffic from 10.1.1.0/24 to 172.16.2.0/24 
!--- is encrypted. The traffic which does not match the access list 
!--- is unencrypted for the Internet.

access-list 100 permit ip 10.1.1.0 0.0.0.255 172.16.2.0 0.0.0.255
```
