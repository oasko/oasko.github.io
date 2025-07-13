## 第一 Forensics - Analyse the PCAP

我们可以发现上传反弹shell的界面在/development

![image.png](https://s2.loli.net/2025/04/20/pEft2YMZLRjxF8V.png)

点击追踪流就能看到反弹shell的具体命令

![image.png](https://s2.loli.net/2025/04/20/7b84EhAkI1RewVc.png)


攻击者是如何建立持久性的？

要注意这种广播寻呼之后的报文 
![image.png](https://s2.loli.net/2025/04/20/WPE7Dv9Ixh3oUCt.png)

在120个中发现了攻击者具体的操作 也找到了密码

![image.png](https://s2.loli.net/2025/04/20/4h8xZqTjzc5wln1.png)


发现攻击者拉取了一个github中的ssh后门


![image.png](https://s2.loli.net/2025/04/20/Czg4YPEBfd5hVDn.png)

因为我们是用james登录的 我们就剩下四个账户


![image.png](https://s2.loli.net/2025/04/20/jSrctIFqUv976xa.png)


## 第二 Research - Analyse the code


我们去github中查看ssh后门的源码 就能找到初始hash值和硬编码源

![image.png](https://s2.loli.net/2025/04/20/92o3F48rpuxzYaX.png)

![image.png](https://s2.loli.net/2025/04/20/f7yI4nSkcCRiQNH.png)


攻击者的hash
![image.png](https://s2.loli.net/2025/04/20/E4Herzow98nyMUI.png)

这里要想破解出密码 需要用到hashcat工具 需要提供hash和硬编码源 这两个参数 因为是加盐的hash值
 
```
hashcat -m 1710 -a 0 hash.txt rockyou.txt
```

![image.png](https://s2.loli.net/2025/04/20/V2QSdMyhwYOZ4ec.png)

破解成功

## 第三 Attack - Get back in!

用ssh登录 需要加-oHostKeyAlgorithms=+ssh-rsa参数 -p指定端口

![image.png](https://s2.loli.net/2025/04/20/nMbZaluFWYKUPrm.png)

攻击者一般会留一个方便他们操作的提权脚本还是啥 在james目录中 一个隐藏文件
![image.png](https://s2.loli.net/2025/04/20/CTI4LBwlMtEzJdY.png)
