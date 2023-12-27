---
layout: post
title:  "NAT穿透"
date:   2020-03-13 07:58:58 +0800
categories: nat traversal network
---



## NAT



> NAT traversal



### NAT实现方式

1）静态NAT

<img src="https://blog-10039692.file.myqcloud.com/1505808407153_9340_1505808407349.png" alt="img" style="zoom:50%;" />

2）NAPT

<img src="https://blog-10039692.file.myqcloud.com/1505808416923_4531_1505808417110.png" alt="img" style="zoom:50%;" />



### NAT的主要类型



#### Full Cone

内部机器Client访问外网机器C，NAT打开一个端口，后面外网的任意ip和任意port都可以访问这个端口，也就是任意ip+任意port可以访问机器Client。

<img src="https://blog-10039692.file.myqcloud.com/1505808434127_6861_1505808434549.png" alt="img" style="zoom: 25%;" />



#### Address Restricted Cone

内部机器Client访问外网机器A，NAT打开一个端口，后面机器A的任意port可以访问这个端口，就是只能固定ip+任意port访问Client。

<img src="https://blog-10039692.file.myqcloud.com/1505808448549_1177_1505808449018.png" alt="img" style="zoom:25%;" />





#### Port Restricted Cone

内部机器Client访问外网机器A，NAT打开一个端口，后面机器A的固定port可以访问这个端口，就是只能固定ip+固定port访问Client。

<img src="https://blog-10039692.file.myqcloud.com/1505808448549_1177_1505808449018.png" alt="img" style="zoom:25%;" />



#### Symmetric

连接不同的外部Server，NAT打开的端口会变化。也就是内部机器Client连接外网机器A时，NAT会打开一个端口，连接外网机器B时又会打开另外一个端口。

<img src="https://blog-10039692.file.myqcloud.com/1505808476505_6782_1505808476983.png" alt="img" style="zoom:25%;" />



## NAT穿越技术



1. UPnP，把NAT端口（对外）映射到UPnP端口，再由UPnP程序处理（我们自己写的符合UPnP程序）。迅雷，电骡基于这个方式实现。需要操作系统支持UPnP。
2. ALG(应用层网关),如同一个NAT的插件，让NAT能识别插入的协议从而避免打洞。确定是需要系统支持插入的协议，比如，FTP，SIP，DNS等。
3. STUN/TURN/ICE，相比上面2者





- SBC：？？TODO



### 应用场景



- 对等网络
- VoIP领域
- 网络视频监控



### UDP打洞算法

> 英文名：UDP hole punching



假设有两台分别处于各自的私有网络中的主机：A和B；N1和N2是两个网络的NAT设备，分别拥有IP地址P1和P2；S是一个使用了一个众所周知的、从全球任何地方都能访问得到的IP地址的公共服务器



1. A和B分别和S建立UDP连接；NAT设备N1和N2创建UDP转换状态并分配临时的外部端口号
2. S检查UDP包，看A和B的端口是否是正在被使用的（否则的话N1和N2应该是应用了端口随机分配，这会让路由验证变得更麻烦）
3. 如果端口不是随机化的，那么A和B各自选择端口X和Y，并告知S。S会让A发送UDP包到P2:Y，让B发送UDP包到P1:X
4. A和B通过转换好的IP地址和端口直接联系到对方的NAT设备；

[参考](https://zh.wikipedia.org/wiki/UDP%E6%89%93%E6%B4%9E)



### TPC遇到的问题

TCP和UDP在打洞上不同，这是因为伯克利socket的API造成的：UDP的socket允许多个socket绑定到同一个本地端口，而TCP的socket则不允许。



1. A B要连接到S，肯定首先A B双方都会在本地创建一个socket，去连接S上的socket。创建一个socket必然会绑定一个本地端口。（假设为8888，这样A和B才分别建立了到 S的通信信道）
2. 接下来就需要打洞了，打洞则需要A和B分别发送数据包到对方的公网IP。但是问题就在这里，因为NAT设备是根据端口号来确定session，

- 如果是UDP的socket，A B可以分别再创建socket，然后将socket绑定到8888，这样打洞就成功了。
- 但是如果是TCP的socket，则不能再创建socket并绑定到8888了，这样打洞就无法成功。

TODO：还是不明白



[参考](https://juejin.im/entry/5b5d9232f265da0f9f4e7448)



### TCP打洞-TODO



[链接：非常好的文章](http://www.52im.net/thread-542-1-1.html)

[Wiki](https://en.wikipedia.org/wiki/TCP_hole_punching)







## STUN原理

假设有如下UAC（B），NAT（A），SERVER（C）。

UAC的IP为IPB，NAT的IP为 IPA ，SERVER的 IP为IPC1 、IPC2。请注意，服务器C有两个IP。 



### STEP1

B向C的IP1的pot1端口发送一个UDP 包。C收到这个包后，会把它收到包的源IP和port写到UDP包中，然后把此包通过IP1和port1发还给B。

当B收到此UDP后，把此UDP中的IP和自己的IP做比较，如果是一样的，就说明自己是在公网。



### STEP2

B向C的IP1发送一个UDP包，请求C通过另外一个IP2和PORT（不同与SETP1的IP1）向B返回一个UDP数据包。

如果B收到了这个数据包，那说明什么？说明NAT来着不拒，不对数据包进行任何过滤，这也就是STUN标准中的full cone NAT。



### STEP3

B向C的IP2的port2发送一个数据包，C收到数据包后，把它收到包的源IP和port写到UDP包中，然后通过自己的IP2和port2把此包发还给B。  

如果这个port和step1中的port一样，那么可以肯定这个NAT是个CONE NAT，否则是对称NAT。

> 道理很简单：根据对称NAT的规则，当目的地址的IP和port有任何一个改变，那么NAT都会重新分配一个port使用。
>
> 而在step3中，和step1对应，我们改变了IP和port。因此，如果是对称NAT,那这两个port肯定是不同的。 



### STEP4

B向C的IP2的一个端口PD发送一个数据请求包，要求C用IP2和不同于PD的port返回一个数据包给B。 

如果B收到了，那也就意味着只要IP相同，即使port不同，NAT也允许UDP包通过。显然这是restrict cone NAT。如果没收到，那就是port restrict NAT.。



[参考](https://blog.csdn.net/rudyn/article/details/25232303)





<img src="https://img-blog.csdn.net/20180503220356422" alt="img" style="zoom:150%;" />



## 实现



[PJNATH](https://www.pjsip.org/pjnath/docs/html/index.htm?spm=a2c4e.10696291.0.0.4d9919a4qR66yE)







## 其它



UDP的协议进行数据传输穿透NAT的成功率比较高，接近100%。

TCP则存在一些情况无法实现穿越，主要受限路由器的端口映射机制。

要实现NAT穿越，需要有穿越控制服务器部署在互联网（有固定的域名或者IP），由该服务器来协助网络摄像机和客户端来实现NAT穿越。

有些服务器还能在TCP不能穿越的情况下，实现RELAY(数据中继转发)的功能，以确保二者之间能实现数据通信。



最后笔者给出几个技术方案的建议，有兴趣的朋友可以自己再去做深入研究，欢迎探讨。

1. NAT穿越库的选择，笔者推荐PJSIP，网路摄像机以及客户端都可以采用。

2. NAT穿越控制服务器的选择，笔者推荐OPENSIPS。

3. 可靠UDP传输方案的选择，推荐UDT。





## 参考



[Wiki](https://zh.wikipedia.org/wiki/NAT%E7%A9%BF%E9%80%8F)

