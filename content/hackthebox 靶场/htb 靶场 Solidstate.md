---
UID: 20250615093948
aliases: 
tags:
  - 靶场
  - 渗透
  - hackthebox
source: 
cssclasses: 
created: 2025-06-15
---
![image.png](https://s2.loli.net/2025/06/15/KbV4sDTFd5LifIj.png)
[[htb solidstate 思]]
## 扫描端口
![image.png](https://s2.loli.net/2025/06/15/Geofq6RzTcrdXFi.png)


有 ssh 端口 web 服务端口和 pop 3 端口。可能这个 pop 3 端口有点作用

用 rustscan 全端口扫一下

![image.png](https://s2.loli.net/2025/06/15/vt3eDU7zbak9odj.png)

发现还有一个 4555 端口，先留着，万一有用

## web 服务信息收集
![image.png](https://s2.loli.net/2025/06/15/O73256PpcrIhVRl.png)


目录扫描也没出东西，只能说不在这边

![image.png](https://s2.loli.net/2025/06/15/w3LZRKXaxdS9ujG.png)

可能是没详细扫描端口
![](https://s2.loli.net/2025/06/15/PsAkx8FWoVbep7l.png)

## 漏洞利用
4555 疑似是个登录的系统。找找漏洞

![image.png](https://s2.loli.net/2025/06/15/nuBGfvDSgL1pRVE.png)

![image.png](https://s2.loli.net/2025/06/15/nhSWD3RvBE6ys5c.png)

但这个没迪奥用

## james 服务器改密码
只能老老实实登录服务器
```
nc ip port
```

![image.png](https://s2.loli.net/2025/06/15/BVsOWgEF7MclfXP.png)

挨个改密码，然后登录 pop 服务器进行检查是否有文件。

retr 用来查看邮件

在 mindy 中找到账密
![image.png](https://s2.loli.net/2025/06/15/9HIVgkWrutRmxvZ.png)

用这个来连接 ssh 
![image.png](https://s2.loli.net/2025/06/15/3itFdfcHNhLMpVu.png)

连接的时候发现，刚刚使用漏洞利用上传的反弹 shell 有个报错，忘记打开反弹 shell 的端口了

打开后再重新登录 ssh，会发现已经逃逸成功，是个无限制的 shell

![image.png](https://s2.loli.net/2025/06/15/nQiL2H6IkC4J1NP.png)

## 提权
![image.png](https://s2.loli.net/2025/06/15/7wb5fr8ZXtdzOBx.png)

找到第一个 flag

随便在系统中找找，发现有个 root 权限的脚本，还是 777 权限
![image.png](https://s2.loli.net/2025/06/15/SJd6DNMLImrxjzt.png)

![image.png](https://s2.loli.net/2025/06/15/kVKSuWJTwzvMPLm.png)


![image.png](https://s2.loli.net/2025/06/15/6yBQG8roY5lqhbc.png)


等段时间可以获得 root 权限的反弹 shell

![image.png](https://s2.loli.net/2025/06/15/imrxbnkGSWwyZ3h.png)


![image.png](https://s2.loli.net/2025/06/15/GlZ4gOpFHqC1R6s.png)
