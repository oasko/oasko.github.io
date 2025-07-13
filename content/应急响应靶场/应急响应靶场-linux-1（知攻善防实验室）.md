---
title: 应急响应靶场 linux 1（知攻善防实验室）
date: 2025-02-21 16:28:42
tags: 靶场 linux
top_img: transparent
---

花了4.8米搞了个夸克网盘vip下载这些靶机来练手 光下载这些就花费了点时间 还直接把我机哥干宕机了

话不多说来看看linux 1这个靶机

每个机子都是有个密码的 只能在网上找密码登录进去。登录完，第一步 现在桌面打开题解的脚本。

## 开搞
万事俱备 先看看历史记录有啥之前输入的命令吗？也许里面就有携带着一些信息。

![flag](/images/应急响应靶场-linux-1（知攻善防实验室）/flag1.png)

果不其然 还真有命令 修改了开机启动程序rc.local 第一个flag到手。

我们进这个文件里面检查一下

![flag2](/images/应急响应靶场-linux-1（知攻善防实验室）/flag2.png)

ok 又有一个flag

我们目前还剩下一个flag的攻击者的ip

既然是被攻击了 那么在/etc/passwd 和/etc/shadow里面估计会有点记录吧？进去看两眼

![ip](/images/应急响应靶场-linux-1（知攻善防实验室）/redis.png)

发现有个redis服务 目标明确了一点

检查一下redis的日志。我搜了一下，客户端连接会有个Accepted的字样。用grep命令过滤出Accepted字样的记录

![ip](/images/应急响应靶场-linux-1（知攻善防实验室）/ip.png)

出现了个ip (192.168.75.129) 突然有了个法子 用last -i 显示系统的登录历史记录，并让它显示每个登录会话的 IP 地址。 

![ip2](/images/应急响应靶场-linux-1（知攻善防实验室）/ip2.png)

这个ip好巧不巧就在这里面 那可以石锤了

第三个flag就离谱了，在redis的日志里面没找到，别的地方也找不到。然后想起来没去看配置文件

![flag3](/images/应急响应靶场-linux-1（知攻善防实验室）/flag3.png)

好机会 还真在这里面

任务完成！

![success](/images/应急响应靶场-linux-1（知攻善防实验室）/success.png)