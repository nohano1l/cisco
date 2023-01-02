![1](https://github.com/nohano1l/cisco/blob/master/midtest/1.jpg)
```
vpc
`
ip 192.168.1.1 255.255.255.0 192.168.1.254
`
.
.
.
SW(Cisco IOL L2 icon改sw)
R(Cisco IOL Image改L3)
`
no
enable(en)
#config terminal(conf t)
#interface e0/0(int e0/0)
#ip addr 192.168.1.254 255.255.255.0
#no shutdown(no shut)
#int e0/1
#ip addr 192.168.2.254 255.255.255.0
#no shut

#end
#show ip interface brief(sh ip int br)
`

vpc ping 其他台
```

![2](https://github.com/nohano1l/cisco/blob/master/midtest/2.jpg)
```

```

![3](https://github.com/nohano1l/cisco/blob/master/midtest/3.jpg)
```

```

![4](https://github.com/nohano1l/cisco/blob/master/midtest/4.jpg)
```

```