## PNETLab
![]()
sw
cisco IOL L2 改icon
vpc3
    ip 192.168.1.1 255.255.255.0 192.168.1.254
vpc2
    ip 192.168.1.2 255.255.255.0 192.168.1.254
vpc

    ip 192.168.2.1 255.255.255.0 192.168.2.254
    show ip

IP/MASK     : 192.168.2.1/24
GATEWAY     : 192.168.2.254
DNS         :
MAC         : 00:50:79:66:68:05
LPORT       : 20000
RHOST:PORT  : 127.0.0.1:30000
MTU         : 1500

R4
cisco IOL Image改L3

    no
    enable #進入特權模式會從 > 變成 #
    show ip interface brief #查看介面卡資訊(sh ip int br)
    config terminal #回到設定模式(conf t)
    interface ethernet 0/1 (int e0/1)#需切換到想要設定的介面
    ip addr 192.168.2.254 255.255.255.0
    no shutdown (no shut)#啟動
    exit
    int e0/0
    ip addr 192.168.1.254 255.255.255.0
    no shut
    exit
    show ip int brief

vpc

    ping 192.168.2.254
//通常第一個封包會timeout是因為arp還沒有解析完成
    ping 192.168.1.254
    ping 192.168.1.1

當以上完成 表示網路拓譜 設定完成

## 基本IP概念
IP位址 子網路遮罩(Mask) 預設閘道(Gateway)
`
子網路遮罩就是用來判斷目的地是不是同一個網段， 當目的地不是同一個網段時就會把資料交給預設閘道(路由器)來轉送。
`
QA:ip中的/24
`
/24 ＝ 255.255.255.0
意思就是指 SubnetMASK 有 24 個 「1」，就等於 255.255.255.0（25個 1＝ 11111111.11111111.11111111.00000000）
`