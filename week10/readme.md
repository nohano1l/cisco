# 11/8
```
小技巧
IOL(3L)R1
vpc1,2
R1
先把指令打在記事本
hostname R1
interface ethernet 0/0
    ip addr 192.168.1.254 255.255.255.0
    no shut
interface ethernet 0/1
    ip addr 192.168.2.254 255.255.255.0
    no shut
再複製到R1
# end
# show ip int brief
# show running
copy running config到記事本再貼到R1
```
## ipv6 手動設定
```
# ipv6 unicast-routing
# int e0/0
# ipv6 enable
# ipv6 addr 2000:0:1:1::1/64
# no shut
# int e0/1
# ipv6 enable
# ipv6 addr 2000:0:1:2::1/64
# no shut
vpc1
# ip 2000:0:1:1::2/64 2000:0:1:1::1
# show ipv6
vpc2
# ip 2000:0:1:2::2/64 2000:0:1:2::1
# show ipv6
vpc1
# ping 2000:0:1:1::1
# ping 2000:0:1:2::1
# ping 2000:0:1:2::2
R1
# do show int e0/0
vpc1,2
# clear ipv6
# show ipv6
vpc1
# ip auto
# show ipv6
```
## ipv6 自動設定
```
IOL L2(改icon)sw1
vpc1,2
sw1
# hostname sw1
# ipv6 unicast-routing
# int e0/0
# no switchport
# ipv6 enable
# ipv6 addr 2000:0:1:3::1/64
# no shut
# ipv6 nd other-config-flag
# int e0/1
# no switchport
# ipv6 enable
# ipv6 addr 2000:0:1:4::1/64
# no shut
# ipv6 nd other-config-flag
vpc1,2
# ip auto
# show ipv6
可以互ping
```
## 
```
IOL(3L)R1,IOL L2(改icon)sw1
vpc1,2,3
R1
# hostname R1
# ipv6 unicast-routing
# ipv6 dhcp pool DP1
# address prefix 2000:0:1::/48
# dns-server 2000:0:1::2
# domain-name test.com
# exit
# int e0/0
# ipv6 enable
# ipv6 addr 2000:0:1::1/48
# ipv6 dhcp server DP1
# no shut
vpc1
# ip 2000:0:1::2/48 2000:0:1::1
# ping 2000:0:1::1
# show ipv6
vpc2
# ip auto
R1
# exit
# ipv6 dhcp pool DP2
# address pre 2000:0:2::/48
# dns-server 2000:0:2::2
# domain-name example.com
# int e0/1
# ipv6 enable
# ipv6 addr 2000:0:2::1/48
# ipv6 dhcp server DP2
# no shut
vpc3
# ip auto
# show ipv6
```
## ipv6靜態路由
```
IOL(3L)R1,2,3
R1
# hostname R1
# ipv6 unicast-routing
# int e0/0
# ipv6 enable
# ipv6 addr 2000:0:1:1::1/64
# no shut
R2
# hostname R2
# ipv6 unicast-routing
# int e0/0
# ipv6 enable
# ipv6 addr 2000:0:1:1::2/64
# no shut
# int e0/1
# ipv6 enable
# ipv6 addr 2000:0:1:2::2/64
# no shut
R3
# hostname R3
# ipv6 unicast-routing
# int e0/0
# ipv6 enable
# ipv6 addr 2000:0:1:2::3/64
# no shut
R2
# do ping 2000:0:1:1::1
# do ping 2000:0:1:2::3
R1
# do ping 2000:0:1:2::3
不行
# do show ipv6 route
# exit
# ipv6 route 2000:0:1:2::/64 2000:0:1:1::2
# do show ipv6 route
R3
# exit
# ipv6 route 2000:0:1:1::/64 2000:0:1:2::2
# do show ipv6 route
R1
# do ping 2000:0:1:2::3

R1
# exit
# ipv6 router ospf 1
# router-id 1.1.1.1
# int e0/0
# ipv6 ospf 1 area 0
R2
# exit
# ipv6 router ospf 1
# router-id 2.2.2.2
# int e0/0
# ipv6 ospf 1 area 0
# int e0/1
# ipv6 ospf 1 area 0
R6
# exit
# ipv6 router ospf 1
# router-id 3.3.3.3
# int e0/0
# ipv6 ospf 1 area 0
R1
# do show ipv6 route
R2
# do show ipv6 int brief
R3
# do show ipv6 route
R1
# do ping 2000:0:1:2::3
```