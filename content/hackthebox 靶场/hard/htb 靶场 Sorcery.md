---
UID: 20250619212829
aliases: 
tags:
  - 打靶
  - 靶场
  - hackthebox
source: 
cssclasses: 
created: 2025-06-19
---

## 扫描端口
找出来了一个 443 端口和 ssh 端口
![image.png](https://s2.loli.net/2025/06/19/IFCDnLHac56rblQ.png)

## 信息收集
![image.png](https://s2.loli.net/2025/06/19/1PxHTLm46f9tliu.png)



![image.png](https://s2.loli.net/2025/06/19/3A9exWrShjdJ1I7.png)


有用的是一个 gitea 的网页和一个登录界面。只能找找 gitea 中存在的信息吧

![image.png](https://s2.loli.net/2025/06/19/wUtfJWbSylEgKoa.png)

发现是存在 sql 注入的，只能看看是哪里存在 sql 注入

## 目录扫描
