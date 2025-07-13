---
UID: 20250615125843 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-06-15
---

![image.png](https://s2.loli.net/2025/06/15/QlZOcgPF2mes9dL.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/06/15/ECvWhnGaw2eFJdm.png)




## web 服务信息收集

首页是个可以查看以下几种 php 文件的页面。bp 抓包测试一下
![image.png](https://s2.loli.net/2025/06/15/3DHVbfphWndA9Bk.png)


![image.png](https://s2.loli.net/2025/06/15/N8G9cC5H4DVPYiR.png)

原本是这样的，输入/etc/passwd 试试

![image.png](https://s2.loli.net/2025/06/15/wN6sz3lRnub51qP.png)

还真可以。现在就要考虑一下，查看别的文件。如 shadow 文件，日志文件等

用 whatweb 扫描出是 apache 的服务器

![image.png](https://s2.loli.net/2025/06/15/kGS1ubg9pZwxe8t.png)

搜 shadow 文件弹出报错，只能打开在指定路径下的文件

![image.png](https://s2.loli.net/2025/06/15/xOg1eZKut9nQV8y.png)


绕过也没辙

![image.png](https://s2.loli.net/2025/06/15/hDjdyuTBGLz135E.png)


用伪协议查看了这个 php 文件

![image.png](https://s2.loli.net/2025/06/15/IUhn76ZGmOXbHiy.png)

发现就是简单的，没有一点过滤。但很奇怪
![image.png](https://s2.loli.net/2025/06/15/rXsRIzKMECh3lgy.png)

可能是只能查到指定文件夹的有的文件，但问题是我们不知道这个文件里面有啥内容

继续换别的文件
![image.png](https://s2.loli.net/2025/06/15/WzvHRCnTfNBuKV3.png)


![image.png](https://s2.loli.net/2025/06/15/k3cyrdDR16KIfM8.png)

依旧没有

在看主页的时候留意到一个 php 里面的内容
![image.png](https://s2.loli.net/2025/06/15/6dB51notqER4zhk.png)

最后是生成了一个 pwdbackup. txt
![image.png](https://s2.loli.net/2025/06/15/31P4eyfN7r8soxi.png)

![image.png](https://s2.loli.net/2025/06/15/ZtKBIgDsmyHWVdL.png)

加密十三遍的密码

![image.png](https://s2.loli.net/2025/06/15/VSpIHw7KqMen1NP.png)

十三遍 base 64 加密，拉到 cyberchef 解密，能找到密码


## ssh 登录成功
![image.png](https://s2.loli.net/2025/06/15/ajDPrFe2zwbRcTY.png)

![image.png](https://s2.loli.net/2025/06/15/CaoKRmfg5IeSzHt.png)

可能是 rbash

![image.png](https://s2.loli.net/2025/06/15/cEzLN1QRZ3T7XoV.png)

找到 flag。有个 zip，试着解压一下

![image.png](https://s2.loli.net/2025/06/15/5mOGj9IDAogVfyE.png)

需要密码

## 提权
![image.png](https://s2.loli.net/2025/06/15/jvSqylBefg4E8d3.png)

用了 netstat  -an 找到两个端口


给这两个端口做 ssh 端口转发

![image.png](https://s2.loli.net/2025/06/15/FcUywE3bZYzPdmM.png)

是 vnc
![image.png](https://s2.loli.net/2025/06/15/DTJGLghlEPOimBp.png)


看了 wp，发现那个加密文件有作用
![image.png](https://s2.loli.net/2025/06/15/TlOtMS5hBdc4vPq.png)


```
vncviewer -passwd secret 127.0.0.1:5901
```

![image.png](https://s2.loli.net/2025/06/15/AUEafbwHCXBngTp.png)


成功登录，并获得 root 权限
![image.png](https://s2.loli.net/2025/06/15/A8xOGKvDU9ogX4p.png)
