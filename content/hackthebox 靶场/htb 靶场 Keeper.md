---
UID: 20250620144534
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-06-20
---
![image.png](https://s2.loli.net/2025/06/20/XbOKuDz4VZfeQrW.png)

[[htb 靶场 keeper 思]]
## 扫描端口
发现有个 22 端口和 80 端口
![image.png](https://s2.loli.net/2025/06/20/jklih68ATuRJFIO.png)

## web 服务信息收集
```
echo "10.10.11.227 tickets.keeper.htb keeper.htb" | sudo tee -a /etc/hosts 需要添加域名进去
```
![image.png](https://s2.loli.net/2025/06/20/Yv8jzpamtO9oVT3.png)

![image.png](https://s2.loli.net/2025/06/20/UEQptz2Dh6lLi5e.png)

发现是个登录页面。寻找一下漏洞
![image.png](https://s2.loli.net/2025/06/20/3sGbxeCTNFmMqAt.png)

漏洞利用不了，就看看默认的账户凭证

![image.png](https://s2.loli.net/2025/06/20/8ePXNL7hruB1si5.png)

![image.png](https://s2.loli.net/2025/06/20/pfcSakEbL8Mzur5.png)

登录成功

在用户与组中找到一个账户和密码，随便试试
![image.png](https://s2.loli.net/2025/06/20/cDETJGYm8SMtopv.png)

## ssh 登录
![image.png](https://s2.loli.net/2025/06/20/J1GDj4zRupL73fi.png)

居然能登录进去

## 提权
![image.png](https://s2.loli.net/2025/06/20/UZGwuDepol5Jh8H.png)

并没有东西

![image.png](https://s2.loli.net/2025/06/20/hoaOq5pCrGKEx4P.png)

找到 flag

![image.png](https://s2.loli.net/2025/06/20/2k6D4ryqJILbxE5.png)

也是没用东西

感觉可能是从/home 目录下的 zip 下手
![image.png](https://s2.loli.net/2025/06/20/uzFbV59t1wifPOk.png)

解压后发现了类似备份文件的东西

![image.png](https://s2.loli.net/2025/06/20/qvywpiTurIFK6Mn.png)


![image.png](https://s2.loli.net/2025/06/20/IlGKytDjoeH8R59.png)

看了 wp。使用了脚本去破解密码。需要用到提取 keepass 数据库中的密码

破解出密码。但感觉是个乱码
![image.png](https://s2.loli.net/2025/06/20/mlc6BVOskrwJDTd.png)

google 搜一下，是个菜名

![image.png](https://s2.loli.net/2025/06/20/vH6LXy5nBCcaPkh.png)

得出了加密的密码是 rødgrød med fløde。用这个去解密 keepass 数据库

需要用到 keepassx
```
apt install keepassx

keepassxc passcodes.kdbx
```

![image.png](https://s2.loli.net/2025/06/20/Yb3wqIo4cTuERip.png)

![image.png](https://s2.loli.net/2025/06/20/uPJ3HO8KnXtQNGv.png)


找到一个 ssh 的秘钥，把他做成秘钥
![image.png](https://s2.loli.net/2025/06/20/iW4bB83UwlCOfVy.png)

```
puttygen 1.txt -O private-openssh -o id_rsa

将1.txt转换成私钥

ssh root@10.10.11.227 -i id_rsa
用秘钥登录
```

root 登录成功
![image.png](https://s2.loli.net/2025/06/20/bFaG5PeAKX3zlZT.png)

![image.png](https://s2.loli.net/2025/06/20/h6IXqnCUZ8DdRiO.png)
