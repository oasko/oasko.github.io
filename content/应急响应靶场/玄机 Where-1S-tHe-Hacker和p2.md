---
UID: 20250531174716
aliases: 
tags:
  - 打靶
  - 应急响应
source: 
cssclasses: 
created: 2025-05-31
---
# 1
![image.png](https://s2.loli.net/2025/05/31/92CZohHPSrQTEMm.png)

首先先把 d 盾拉到虚拟机进行测试，看看有什么木马吗

![image.png](https://s2.loli.net/2025/05/31/scxM6Pm2dC4ZyWp.png)


能找到第一个 webshell 和第二个 webshell，第二个 webshell 的密码是 pass

查看事件查看器，筛选出 4720，新建用户的 id。能找到隐藏用户的创建时间 2023/11/6 4:45:34
![image.png](https://s2.loli.net/2025/05/31/gpcziD81PV4WrSl.png)





![image.png](https://s2.loli.net/2025/05/31/t9hrgwfUOoIcd8y.png)



![image.png](https://s2.loli.net/2025/05/31/RSzcnWu6hHUv4eC.png)

![image.png](https://s2.loli.net/2025/05/31/GNsTupOl7nQd85v.png)


![image.png](https://s2.loli.net/2025/05/31/jquihVzFb5769Ds.png)

加入管理员组 4732

查看秘钥的日志太乱了，有点难绷，看了 wp

哈希传递攻击，NTLMSSP 可以试着搜一下这个字段的内容

![image.png](https://s2.loli.net/2025/05/31/6ePDuF9q4j18kg3.png)



Windows 安全隔离区。找到两个 cs 木马

![image.png](https://s2.loli.net/2025/05/31/CwpPKQkc5DjBmze.png)

![image.png](https://s2.loli.net/2025/05/31/HqaZ3oGpReUg6IE.png)


# p 2
![image.png](https://s2.loli.net/2025/05/31/KqizTwQ7IVvpLGE.png)

d 盾扫描的第一个 webshell就是答案
![image.png](https://s2.loli.net/2025/05/31/YEwxBuUb8O69Wnv.png)


![image.png](https://s2.loli.net/2025/05/31/bwG6kK4cCoxJlDS.png)

![image.png](https://s2.loli.net/2025/05/31/tmwWzbRMIgOCGkp.png)



用了 d 盾发现，guest 居然有管理员权限


![](https://s2.loli.net/2025/05/31/zpRTsHZegCfxSiP.png)


ftp
![image.png](https://s2.loli.net/2025/05/31/MzGkOE4AyqVINXi.png)


火绒扫描出一个病毒

![image.png](https://s2.loli.net/2025/05/31/WkbfCHVXLFvsNTo.png)

![image.png](https://s2.loli.net/2025/05/31/wjR5SuznEIp4QcB.png)



定时任务

![image.png](https://s2.loli.net/2025/05/31/MUqEA29ntVR61zf.png)

![image.png](https://s2.loli.net/2025/05/31/2FugsLZTkbPoKf5.png)

操作时间

web 管理员弱口令登录
![image.png](https://s2.loli.net/2025/05/31/AXUj4r1EwRBCJSI.png)

其实会发现，前面已经试了很多次登录，在 38 分 31 秒登录成功


webshell 回显地址

在07:38:53会发现回显地址，40 分 10 秒是开始连接

![image.png](https://s2.loli.net/2025/05/31/2SZym8xXgfNeqMT.png)


![image.png](https://s2.loli.net/2025/05/31/hbGjk9FYD3nlxiL.png)

好家伙，全是 php 木马

成功连接 webshell
![image.png](https://s2.loli.net/2025/05/31/Ibv97Cz1cxAfeal.png)

因为前面上传了 webshell，但后面都是返回 404 的代码，这个是第一个 200 回复



点点思路，这两个靶场各有各的重点，第一个靶场是事件查看器的筛选使用，这个我确实不到位。接着就是用户的权限不熟悉，d 盾扫到 guest 用户有管理员权限，其实是被黑客利用了。一开始没反应过来。木马病毒，可以用火绒剑，火绒，Windows 安全中心找找，很有可能在 Windows 中心被拦截了

第二个靶场，是日志审计比较重点。web 弱口令登录，查看回显地址（其实就是为了方便连接一句话木马，往往跟着 url 或者有 ip 的网站，后面跟着的状态码能看出是否连接成功）

##note