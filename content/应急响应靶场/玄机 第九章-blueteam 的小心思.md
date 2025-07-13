---
UID: 20250602141609
aliases: 
tags:
  - 打靶
  - 应急响应
source: 
cssclasses: 
created: 2025-06-02
---

## 第一问
![image.png](https://s2.loli.net/2025/06/02/Ia2nr71AUY9pcXl.png)

![image.png](https://s2.loli.net/2025/06/02/7DanmJKlORuU9WQ.png)

导出到电脑，可以看到账户密码
![image.png](https://s2.loli.net/2025/06/02/zjHrteAiVXQfMwS.png)



## 第二问
![image.png](https://s2.loli.net/2025/06/02/WSa4J7ybjwLmhPd.png)

在查看 apache 的access 日志中发现的。因为是攻击者是在 plugins 上传的木马，就有可能是在pluginmgr（插件管理）中上传


## 第三问
![image.png](https://s2.loli.net/2025/06/02/Mpf8ChSXzZWj1FT.png)

```
find / type f -name "*.php" | xargs grep "eval("
```

![image.png](https://s2.loli.net/2025/06/02/u8iPIycCalAOEY5.png)


![image.png](https://s2.loli.net/2025/06/02/dOugR7QV2YmxzcH.png)



## 第四问
![image.png](https://s2.loli.net/2025/06/02/znectZUmWAXREJK.png)

![image.png](https://s2.loli.net/2025/06/02/ZvhuadFtiwQIrcf.png)


![image.png](https://s2.loli.net/2025/06/02/M3P4ugSBZvIQw1x.png)

![image.png](https://s2.loli.net/2025/06/02/LXc9ia7vGnH81rF.png)


## 第五问
![image.png](https://s2.loli.net/2025/06/02/GUAej3RZhO1DTWK.png)



## 第六问
![image.png](https://s2.loli.net/2025/06/02/QGj2mn7OeMWJIg3.png)

分析发现是创建木马的文件，应该这个就是持久性的后门连接

![image.png](https://s2.loli.net/2025/06/02/6bARoPpYhUCGFu8.png)

## 第六问
root 用户的持久性。应该要在 root 的目录中
![image.png](https://s2.loli.net/2025/06/02/RJDGQLc5YSF6xdm.png)


![image.png](https://s2.loli.net/2025/06/02/cuCU2kW3v1oBGPf.png)

里面还真有木马

![image.png](https://s2.loli.net/2025/06/02/2TVws9FSbe7xfWg.png)


## 数据库

![image.png](https://s2.loli.net/2025/06/02/sryFbvN9p3hoqw1.png)


![image.png](https://s2.loli.net/2025/06/02/hS7GRCuVjW1tLi4.png)

找到一个用户

## 第七问

![image.png](https://s2.loli.net/2025/06/02/ZglN5wSMBjVTk1e.png)

得知 mysql 的存储是在/usr/lib/mysql 中。蓝色的四个就是我们数据库中的文件，

因为除了 jp 的数据库，我们都能访问。唯独 jp 不行。那应该就是 jp 了

![image.png](https://s2.loli.net/2025/06/02/4kRbx1HdcMV3hXB.png)


## 第九问

```
sudo -l

find / -user root -perm -4000 2>/dev/null
```


![image.png](https://s2.loli.net/2025/06/02/9SXoeRZcdOjp5uz.png)


![image.png](https://s2.loli.net/2025/06/02/utITy8Y1nisvwMp.png)

sodo 有很大问题

为什么会分析“/etc/sudoers”呢？

权限配置：了解哪些用户或用户组被授予了 sudo 权限。如果配置不当，可能允许普通用户以 root 权限运行命令，从而导致安全问题。




##注意
为什么会分析“/etc/sudoers”呢？

权限配置：了解哪些用户或用户组被授予了 sudo 权限。如果配置不当，可能允许普通用户以 root 权限运行命令，从而导致安全问题。

1、”~/. bashrc“：用于 Bash Shell，会在每次打开新的终端或登录 Shell 时执行。
2、”/. bash_profile“或”/. profile“：这些文件在用户登录时执行。
3、”~/. zshrc“：用于 Zsh Shell，与”~/. bashrc“类似。
4、”/etc/profile“：为所有用户提供的系统级别的配置文件，
5、”/etc/bash. bashrc“：为所有用户提供的系统级别的配置文件，Bash Shell 专用。

没有搜索仔细，根目录有个流量包。持久化后门不太懂。主要是确实不知道文件的功能