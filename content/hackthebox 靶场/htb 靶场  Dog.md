---
UID: 20250624221624
aliases: 
tags:
  - 打靶
  - 靶场
  - hackthebox
source: 
cssclasses: 
created: 2025-06-24
---

![image.png](https://s2.loli.net/2025/06/24/6vc2paYnuxBCTdU.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/06/24/T9Ktp4WXHvM7eVR.png)

在扫描端口的时候，发现 ssh 服务端口，web 服务端口

80 端口找到很多个可疑文件，有个 git 文件，可以提取出源码

![image.png](https://s2.loli.net/2025/06/24/bokzHyYl279SxZV.png)


用 git dumper 拉取源码，查找里面的信息。

会发现是个 Backdrop CMS。检查一下会有漏洞利用吗
![image.png](https://s2.loli.net/2025/06/24/jha5HGSNft6ogXZ.png)


