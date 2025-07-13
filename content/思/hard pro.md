---
UID: 20250609150300 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-06-09
---

## Disguise
### 扫描端口
![image.png](https://s2.loli.net/2025/06/09/2BzTonuxMgVdIZ3.png)

### web 信息收集
![image.png](https://s2.loli.net/2025/06/09/c7rqAUsD1I32wMQ.png)

发现是个 wp 网站。wpscan 枚举一下
![image.png](https://s2.loli.net/2025/06/09/zfmMcyESlG9R2dw.png)

枚举出两个用户

用这两个用户爆破一下

但爆了五千个还是没爆出来。这个时候就得转换思路了

### 子域名枚举
```
wfuzz -c -w /usr/share/wordlists/amass/subdomains-top1mil-110000.txt -u http://disguise.hmv -H 'host:FUZZ.disguise.hmv' --hl 890
```
![image.png](https://s2.loli.net/2025/06/09/WOXKMIsa5VDewGk.png)

绑了/etc/host 之后，能访问
![image.png](https://s2.loli.net/2025/06/09/uF4AzvWObTSyah2.png)

有登录界面，可以随便注册一个账户，注册完登录进去发现，还是一样的。没有点变化
![image.png](https://s2.loli.net/2025/06/09/VG7aoOL61Idp8HD.png)


### 伪造 session
会发现这里又是一个知识盲区了。只能看 wp

![image.png](https://s2.loli.net/2025/06/09/EfyjlIKikUnt7JZ.png)

抓包会发现有个 dark_session。这个

![image.png](https://s2.loli.net/2025/06/09/7cwlPhnWuIyqpDB.png)


![image.png](https://s2.loli.net/2025/06/09/9xynS4l2AusdmLT.png)


![image.png](https://s2.loli.net/2025/06/09/BjQ2W85uSfzPosm.png)




