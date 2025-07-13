---
UID: 20250521090410
aliases: 
tags:
  - 靶场
  - 渗透
  - vulnhub
  - linux
source: 
cssclasses: 
created: 2025-05-21
---

这个更是重量级。从枚举就吃了苦头。主要也是字典加上经验不够，压根在最开始就没注意到 cg-bin 目录，一直枚举别的目录了。中了兔子洞
接着就是 shellshock 漏洞，这个也是不知道。真没接触过。也怪自己，不怎么会有 google 搜索这些
最后就是用了 linpeas 发现 tcpdump，pspy 发现有个登录 ftp 还是啥的程序在定期运行。用 tcpdump 抓取流量包，能找到 ftp 的登录账户密码。切换成功用户之后，是发现了 root 权限运行的脚本，没想到 python 劫持，修改 python 库中的脚本 import 的库

##note


