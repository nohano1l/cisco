## EtherChannel
```
IOL(png sw)sw1,sw2
# sw1
# hostname sw1
# int range e0/0-1
# switchport trunk encapsulation dot1q
# switchport mode trunk
# channel-group 1 mode on
sw2
# hostname sw2
# int range e0/0-1
# switchport trunk encapsulation dot1q
# switchport mode trunk
# channel-group 1 mode on
sw1
# exit
# do sh etherchannel summary
# do sh int port-channel 1
sw1,sw2
# default interface e0/0
# default interface e0/1
# int range e0/0-1
# no switchport
# channel-group 2 mode on
# exit
# int port-channel 2
sw1
# ip addr 192.168.1.1 255.255.255.0
sw2
# ip addr 192.168.1.2 255.255.255.0
# do show ip int brief
# do sh etherchannel summary
```
## 
```
IOL(L3)R1,R2,R3
R1
# hostname R1
# int lo1
# ip addr 192.168.1.1 255.255.255.0
# no shut
# int lo2
# ip addr 192.168.2.1 255.255.255.0
# no shut
# do show ip int brief
# exit
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
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
# ip addr 32.1.1.3 255.255.255.0
# no shut
# int lo1
# ip addr 3.3.3.3 255.255.255.255
# no shut
# int lo2
# ip addr 8.8.8.8 255.255.255.255
# no shut
# do show ip int brief
R1
# exit
# ip route 0.0.0.0 0.0.0.0 12.1.1.2
# do sh ip route
R2
# exit
# ip route 192.168.1.0 255.255.255.0 12.1.1.1
# ip route 192.168.2.0 255.255.255.0 12.1.1.1
# do sh ip route
R1
# do ping 12.1.1.2 source 192.168.1.1
# do ping 12.1.1.2 source 192.168.2.1
R2
# ip route 0.0.0.0 0.0.0.0 23.1.1.3
# do show ip route
R1
# do ping 8.8.8.8 source 192.168.1.1
不行
R2
# access-list 1 permit 192.168.1.0 0.0.0.255
# access-list 2 permit 192.168.2.0 0.0.0.255
# ip nat pool PAT 23.1.1.2 23.1.1.2 netmask 255.255.255.0
# int e0/0
# ip nat inside
# int e0/1
# ip nat outside
# ip nat inside source list 1 pool PAT overload
R1
# do ping 8.8.8.8 source 192.168.1.1
R2
# do sh ip nat translations

Q:R1 # do ping 8.8.8.8 source 192.168.2.1
不行
A:R2 # ip nat inside source list 1 pool PAT overload
  R1 # do ping 8.8.8.8 source 192.168.2.1

R1
# username cisco secret cisco
# ip domain-name cisco.com
# crypto key generate rsa
1024
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
R2
# ssh -l cisco 12.1.1.1
cisco
>exit
# conf t
# ip nat inside source static tcp 12.1.1.1 22 23.1.1.2 22   //tcp 內 外
# do sh ip nat translations
R3
# ssh -l cisco 23.1.1.2
cisco
```