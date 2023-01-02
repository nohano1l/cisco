# 11/29
## spanning-tree
```
IOL L2(改icon)sw1,sw2,sw3
vpc1,2
sw1
# hostname sw1
sw2
# hostname sw2
# do show spanning vlan 1
sw3
# hostname sw3
# show spanning-tree
sw2
#int e0/0
# spanning-tree vlan 1 cost 100
# do show spanning vlan 1
sw3
# int e0/2
# spanning-tree portfast
# so show sp
```
```
sw1
# spanning-tree mat confiquration
# name Regionl
# instance 1 vlan 11-13
# instance 2 vlan 21-23
sw2
# spanning-tree mst configuration
# name Regionl
# revision 1
# instance 1 vlan 11-13
# instance 2 vlan 21-23
sw3
# spanning-tree mst configuration
# name Regionl
# revision 1
# instance 1 vlan 11-13
# instance 2 vlan 21-23
# do show sp mat con
# exit
# vlan 11
# exit
# vlan 12
# exit
# vlan 13
# exit
# vlan 21
# exit
# vlan 22
# exit
# vlan 23
# exit
# do show vlan status
# do show vlan brief
# no vlan 10
# no vlan 20
# no vlan 50
# no vlan 100
# no vlan 200
sw2
# do show vlan brief
# do show sp mat configuration
sw3
# do show sp mat conf
sw1,2,3
# spanning-tree mode mst
# do show sp
```
## port security
```
IOL L2(改icon)sw1
vpc1,2
vpc1
# ip 192.168.1.1 255.255.255.0
vpc2
# ip 192.168.1.2 255.255.255.0
# ping 192.168.1.1
sw1
# show mac address-table
# clear mac address-table dynamic
vpc1
# ping 192.168.1.2
sw1
# vlan 10
# exit
# do show vlan brief
# int range e0/0-1
# switchport mode access
# switchport access vlan 10
# exit
# clear mac address-table dynamic
vpc1
# ping 192.168.1.2
sw1
# show mac address-table
# int e0/0
# switchport port-security maximum 1
# switchport port-security mac-address sticky   //黏著
# switchport port-security violation shutdown   //違反就shutdown
# do show run
vpc1
# ping 192.168.1.2
sw1
# do show run
sticky 後面多了vpc1的mac address
vpc1
# show ip all
斷sw1,vpc1的連線，重連vpc3
vpc3
# ip 192.168.1.1 255.255.255.0
# ping 192.168.1.2
error
```
