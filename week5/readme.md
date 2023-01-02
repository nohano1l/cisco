# 10/4
## 浮動路由
```
IOL(3L)R1、R2、R3
R1
# hostname R1
# int lo 1
# ip addr 1.1.1.1 255.255.255.255
# no shut
# int e0/0
# ip addr 192.168.1.1 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.2.1 255.255.255.0
# no shut
# do show ip int brief
R2
# hostname R2
# int lo 1
# ip addr 2.2.2.2 255.255.255.255
# no shut
# int e0/0
# ip addr 192.168.2.2 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.3.2 255.255.255.0
# no shut
# do show ip int brief
R3
# hostname R3
# int lo 1
# ip addr 3.3.3.3 255.255.255.255
# no shut
# int e0/0
# ip addr 192.168.1.3 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.3.3 255.255.255.0
# no shut
# do show ip int brief
R1
# do ping 192.168.1.3
# do ping 192.168.2.2
# exit
# ip route 3.3.3.3 255.255.255.255 192.168.1.3
R3
# ip route 1.1.1.1 255.255.255.255 192.168.1.1
R1
# do ping 3.3.3.3 source 1.1.1.1
# do sh ip route
# ip route 3.3.3.3 255.255.255.255 192.168.2.2 100
R2
# ip route 3.3.3.3 255.255.255.255 192.168.3.3
R3
# ip route 1.1.1.1 255.255.255.255 192.168.3.2 100
R2
# ip route 1.1.1.1 255.255.255.255 192.168.2.1
R1
# int e0/0
# shutdown
# do show ip route
R3
# int e0/0
# shutdown
# do show ip route
R1
# do ping 3.3.3.3 source 1.1.1.1
```
## 動態路由
![]()
```
IOL(3L)R1、R2、R3
VPC1、VPC2、VPC3
vpc1
# ip 192.168.1.1 255.255.255.0 192.168.1.254
vpc2
# ip 192.168.2.1 255.255.255.0 192.168.2.254
vpc3
# ip 192.168.3.1 255.255.255.0 192.168.3.254
R1
# hostname R1
# router rip
# version 2
# no auto-summary
# network 12.1.1.0
# network 192.168.1.0
# do show run
# int e0/0
# ip addr 192.168.1.254 255.255.255.0
# no shut
# int e0/1
# ip addr 12.1.1.1 255.255.255.0
# no shut
# do show ip int brief
R2
# hostname R2
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.2.254 255.255.255.0
# no shut
# int e0/2
# ip addr 23.1.1.2 255.255.255.0
# no shut
# do show ip int brief
# router rip
# version 2
# no auto-summary
# network 12.1.1.0
# network 192.168.2.0
# network 23.1.1.0
# do show run
R3
# hostname R3
# int e0/0
# ip addr 12.1.1.3 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.3.254 255.255.255.0
# no shut
# do show ip int brief
# router rip
# version 2
# no auto-summary
# network 23.1.1.0
# network 192.168.3.0
R1
# do show ip route
R3
# do show ip route
vpc1
# ping 192.168.1.254
# ping 192.168.2.1
# ping 192.168.3.1
可以ping其他任何地方
# trace 192.168.3.1
```
AD管理距離

![]()

如果wireshark不能用
copy wireshark_wrapper.bat到Program Files > EVE-NG
打開wireshark_wrapper.bat改上自己電腦wireshark的位置