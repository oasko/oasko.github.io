# SMB
SMB - 服务器消息块协议 - 是一种客户端-服务器通信协议，用于共享对网络上文件、打印机、串行端口和其他资源的访问

SMB 协议称为响应请求协议，这意味着它在客户端和服务器之间传输多条消息以建立连接。客户端使用 TCP/IP 连接到服务器（实际上是 NetBIOS over RFC1001 和 RFC1002 中指定的 TCP/IP）、NetBEUI 或 IPX/SPX。

samba协议在unix中运行

## smb枚举
### Enum4Linux 
用于枚举 Windows 和 Linux 系统上的 SMB 共享的工具
[[linux工具#Enum4Linux ]]

## smbclient
远程访问 SMB 共享

```
-U [name] ：指定用户

-p [port] ： 指定端口
```

```
smbclient //[IP]/[SHARE]
```

匿名登录

```
smbclient //10.10.73.8/profiles -U Anonymous -p 445
```

![image.png](https://s2.loli.net/2025/04/23/MGXD2Kq8z4bNhUk.png)


# Telnet