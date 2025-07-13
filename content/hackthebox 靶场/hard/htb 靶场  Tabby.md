---
UID: 20250705160939
aliases: 
tags:
  - 打靶
  - 靶场
  - hackthebox
source: 
cssclasses: 
created: 2025-07-05
---

![image.png](https://s2.loli.net/2025/07/05/twRjBUCsPKM3qy2.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/07/05/53yYfQxeDj9cSnK.png)

扫描出 80 端口 22 端口和 8080 端口

![image.png](https://s2.loli.net/2025/07/05/QOystiPHzRWq3hG.png)


## 80 端口的信息收集
![image.png](https://s2.loli.net/2025/07/05/Dx4fSgZrUOyck8u.png)


用 gobuster 扫描文件
![image.png](https://s2.loli.net/2025/07/05/A158zLoXI2kMJdh.png)

发现有个 files，那说明可能存在文件包含漏洞。

![image.png](https://s2.loli.net/2025/07/05/QydcZ75ue1Ep3Kt.png)

```
http://megahosting.htb/news.php?file=statement
```

一个熟悉的格式，试试/../../../../etc/passwd

![image.png](https://s2.loli.net/2025/07/05/kIyBo4Ys96P5W8q.png)

发现有效果，可以考虑一下能访问日志吗，


![image.png](https://s2.loli.net/2025/07/05/FQnwZup3zYr9WLt.png)

先收集到这里，换一个端口找找看

## 8080 端口的信息收集
![image.png](https://s2.loli.net/2025/07/05/ekMvi7DZgmbpH6U.png)


![image.png](https://s2.loli.net/2025/07/05/6E1G4BkUVxwyYi2.png)

找到管理员密码，试试看。

没有作用，只能找别的方法
![image.png](https://s2.loli.net/2025/07/05/1ZhlazH4DRCNGTb.png)

仔细看文档会发现，用户存在/etc/tomcat 9/tomcat-users. xml 中，用刚刚的文件包含漏洞能不能爆出这个文档

![image.png](https://s2.loli.net/2025/07/05/iTLVU7a153IPtDz.png)

瞄了一眼 wp，发现我们是搭建一个本地的 tomcat 来找 tomcat-users. xml 这个文件，其实这个文件在/etc/中，我们是没有权限读取的

在/usr/share/tomcat 9/etc/tomcat-users. xml 的路径中查看源码就能查看到
![image.png](https://s2.loli.net/2025/07/05/yD3IsXcqYJ8OHEA.png)

成功登录
![image.png](https://s2.loli.net/2025/07/05/G5erf49M1l2HXOI.png)

在/manager 这个路径中，我们是没有权限访问的
![image.png](https://s2.loli.net/2025/07/05/l2o8XBjvsMi6Dyw.png)

模糊测试一波
```
wfuzz -u http://megahosting.htb:8080/manager/FUZZ -w /usr/share/wfuzz/wordlist/general/common.txt --hc 404
```

![image.png](https://s2.loli.net/2025/07/05/v4Cr7VH2LuxF96k.png)

有个 text 的路径

![image.png](https://s2.loli.net/2025/07/05/PT4W3SGguNanXF5.png)


用 curl 进行上传

