---
UID: 20250605150954 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-06-05
---

# 流量分析  
  
1.被控机先向 TeamServer 发送心跳包，包中包含了主机信息和协商密钥等信息，而这些信息都被使用 RSA 公钥进行了加密放在了 Cookie 中 

2、server 端第一次心跳之后进入睡眠，并用私钥将数据包解开获得主机信息和协商密钥，基于协商密钥会生成新的 AES key 和 HMAC key 

3、睡眠时间过后，会再一次发送心跳包询问是否有新的命令，当有新的命令出现时会将其数据包加密后发送，而命令会作为该回应包的 body 发送 

4、被控端在接收到数据包之后进行解密获取命令，再将命令执行的结果加密后返回给 TeamServer，该次传输使用 POST 请求发送回 TeamServer 

5、TeamServer 解密之后能看到明文的回显 

6（不同情况）、当 3 中所说的睡眠时间过了之后，再次发送心跳包却没有收到新的指令信息，这时候 teamserver 就会返回空包


fscan














![image.png](https://s2.loli.net/2025/06/05/qsWnPMF28jHiOtU.png)
## 第一问

