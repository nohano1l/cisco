# 11/15
## vlan
```
IOL L2(改icon)sw1
vpc1,2,3,4
vpc1
# ip 192.168.1.1 255.255.255.0
vpc2
# ip 192.168.1.2 255.255.255.0
vpc3
# ip 192.168.1.3 255.255.255.0
vpc4
# ip 192.168.1.4 255.255.255.0
vpc1
# ping 192.168.1.2
# ping 192.168.1.3
# ping 192.168.1.4
sw1
# hostname sw1
# show vlan
# vlan 10
# name vlan10
# vlan 20
# name vlan20
# do show vlan
# interface range e0/0-1
# switchport mode access
# switchport access vlan 10
# do show vlan brief
# exit
# int range e0/2-3
# switchport mode acc
# sw ac vlan 20
# do show vlan brief
vpc1
# ping 192.168.1.2
O
# ping 192.168.1.3
X
# ping 192.168.1.4
X
```
## 
```
IOL L2(改icon)sw1,sw2
vpc1,2,3,4
vpc1
# ip 192.168.1.1 255.255.255.0
vpc2
# ip 192.168.1.2 255.255.255.0
vpc3
# ip 192.168.1.3 255.255.255.0
vpc4
# ip 192.168.1.4 255.255.255.0
sw1
# do show vlan brief
# exit
# int e0/0
# switch mode ac
# switch acc vlan 10
# exit
# int e0/2
# switch mode access
# switchport access vlan 10
# exit
# int e0/1
# switchport mode access
# switch acc vlan 20
# exit
# int e0/3
# switchport mode acc
# switchport acc vlan 20
# do show vlan brief
sw2
# vlan 10
# vlan 20
# do show vlan brief
# int range e0/0,e0/2
# switchport mode acc
# switchport acc vlan 10
# int range e0/1,e0/3
# switchport mode acc
# switch access vlan 20
# do show vlan brief
vpc1
# ping 192.168.1.2
X
# ping 192.168.1.3
O
# ping 192.168.1.4
X

sw1,sw2
# int e0/2
# switchport trunk encapsulation dot1q
# switchport mode trunk
# do show interface trunk
vpc1
# ping 192.168.1.3
O
# ping 192.168.1.4
X
vpc2
# ping 192.168.1.4
O

vpc5,6
sw1,2
# int e0/3
# no switchport mode acc
# no switchport access vlan 20
# do show vlan
vpc5 ping vpc6 原生vlan不會打標籤
改原生標籤
sw1,sw2
# int e0/2
# switchport trunk native vlan 10
# do show interface trunk
只允許特定vlan
# int e0/2
# switchport trunk allowed vlan 1,10
vpc2
# ping 192.168.1.4
X
```