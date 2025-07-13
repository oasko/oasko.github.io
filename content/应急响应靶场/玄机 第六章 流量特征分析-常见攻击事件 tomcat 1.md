---
UID: 20250602104335 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-06-02
---
![image.png](https://s2.loli.net/2025/06/02/652JXvnIMi8Fzax.png)


## 第一问
![image.png](https://s2.loli.net/2025/06/02/heB9HatcRsIEyAD.png)


扫描会发现下面有一大串来自 14.0.0.120 的 ip 的扫描。因为是 14.0.0.120 sys 扫描 10.0.0.12 这个 ip，12 会返回一个 rst 的包。所以能分析出是 14.0.0.120 这个 ip 是攻击者


## 第二问
使用站长之家能查到 ip 所在地

![image.png](https://s2.loli.net/2025/06/02/mljDFXyvWnH5R6C.png)

## 第三问
查看攻击者的http 包会发现，攻击者使用的 wb 服务是 8080 端口

![image.png](https://s2.loli.net/2025/06/02/ubRmIixLZEo5hqV.png)

## 第四问
在检查流量包的时候。发现是 gobuster 的代理

![image.png](https://s2.loli.net/2025/06/02/hT3mePN6ASbpsuj.png)

那就是使用 gobuster 工具
## 第五问 
找用户密码，这个是看了 wp。其实能给 tomcat 上传文件的时候就应该是登录成功了，没认真看流量包。流量包里头有个加密过后和账户密码

![image.png](https://s2.loli.net/2025/06/02/4HG3CBOc1KTYW6L.png)

是个 base 64 加密

![image.png](https://s2.loli.net/2025/06/02/hBxHt2vI3kNPjib.png)


##  第六问
找 post 包。能找到攻击者发的木马

![image.png](https://s2.loli.net/2025/06/02/dFHK6RzxoDEAi84.png)


慢慢往下找能看到攻击者在干什么
![image.png](https://s2.loli.net/2025/06/02/y7G569rLEfHWzDN.png)


## 第七问
发现有个反弹 shell，这个就是权限维系的命令
![image.png](https://s2.loli.net/2025/06/02/AEgzQ4ty7qXxv9J.png)
