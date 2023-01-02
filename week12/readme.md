# 11/22
## DTP
```
IOL L2(改icon)sw1,sw2,sw3
sw1
# hostname sw1
# vtp domain test
# vtp password cisco
# do show vtp status
# vlan 10
# exit
# vlan 20
# exit
# vlan 30
# exit
# do show vlan brief
# do show vtp status
# int e0/0
# switchport trunk encapsulation dot1q
# switchport mode trunk
sw2
# hostname sw2
# int e0/0
# switchport trunk encapsulation dot1q
# switchport mode trunk
# do show int trunk
# vtp domain test
# vtp password cisco
# vtp mode client
# do show vtp status
# do show vlan brief
vlan自動建立
sw1
# vlan 40
# exit
# vlan 50
# exit
sw2
# do show vlan brief
sw1
# no vlan 40
# no vlan 50
sw2(client)
# do show vlan brief
# vlan 100
X
sw3
# vtp domain test
# vtp password cisco
預設會是server，網路會亂掉，sw3可以設vlan
# vtp mode transparent      //透明模式
# vlan 200
# exit
# vlan 300
# exit
# do show vlan brief
sw1(server)
# do show vlan brief
沒有增加vlan

//client影響server
sw1,2
# int e0/0
# shut
sw2
# vtp mode server 
# vlan 100
# exit
# vlan 200
# exit
# do show vlan brief
# vtp mode client
sw1,2
# int e0/0
# no shut
sw1
# do show vlan brief
```
## root
```
sw1,2
# show spanning-tree
vpc1,2
vpc1
# ip 192.168.1.1 255.255.255.0
vpc2
# ip 192.168.1.2 255.255.255.0
更改root bridge
sw2
# spanning-tree vlan 1 root primary
# do show spanning-tree vlan 1
```