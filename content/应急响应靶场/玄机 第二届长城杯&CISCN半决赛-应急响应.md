---
UID: 20250604223648
aliases: 
tags:
  - 打靶
  - 应急响应
source: 
cssclasses: 
created: 2025-06-04
---


![image.png](https://s2.loli.net/2025/06/04/9xN6CsYFjugrUpe.png)

在日志中找到有经常运行这个命令，cat 查看一下

![image.png](https://s2.loli.net/2025/06/04/2cGQdZ3msTuAfSg.png)

是个程序，导出拉到微步沙箱，发现是个木马，保持持久性的

![image.png](https://s2.loli.net/2025/06/04/6Z1hAXWaTC4qDoc.png)

/lib/modules/5.4.0-84-generic/kernel/drivers/system/system-upgrade. ko

![image.png](https://s2.loli.net/2025/06/04/NRrk6MxvVKAsymH.png)


/usr/bin/touch /etc/systemd/system/system-upgrade. service -r /etc/systemd/system/syslog. service


发现是个持久性程序之后，拉到 ida 进行解析。因为题目说，在持久性程序中。会有运行木马的程序，

![image.png](https://s2.loli.net/2025/06/04/aVZhe7o85ws9kGS.png)


发现木马，前面则是 ip 和端口

![image.png](https://s2.loli.net/2025/06/04/d9fWQP5waH4l23t.png)

其实在日志中也能找到蛛丝马迹

![image.png](https://s2.loli.net/2025/06/04/QrqPSsbWMDpmaJR.png)



原始名
一开始也是没想到，用这个工具能复原出攻击者运行的命令
![image.png](https://s2.loli.net/2025/06/04/Q5VqdteK3m1DrFh.png)


![image.png](https://s2.loli.net/2025/06/04/PDv6FJaXgL8wGix.png)

在其中找到持久性程序的原名



逆向
![image.png](https://s2.loli.net/2025/06/04/uxOjEBr9hoIwZte.png)
