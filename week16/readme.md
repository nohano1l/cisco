# 12/20
## ACL
```
IOL(L3)R1,R2,R3
R1
# hostname R1
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
# int lo 1
# ip addr 192.168.1.1 255.255.255.0
# no shut
# int lo 2
# ip addr 192.168.2.1 255.255.255.0
# no shut
# do show ip int brief
R2
# hostname R2
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# int e0/1
# ip addr 23.1.1.2 255.255.255.0
# no shut
# do show ip int brief
R3
# hostname R3
# int e0/0
# ip addr 23.1.1.3 255.255.255.0
# no shut
# int lo 1
# ip addr 3.3.3.3 255.255.255.255
# no shut
# int lo 2
# ip addr 8.8.8.8 255.255.255.255
# no shut
# do show ip int brief
R1
# router ospf 1
# router-id 1.1.1.1
# network 192.168.1.0 0.0.0.255 a 0
# network 192.168.2.0 0.0.0.255 a 0
# network 12.1.1.0 0.0.0.255 a 0
R2
# exit
# router ospf 1
# router-id 2.2.2.2
# network 12.1.1.0 0.0.0.255 a 0
# network 23.1.1.0 0.0.0.255 a 0
R3
# exit
# router ospf 1
# router-id 3.3.3.3
# network 23.1.1.0 0.0.0.255 a 0
# network 3.3.3.3 0.0.0.0 a 0
# network 8.8.8.8 0.0.0.0 a 0
R1,2
# do show ip route
R1
# do ping 3.3.3.3 source 192.168.1.1
# do ping 3.3.3.3 source 12.1.1.1
R2
# access-list 1 deny 192.168.1.0 0.0.0.255
# access-list 1 permit any
# do show access-list
# int e0/0
# ip access-group 1 in
R1
# do ping 3.3.3.3 source 192.168.1.1
失敗
# do ping 3.3.3.3 source 12.1.1.1
成功

R2
# username cisco password cisco
# ip domain-name test.com
# crypto key generate rsa
1024
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
R1
# ssh -l cisco 12.1.1.2
cisco
> exit
R3
# ssh -l cisco 23.1.1.2
cisco
> exit
R2
# access-list 2 permit 12.1.1.1 0.0.0.0
或
# access-list 2 permit host 12.1.1.1

# access-list 2 deny any
# do show access-list
# line vty 0 4
# access-class 2 in
R1
# ssh -l cisco 23.1.1.2
cisco
R3
# ssh -l cisco 23.1.1.2
X
R2
# show run
# int e0/0
# no ip access-group 1 in
# line vty 0 4
# no access-class 2 in
R1
# ssh -l cisco 12.1.1.2
cisco
> exit
R3
# ssh -l cisco 23.1.1.2
cisco
> exit
R1
# ping 3.3.3.3 source 192.168.1.1
# ping 8.8.8.8 source 192.168.1.1
R2
# exit
# access-list 101 permit icmp 192.168.1.0 0.0.0.255 host 3.3.3.3
# access-list 101 deny icmp 192.168.1.0 0.0.0.255 host 8.8.8.8
# do show access-list
# int e0/0
# ip access-group 101 in
R1
# ping 3.3.3.3 source 192.168.1.1
O
# ping 8.8.8.8 source 192.168.1.1
X

R2
# int e0/0
# no ip access-group 101 in
# int e0/1
# ip access-group 101 out
R1
# ping 3.3.3.3 source 192.168.1.1
O
# ping 8.8.8.8 source 192.168.1.1
X
//命名式用法
R2
# no ip access-group 101 out
# ip access-list extended rule1.0
# permit icmp 192.168.1.0 0.0.0.255 host 3.3.3.3
# deny icmp 192.168.1.0 0.0.0.255 host 8.8.8.8
# exit
# do show ip access-list
# int e0/0
# ip access-group rule1.0 in
R1
# ping 3.3.3.3 source 192.168.1.1
O
# ping 8.8.8.8 source 192.168.1.1
X
//插入子規則
R2
# ip access-list extended rule1.0
# 15 permit icmp 192.168.1.0 0.0.0.255 host 6.6.6.6
# do show ip access-list
```
## NAT
```
R2
# exit
# access-list 10 permit 192.168.1.0 0.0.0.255
# ip nat pool DNATPOOL 23.1.1.100 23.1.1.100 23.1.1.200 netmask 255.255.255.0
# int e0/0
# ip nat inside
# int e0/1
# ip nat outside
# exit
# ip nat inside source list 10 pool DNATPOOL
R1
# ping 3.3.3.3 source 192.168.1.1
```