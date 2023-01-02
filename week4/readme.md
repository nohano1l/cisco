# 9/27
## 
```
IOL(L3)R1、R2、R3
R1
# hostname R1
# int e0/0
# ip addr 192.168.1.1 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.2.1 255.255.255.0
# no shut
# int lo 1
# ip addr 1.1.1.1 255.255.255.0
# no shut
# do show ip int brief
R3
# hostname R3
# int e0/0
# ip addr 192.168.1.3 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.3.3 255.255.255.0
# no shut
# int lo 1
# ip addr 3.3.3.3 255.255.255.0
# no shut
# do show ip int brief
R2
# hostname R2
# int e0/0
# ip addr 192.168.2.2 255.255.255.0
# no shut
# int e0/1
# ip addr 192.168.3.2 255.255.255.0
# no shut
# int lo 1
# ip addr 2.2.2.2 255.255.255.0
# no shut
# do show ip int brief
R1
# ip route 3.3.3.3 255.255.255.255 192.168.1.3
# do sh ip route
# do ping 3.3.3.3 source 1.1.1.1
R3
# ip route 1.1.1.1 255.255.255.255 192.168.1.1
R1
# do ping 3.3.3.3 source 1.1.1.1
```
## tftp
```
IOL(L3)R1
network(management)
linux(centos)
R1
# hostname R1
# int lo 1
# ip addr 1.1.1.1 255.255.255.255
# no shut
# end
vm1
# yum install -y xinetd tftp-server
# gedit /etc/xinetd.d/tftp
server_args = -c -s /var/lib/tftpboot
disable = no
# chmod 777 /var/lib/tftpboot/
# systemctl restart xinetd.service
# systemctl stop firewalld
# getenforce
# setenforce 0
# getenforce
# ifconfig
R1
# int e0/0
# ip addr dhcp
# no shut
# do sh ip int brief
# ping ip(vm)
# copy startup-config tftp:
192.168.157.128  //看自己的ip
Enter
vm
# cd /var /lib/tftpboot
# cat r1-confg
R1
# write erase
# reload
# int e0/0
# ip addr dhcp
# no shut
# do sh ip int brief
# end
# copy tftp: startup-config
r1-confg
# copy startup-config running-config
# sh ip int brief
```