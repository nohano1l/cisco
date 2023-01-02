# 10/11
## 
```
IOL(3L)R1、R2(serial=1)、R3(serial=1)
R2-s1/0-R3
R2
# hostname R2
# do show int e0/0
# do show int s1/0
# int e0/0
# ip addr 56.1.1.2 255.255.255.0
# no shut
# int s1/0
# ip addr 67.1.1.2 255.255.255.0
# no shut
# int lo 1
# ip addr 2.2.2.2 255.255.255.0
# do show ip int brief
# router rip
# version 2
# no auto-summary
# network 56.1.1.0
# network 67.1.1.0
# network 2.2.2.0
R1
# hostname R1
# int e0/0
# ip addr 56.1.1.1 255.255.255.0
# no shut
# int s1/0
# ip addr 57.1.1.1 255.255.255.0
# no shut
# int lo 1
# ip addr 1.1.1.1 255.255.255.0
# do show ip int brief
# router rip
# version 2
# no auto-summary
# network 56.1.1.0
# network 57.1.1.0
# network 1.1.1.0
R3
# hostname R3
# int e0/0
# ip addr 57.1.1.3 255.255.255.0
# no shut
# int s1/0
# ip addr 67.1.1.3 255.255.255.0
# no shut
# int lo 1
# ip addr 3.3.3.3 255.255.255.0
# do show ip int brief
# router rip
# version 2
# no auto-summary
# network 57.1.1.0
# network 67.1.1.0
# network 3.3.3.0
R2
# do show ip route
# exit
# ip route 3.3.3.0 255.255.255.0 56.1.1.2
# do show ip route
```
## ospf
```
IOL(3L)R1、R2、R3
R1
# hostname R1
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
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
R1
# router ospf 100
# network 12.1.1.0 0.0.0.255 area 0     //0.0.0.255反掩碼wildcard mask
R2
# router ospf 200
# network 12.1.1.0 0.0.0.255 a 0
# network 23.1.1.0 0.0.0.255 a 0
R3
# router ospf 300
# network 23.1.1.0 0.0.0.255 a 0
R1
# do show ip route
# do show ip ospf neighbor
```