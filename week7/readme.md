# 10/18
## OSPF
```
IOL(3L)R1、R2、R3
R1
# hostname R1
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
# int lo1
# ip addr 1.1.1.1 255.255.255.0
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
# int lo1
# ip addr 2.2.2.2 255.255.255.0
# no shut
# do show ip int brief
R3
# hostname R3
# int e0/0
# ip addr 23.1.1.3 255.255.255.0
# no shut
# int lo1
# ip addr 3.3.3.3 255.255.255.0
# no shut
# sh ip int brief
R1
# router ospf 100
# network 12.1.1.0 0.0.0.255 area 0
# network 1.1.1.0 0.0.0.255 area 0
R2
# router ospf 100
# network 12.1.1.0 0.0.0.255 a 0
# network 23.1.1.0 0.0.0.255 a 0
# network 2.2.2.0 0.0.0.255 a 0
R3
# router ospf 100
# network 23.1.1.0 0.0.0.255 a 0
# network 3.3.3.0 0.0.0.255 a 0
R1
# do show ip route

vpc1,2
vpc1
# ip 192.168.1.1 255.255.255.0 192.168.1.254
vpc2
# ip 192.168.2.1 255.255.255.0 192.168.2.254
R1
# int e0/2
# ip addr 192.168.1.254 255.255.255.0
# no shut
# int e0/1
# ip addr 13.1.1.1 255.255.255.0
# no shut
# router ospf 100
# network 13.1.1.0 0.0.0.255 a 0
# network 192.168.1.0 0.0.0.255 a 0
R2
# int e0/2
# ip addr 192.168.2.254 255.255.255.0
# no shut
# router ospf 100
# network 192.168.2.0 0.0.0.255 a 0
R3
# int e0/1
# ip addr 13.1.1.3 255.255.255.0
# no shut
# router ospf 100
# network 13.1.1.0 0.0.0.255 a 0
R1
# exit
# show ip route
vpc1
# ping  192.168.2.1
# trace 192.168.2.1
R1
# show ip ospf neighbor     //鄰居表
# show ip ospf database     //拓譜表
vpc1
# ping  192.168.2.1 -t
R1
# int e0/0
# shut
ping 會timeout一下下
vpc1
# trace 192.168.2.1
改路徑
```
## area
```
IOL(3L)R1、R2、R3
R1
# hostname R1
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
# int lo1
# ip addr 1.1.1.1 255.255.255.0
# router ospf 100
# network 1.1.1.0 0.0.0.255 a 1
# network 12.1.1.0 0.0.0.255 a 1
R2
# hostname R2
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# int e0/1
# ip addr 23.1.1.2 255.255.255.0
# no shut
# int lo1
# ip addr 2.2.2.2 255.255.255.0
# router ospf 100
# network 12.1.1.0 0.0.0.255 a 1
# network 23.1.1.0 0.0.0.255 a 0
# network 2.2.2.2 0.0.0.255 a 0
R3
# hostname R3
# int e0/0
# ip addr 23.1.1.3 255.255.255.0
# no shut
# int lo1
# ip addr 3.3.3.3 255.255.255.0
# router ospf 100
# network 23.1.1.0 0.0.0.255 a 0
# network 3.3.3.0 0.0.0.255 a 0
R1
# do show ip ospf neighbor
# do show ip ospf database
R3
# do show ip ospf neighbor
# do show ip ospf database
```
## rip & ospf 路由器可以同時跑不同的路由協定，最後存放路由表由AD值決定
```
IOL(3L)R1(serial=1)、R2(serial=1)、R3
vpc1,2
vpc1
# ip 192.168.1.1 255.255.255.0 192.168.1.254
vpc2
# ip 192.168.2.1 255.255.255.0 192.168.2.254
R1
# hostname R1
# int e0/1
# ip addr 192.168.1.254 255.255.255.0
# no shut
# int s1/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
# int e0/0
# ip addr 13.1.1.1 255.255.255.0
# no shut
# exit
# router rip
# version 2
# no auto-summary
# network 192.168.1.0
# network 12.1.1.0
# network 13.1.1.0
R2
# hostname R2
# int e0/1
# ip addr 192.168.2.254 255.255.255.0
# no shut
# int s1/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# int e0/0
# ip addr 23.1.1.2 255.255.255.0
# no shut
# exit
# router rip
# version 2
# no auto-summary
# network 192.168.2.0
# network 12.1.1.0
# network 23.1.1.0
R3
# hostname R3
# int e0/0
# ip addr 13.1.1.3 255.255.255.0
# no shut
# int e0/1
# ip addr 23.1.1.3 255.255.255.0
# no shut
# exit
# router rip
# version 2
# no auto-summary
# network 13.1.1.0
# network 23.1.1.0
R1
# exit
# do show ip route
vpc1
# trace 192.168.2.1
R1
# exit
# router ospf 100
# router-id 1.1.1.1
# network 192.168.1.0 0.0.0.255 a 0
# network 12.1.1.0 0.0.0.255 a 0
# network 13.1.1.0 0.0.0.255 a 0
R3
# exit
# router ospf 100
# router-id 3.3.3.3
# network 13.1.1.0 0.0.0.255 a 0
# network 23.1.1.0 0.0.0.255 a 0
R2
# exit
# router ospf 100
# router-id 2.2.2.2
# network 192.168.2.0 0.0.0.255 a 0
# network 12.1.1.0 0.0.0.255 a 0
# network 23.1.1.0 0.0.0.255 a 0
R1
# do show ip route
vpc1
# trace 192.168.2.1
```