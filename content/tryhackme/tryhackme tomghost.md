## nmap扫描和字典爆破
发现一个8080端口
## 漏洞利用
搜索有没有apache tomcat 9.0.30版本的漏洞

## ghostcat
在msf中利用这个漏洞 出现了账户密码 刚好开放了ssh
的端口 登录进去看看

![image.png](https://s2.loli.net/2025/05/06/zfKwOUCisdhM9v1.png)

## 提权
登录成功 

发现有一个gpg和asc
[[anonforce教程]]
需要用到john破解

```
第一步
gpg2john tryhackme.asc >  /root/test/hash
导出asc转化为john能识别的格式
```

![image.png](https://s2.loli.net/2025/05/06/cM3TdJjAPX1NS5K.png)


```
第二步
john hash -wordlist=/root/rockyou.txt
破解
```

![image.png](https://s2.loli.net/2025/05/06/WGK4NXRLv2PbkMq.png)

破解出密码 然后导入密钥

```
第三步
导入密钥
gpg --import name.asc
```

![image.png](https://s2.loli.net/2025/05/06/hcXpgzb21N9xQBI.png)

第四步

```
gpg解密 用刚才的密码
gpg -d credential.pgp 
```

![image.png](https://s2.loli.net/2025/05/06/g1KroBAtEXzUejN.png)

获得另一个用户的密码

ssh登录一下

## ssh登录 提权
![image.png](https://s2.loli.net/2025/05/06/oO56IdHAWKDlMyZ.png)


![image.png](https://s2.loli.net/2025/05/06/J8hbaQRSlrKHknB.png)

第一个flag

用zip提权 gtfbin中有现成的命令

![image.png](https://s2.loli.net/2025/05/06/1JreghGkRSuBdDI.png)
