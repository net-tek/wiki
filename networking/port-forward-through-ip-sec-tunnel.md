<!-- TITLE: Port Forward Through Ip Sec Tunnel -->

Explanations :
We need to port forward a few services through a IPSEC tunnel to a server on another site. How to set this up, we already have a configured IPSEC tunnel up and running
Site A WAN IP: 1.1.1.1 INT IP: 172.16.0.0/16 want to port forward port 80 to 172.17.10.10 in site B
Site B WAN IP: 2.2.2.2 INT IP: 172.17.0.0/16 want to receive packets on port 80 through site A on 1.1.1.1
[Internet]->1.1.1.1:80 (WAN Site A) -> [IPSEC tunnel] -> 172.17.10.10:80 (LAN Site B)

Make traditional port forward (DNAT) : 1.1.1.1:80 --> 172.17.10.10:80 Make SNAT rule : 172.17.10.10:80 --> Change source to 172.16.1.1 (Translate source)