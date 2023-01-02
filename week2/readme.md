# 9/13
## 
```
先到C槽program files - EVE-NG 找wireshark_wrapper.bat改password="pnet"
下載winserver-2008-lite-u1到pnet /opt/unetlab/addons/qemu
pnet
# cd /opt/unetlab/addons/qemu/winserver-2008-lite-u1
# mv hda-001.qcow2 hda.qcow2
新增windows server新增linux選tinycore新增network選management
```
## dhcp server
![]()
```
IOL(l3)R1
no
en
conf t
# hostname R1
# int e0/0
# ip addr 192.168.1.254 255.255.255.0
# no shut
# do sh ip int brief
# exit
# ip dhcp pool1
# network 192.168.1.0 255.255.255.0
# default-router 192.168.1.254
# dns-server 8.8.8.8
# exit
# ip dhcp excluded-address 192.168.1.1 192.168.1.9
# ip dhcp excluded-address 192.168.1.101 192.168.1.254 
IOL(png sw)sw
vpc1、vpc2
# ip dhcp
# show ip
互ping
# wr
# sh running-config
```