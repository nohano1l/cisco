# 10/25
## IP SLA
```
IOL(3L)R1、R2、R3
R1
# hostname R1
# int e0/0
# ip addr 100.1.1.2 255.255.255.0
# no shut
# int e0/1
# ip addr 200.1.1.2 255.255.255.0
# no shut
R2
# hostname R2
# int e0/0
# ip addr 100.1.1.1 255.255.255.0
# no shut
# int lo1
# ip addr 123.1.1.1 255.255.255.0
# no shut
R3
# hostname R3
# int e0/0
# ip addr 200.1.1.1 255.255.255.0
# no shut
# int lo1
# ip addr 123.1.1.1 255.255.255.0
# no shut
R1
# exit
# ip route 0.0.0.0 0.0.0.0 100.1.1.1
# ip route 0.0.0.0 0.0.0.0 200.1.1.1 10     //10是costs
# do show ip route
# do ping 123.1.1.1
# end
# traceroute 123.1.1.1
# int e0/0
# shut
# do show ip route
# no shut
# do show ip route
R2
# int e0/0
# shut
R1
# do ping 123.1.1.1
不通
R2
# no shut
R1
# exit
# no ip route 0.0.0.0 0.0.0.0 100.1.1.1
# no ip route 0.0.0.0 0.0.0.0 200.1.1.1 10
# ip sla 1
# icmp-echo 100.1.1.1
# frequency 3
# exit
# ip sla schedule 1 life forever start-time now
# track 10 ip sla 1 rechability //編號10的追蹤事件
# exit
# ip route 0.0.0.0 0.0.0.0 100.1.1.1 track 10
# ip route 0.0.0.0 0.0.0.0 200.1.1.1 10
# do show ip route
R2
# int e0/0
R1
# do ping 123.1.1.1 repeat 1000000
R2
# shut
R1
ctrl+shift+6
# do show ip route
```
## 遞迴路由
```
R1
# ip route 1.2.3.4 255.255.255.255 5.5.5.5
# ip route 5.5.5.5 255.255.255.255 6.6.6.6
# ip route 6.6.6.6 255.255.255.255 7.7.7.7
# ip route 7.7.7.7 255.255.255.255 null 0
# do show ip route
# do show ip cef
//不啟動cef
# no ip cef
# do show ip cef
會一直查表，效率比較差
```
## EIGRP
```
IOL(3L)R1、R2、R3
R1
# hostname R1
# int e0/0
# ip addr 12.1.1.1 255.255.255.0
# no shut
R2
# hostname R2
# int e0/0
# ip addr 12.1.1.2 255.255.255.0
# no shut
# int e0/1
# ip addr 23.1.1.2 255.255.255.0
# no shut
R3
# hostname R3
# int e0/0
# ip addr 23.1.1.3 255.255.255.0
# no shut
R1
# router eigrp 10
# no auto-summary
# network 12.1.1.0 0.0.0.255
R2
# router eigrp 10
# no auto-summary
# network 12.1.1.0 0.0.0.255
# network 23.1.1.0 0.0.0.255
R3
# router eigrp 10
# no auto-summary
# network 23.1.1.0 0.0.0.255
R1
# do show ip route
# do show ip eigrp neighbors
# do show ip eigrp topology detail-links
```
## 安全性
```
R1
# key chain mychain
# key 1
# key-string 123456
# int e0/0
# ip authentication key-chain eigrp 10 mychain
# ip authentication mode eigrp 10 mds
# do show ip eigrp neighbors
R2
# key chain mychain
# key 1
# key-string 123456
# int e0/0
# ip authentication key-chain eigrp 10 mychain
# ip authentication mode eigrp 10 mds
R1
# do show ip eigrp neighbors
```