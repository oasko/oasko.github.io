---
UID: 20250620231642
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-06-20
---

![image.png](https://s2.loli.net/2025/06/20/UHm2JReEINjoXri.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/06/21/I6DnCjEkLhGlNBO.png)

有个 ssh 服务和 web 服务
需要绑一个 pilgrimage. htb 的域名

## 信息收集
![image.png](https://s2.loli.net/2025/06/21/fAUiGnYV6pgWkE3.png)

在网站中发现可以直接注册一个用户，还是可以上传文件的注入点

想办法上传一个反弹 shell

但看了一眼上传的格式，只能上传 png 格式，只能从图片马下手了

上传成功
![image.png](https://s2.loli.net/2025/06/21/Bs1btnuHlXqiL3T.png)

给出来了网址，访问这个网址能显示照片，但反弹 shell 不成功

又用了蚁剑去连接
![image.png](https://s2.loli.net/2025/06/21/2wJRhLTXSrs1euD.png)

发现是 405 报错，方法不被允许，可能是请求方式出问题了。很奇怪

## 目录扫描
![image.png](https://s2.loli.net/2025/06/21/yabwnj1I2EKqAFH.png)

找到一个 git 文件，用 git-dumper 拉取文件

![image.png](https://s2.loli.net/2025/06/21/fOg9Zd1Wy2sNKHb.png)
