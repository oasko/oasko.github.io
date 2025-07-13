---
title: 应急响应靶场 linux 2（知攻善防实验室）
date: 2025-02-22 19:50:39
tags: linux 靶场
top_img: transparent
---
现在是第二个应急响应靶场 linux 2 是一个命令行界面的centos界面。

登录账户是root 密码是Inch@957821.(记住后面有一个点)

登录进去之后 不太方便操作 我们就ssh连接自己的虚拟机会比较方便一点

进去之后会发现一个题解的脚本和一个pcap数据包 我是用xftp下载到kali里面

打开完题解脚本就正式开搞

## 第一问 攻击者ip
这个就要打开数据包来检查一下 会发现有个ip一直在给一个ip发送php文件

![php](/images/应急响应靶场-linux-2（知攻善防实验室）/2php.png)

分别是index.php和version2.php这两个文件 发送这些的源地址是192.168.20.1

攻击者就是192.168.20.1

## 第二问 攻击者修改的管理密码
一开始 我也是神经病 就一直在wireshark里面看有啥线索吗 结果毫无发现。只能退回去到centos中找

第一个就是看看history 绝对有点东西在里面

![bt](/images/应急响应靶场-linux-2（知攻善防实验室）/bt.png)

![bt](/images/应急响应靶场-linux-2（知攻善防实验室）/api1.png)

找到一些东西 但一开始没看到bt 注意力都在第二张图这里 其实和第二题没多大关系

接着我们找找进程

```
ps -aux
```

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/mysql.png)

在里面找到了个mysql进程 这下子好玩了 但此时又有个问题 不知道mysql的账户密码

在搜索的时候得知config.inc.php 里面就有mysql的账户密码！

输入命令来找找这个文件在哪

```
find / -name config.inc.php
```

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/soumingzi.png)

一个一个检查 总会有一个是的 可以先看一下backup那一个备份的文件

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/smima.png)

知道了账户和密码 账户是kaoshi 密码就是上面那一串

知道账户密码就开始登录

```
mysql -ukaoshi -p密码
```

进去之后来查询一下总共有哪些数据库 有一个数据库名字叫kaoshi 我们优先访问它 查一下他里面有什么数据吗

```
show databases;

use kaoshi;

select tables;

select * from x2_user;(来查看有重要的值可以查询)

select username,serpassword from x2_user;
```
![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/mysql1.5.png)

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/tables.png)

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/mysql2.png)

密码查出来了 第一个就是 但这个值看起来怪怪的 疑似md5 就拉到网站里面转换一下

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/mimamd.png)

密码是Network@2020

## 第三问 第一次提交的webshell的url
这个在数据包中就能看到 找到post包就行

![mysql](/images/应急响应靶场-linux-2（知攻善防实验室）/pa0.png)

url就是index.php/user-app-register

## 第四问 webshell的连接密码
其实在上一题的截图中就能找到 Network2020就是密码

代码的第一句就是Network2020=@ini_set("display_errors", "0");

是所有webshell客户端链接PHP类WebShell都有的一种代码 

通常是为了增强脚本的安全性，确保攻击者在执行恶意操作时不会暴露错误信息。此设置经常与其他隐藏活动的代码（如禁止文件上传限制、隐藏访问标记等）配合使用，目的是让 WebShell 在目标服务器上需要很长时间，而不存在被管理员感知。



## 第五问 后续上传的php是啥
![php](/images/应急响应靶场-linux-2（知攻善防实验室）/2php.png)

第二个是version2.php

## flag1
第一个flag是在wireshark中找到的 

![flag](/images/应急响应靶场-linux-2（知攻善防实验室）/flag0.1.png)

![flag](/images/应急响应靶场-linux-2（知攻善防实验室）/flag1.png)


## flag2 & flag3
我们在看history的时候就有碰到flag3

![env](/images/应急响应靶场-linux-2（知攻善防实验室）/env.png)

我们直接输入env就能看到flag2

linux2靶机完成 