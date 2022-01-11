1.




route-views>show ip route 1.0.132.0
Routing entry for 1.0.132.0/24
  Known via "bgp 6447", distance 20, metric 0
  Tag 6939, type external
  Last update from 64.71.137.241 6w1d ago
  Routing Descriptor Blocks:
  * 64.71.137.241, from 64.71.137.241, 6w1d ago
      Route metric is 0, traffic share count is 1
      AS Hops 3
      Route tag 6939
      MPLS label: none




route-views>show bgp 1.0.132.0
BGP routing table entry for 1.0.132.0/24, version 1386911439
Paths: (21 available, best #21, table default)
  Not advertised to any peer
  Refresh Epoch 1
  4901 6079 6939 38040 23969
    162.250.137.254 from 162.250.137.254 (162.250.137.254)
      Origin IGP, localpref 100, valid, external
      Community: 65000:10100 65000:10300 65000:10400
      path 7FE1355F6E90 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  57866 6461 7473 38040 23969
    37.139.139.17 from 37.139.139.17 (37.139.139.17)
      Origin IGP, metric 0, localpref 100, valid, external
      Community: 57866:304 57866:501
      path 7FE0C4BADF70 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  53767 6939 38040 23969
    162.251.163.2 from 162.251.163.2 (162.251.162.3)
      Origin IGP, localpref 100, valid, external
      Community: 53767:2000
      path 7FE101C5AAF0 RPKI State not found
      rx pathid: 0, tx pathid: 0
  Refresh Epoch 1
  3333 38040 23969
    193.0.0.56 from 193.0.0.56 (193.0.0.56)
      Origin incomplete, localpref 100, valid, external
      path 7FE13ABCAE48 RPKI State not found




2.



root@vagrant:~# ip link add name dummy0 type dummy
root@vagrant:~# ip addr add 192.168.0.2/24 dev dummy0
root@vagrant:~# ip link set dummy0 up
root@vagrant:~# ip addr add 192.168.1.0/24 dev dummy0
root@vagrant:~# ip addr add 192.168.2.0/24 dev dummy0
root@vagrant:~# ip ro sh
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15
10.0.2.2 dev eth0 proto dhcp scope link src 10.0.2.15 metric 100
192.168.0.0/24 dev dummy0 proto kernel scope link src 192.168.0.2
192.168.1.0/24 dev dummy0 proto kernel scope link src 192.168.1.0
192.168.2.0/24 dev dummy0 proto kernel scope link src 192.168.2.0




3.




root@vagrant:~# ss -tlpn
State    Recv-Q   Send-Q       Local Address:Port       Peer Address:Port   Process
LISTEN   0        4096               0.0.0.0:111             0.0.0.0:*       users:(("rpcbind",pid=613,fd=4),("systemd",pid=1,fd=35))
LISTEN   0        4096         127.0.0.53%lo:53              0.0.0.0:*       users:(("systemd-resolve",pid=614,fd=13))
LISTEN   0        128                0.0.0.0:22              0.0.0.0:*       users:(("sshd",pid=1037,fd=3))
LISTEN   0        4096                  [::]:111                [::]:*       users:(("rpcbind",pid=613,fd=6),("systemd",pid=1,fd=37))
LISTEN   0        128                   [::]:22                 [::]:*       users:(("sshd",pid=1037,fd=4))




4.



root@vagrant:~# ss -ulpn
State    Recv-Q   Send-Q       Local Address:Port       Peer Address:Port   Process
UNCONN   0        0            127.0.0.53%lo:53              0.0.0.0:*       users:(("systemd-resolve",pid=614,fd=12))
UNCONN   0        0           10.0.2.15%eth0:68              0.0.0.0:*       users:(("systemd-network",pid=415,fd=19))
UNCONN   0        0                  0.0.0.0:111             0.0.0.0:*       users:(("rpcbind",pid=613,fd=5),("systemd",pid=1,fd=36))
UNCONN   0        0                     [::]:111                [::]:*       users:(("rpcbind",pid=613,fd=7),("systemd",pid=1,fd=38))





5.

