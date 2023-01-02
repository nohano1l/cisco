![1](https://github.com/nohano1l/cisco/blob/master/1213/1.jpg)
```
DHCPserver(R)
# en
# conf t
# hostname DHCPserver
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# exit
# ip dhcp pool Pool10
# network 192.168.10.0 255.255.255.0
# default-router 192.168.10.254
# dns-server 8.8.8.8
# exit
# ip dhcp pool Pool20
# network 192.168.20.0 255.255.255.0
# default-router 192.168.20.254
# dns-server 8.8.8.8
# exit
SW
# en
# vlan 10
# exit
# vlan 20
# exit
# do sh vlan br
# int e0/0
# switchport mode access
# switchport access vlan 10
# int e0/1
# switchport mode access
# switchport access vlan 20
# int e0/2
# switchport trunk encapsulation dot1q
# switchport mode trunk
R1
# en
# conf t
# hostname R1
# int e0/1
# ip addr 12.1.1.1 255.255.255.0
# no shut
# do ping 12.1.1.2
# int e0/0
# no shut
# int e0/0.10
# encapsulation dot1Q 10
# ip addr 192.168.10.254 255.255.255.0
# no shut
# ip helper-address 12.1.1.2
# exit
# int e0/0.20
# encapsulation dot1Q 20
# ip addr 192.168.20.254 255.255.255.0
# no shut
# ip helper-address 12.1.1.2
# exit
DHCP
# router ospf 1
# router-id 2.2.2.2
# network 12.1.1.0 0.0.0.255 a 0
R1
# router ospf 1
# router-id 1.1.1.1
# network 12.1.1.0 0.0.0.255 a 0
# network 192.168.10.0 0.0.0.255 a 0
# network 192.168.20.0 0.0.0.255 a 0
DHCP
# do sh ip route
vpc1
# ip dhcp
vpc2
# ip dhcp
互ping
```
![2](https://github.com/nohano1l/cisco/blob/master/1213/2.jpg)
```
sw2
# vlan 10
# exit
# vlan 20
# exit
# do sh vlan br
# int e0/1
# switchport mode access
# switchport access via 10
# int e0/2
# switchport mode access
# switchport access via 20
# int e0/0
# switchport trunk encapsulation dot1q
# switchport mode trunk
# exit
# vlan 255
# exit
# do sh vlan br
# int vlan 255
# ip addr 192.168.255.1 255.255.255.0
# exit
# ip default-gateway 192.168.255.254
L3sw1
# en
# conf t
# hostname l3sw1
# vlan 10
# exit
# vlan 20
# exit
# vlan 255
# exit
# do sh vlan br
# int vlan 10
# ip addr 192.168.10.254 255.255.255.0
# no shut
# int vlan 20
# ip addr 192.168.20.254 255.255.255.0
# no shut
# int vlan 255
# ip addr 192.168.255.254 255.255.255.0
# no shut
# do sh ip int br
vpn1
# ip 192.168.10.1 255.255.255.0 192.168.10.254
vpn2
# ip 192.168.20.1 255.255.255.0 192.168.20.254
L3sw1
# exit
# int e0/0
# no shut
# switchport trunk encapsulation dot1q
# switchport mode trunk
sw2
# int vlan 255
# no shut
# do sh ip int br
L3sw1
# exit
# username cisco password cisco
# ip domain-name test.com
# crypto key generate rsa
:1024
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
sw2
# username cisco password cisco
# ip domain-name test.com
# crypto key generate rsa
:1024
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
L3sw1
# exit
# int e0/1
# no switchport
# ip addr 12.1.1.1 255.255.255.0
# no shut
# do sh ip int br
# exit
# router ospf 1
# router-id 1.1.1.1
# network 12.1.1.0 0.0.0.255 a 0
# network 192.168.10.0 0.0.0.255 a 0
# network 192.168.20.0 0.0.0.255 a 0
# network 192.168.255.0 0.0.0.255 a 0
R3
# en
# conf t
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# router ospf 1
# router-id 2.2.2.2
# network 12.1.1.0 0.0.0.255 a 0
# exit
# do sh ip route
# exit
# ping 192.168.255.254
# ssh -l cisco 192.168.255.254
sw2
# ip route 0.0.0.0 0.0.0.0 192.168.255.254
# do sh ip route
R3
# ping 192.168.255.1
# ssh -l cisco 192.168.255.1
```