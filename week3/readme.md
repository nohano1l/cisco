# 9/20
## telnet
```
IOL(L3)R1
# hostname R1
# int e0/0
# ip addr 192.168.1.1 255.255.255.0
# no shut
IOL(L3)R2
# hostname R2
# int e0/0
# ip addr 192.168.1.2 255.255.255.0
# no shut
# do ping 192.168.1.1
# int lo1
# ip addr 1.2.3.4 255.255.255.255
# do sh ip int brief
# int lo2
# ip addr 2.3.4.5 255.255.255.255
# do sh ip int brief
R1
# exit
# line vty 0 4
# password cisco
# login
# transport input telnet
R2
# telnet 192.168.1.1
cisco
```
## ssh
```
# username user privilegs 15 secret user
# enable secret 1234
# ip domain-name example.com
# crypto key qenerate rsa
1024
# ip ssh version 2
# line vty 0 4
# login local
# transport input ssh
# exit
R2
telnet被關閉了
R1
# transport input ssh telnet
R2
# ssh -l user 192.168.1.1
1234

新增network(選擇management)
R1
# int e0/1
# ip addr dhcp
# no shut
# do sh ip int brief
開linux
# ssh user@ip
可以連接
```