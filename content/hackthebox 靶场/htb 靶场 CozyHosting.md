---
UID: 20250623163218
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-06-23
---

![image.png](https://s2.loli.net/2025/06/23/PGqKxYo26eCJld7.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/06/23/bAO2IELgRWxluSH.png)

有 web 服务端口和 ssh 端口

## web 服务信息收集
![image.png](https://s2.loli.net/2025/06/23/uhANq9JPcKBLUbC.png)


有个登录界面

![image.png](https://s2.loli.net/2025/06/23/15z86r4aqeKELVO.png)

看登录界面是 bootstrapmade的

有个很奇怪的一点，就是没有什么目录，也没有什么 txt 文件之类的。

![image.png](https://s2.loli.net/2025/06/23/Elkqy17ArSxaMcJ.png)

有个报错界面

没有头绪了，就在 bootstrapmade 官网转转，发现了靶场的模版。估计我们可以从中找到有什么可以利用的地方吗

![image.png](https://s2.loli.net/2025/06/23/8QGmIbl9AoWi7y5.png)

也没发现。就回到报错页面，搜了一下发现是个 java spring boot 的报错页面

那我们就使用 spring boot 的目录枚举

![image.png](https://s2.loli.net/2025/06/24/hL2aTM74nxqHdBs.png)

在里面找到 cookie 值
![image.png](https://s2.loli.net/2025/06/24/sckujzweGoO3EvU.png)

我们在登录界面随便输入东西，就会新增加一个 cookie 值
![image.png](https://s2.loli.net/2025/06/24/B3VfDx4CoNvd5bl.png)



