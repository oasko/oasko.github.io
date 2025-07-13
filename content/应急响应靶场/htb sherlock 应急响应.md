---
UID: 20250609213814
aliases: 
tags:
  - 应急响应
source: 
cssclasses: 
created: 2025-06-09
---

## Brutus
![image.png](https://s2.loli.net/2025/06/09/QozskSCv6baF8fH.png)

### 问题一二
![image.png](https://s2.loli.net/2025/06/09/vdKXMlx4f9VESY2.png)


#### 第一问
![image.png](https://s2.loli.net/2025/06/09/FIphyHrZoaztMRL.png) 我们会发现 65.2.161.68 这个 ip 一直在攻击 ssh

#### 第二问
![image.png](https://s2.loli.net/2025/06/09/SEFqCDgU7A6aGTy.png)

发现是 root 用户登录成功
### 问题三四
![image.png](https://s2.loli.net/2025/06/09/osJSbd7Bm4NF6Vf.png)

#### 第三问
utmpdump wtmp
但 kali 中一直没有很奇怪

#### 第四问
找 session 和 root。发现是 37
![image.png](https://s2.loli.net/2025/06/09/UHsSQlpfIwt6vN8.png)

#### 第五问
会发现新建了一个用户 cyberjunkie
![image.png](https://s2.loli.net/2025/06/09/8JhtZvLjg4qGxBN.png)


### 问题五六七
![image.png](https://s2.loli.net/2025/06/09/NeTjPYmDCWzvulB.png)

#### 第五问
![image.png](https://s2.loli.net/2025/06/09/k4TLiYqlsXxmgMC.png)

 T1136.001
#### 第六问

？ 奇葩看了 wp


#### 第七问
![image.png](https://s2.loli.net/2025/06/09/QjTypBXh74G6CPc.png)

找到了

## 