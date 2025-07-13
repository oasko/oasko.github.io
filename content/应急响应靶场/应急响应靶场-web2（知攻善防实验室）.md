![ip1](/images/应急响应web2/title.png)

打开靶场发现桌面有个phpstudy 点击进去 发现是apache的web服务 找到日志文件翻一下 能找到第一个ip


![ip1](/images/应急响应web2/ip1.png)

发现一个ip 再往后会发现这个ip上传了一个system.php进去 现在要找第二个ip

跑了ds 直接用powershell来提取事件的具体消息

```
提取所有网络登录（类型3）和远程桌面登录（类型10）的记录：

Get-WinEvent -FilterHashtable @{LogName='Security';ID=4624} |  
Where-Object {$_.Properties[8].Value -in @(3,10)}  
```

```
Get-WinEvent -FilterHashtable @{LogName='Security';ID=4624} |  
Where-Object {$_.Properties[8].Value -in @(3,10)} |  
Select-Object TimeCreated,  
    @{Name='UserName'; Expression={$_.Properties[5].Value}},  
    @{Name='Domain'; Expression={$_.Properties[6].Value}},  
    @{Name='SourceIP'; Expression={$_.Properties[18].Value}},  
    @{Name='LogonType'; Expression={$_.Properties[8].Value}},  
    @{Name='ProcessName'; Expression={$_.Properties[9].Value}},  
    @{Name='AuthenticationPackage'; Expression={$_.Properties[10].Value}},  
    @{Name='LogonGUID'; Expression={$_.Properties[11].Value}} |  
Format-Table -AutoSize
```

找这两个命令能找到疑似第二个ip 看日志会发现这两个ip之间有点吻合 一个上传webshell 一个负责连接

![ip2](/images/应急响应web2/powershell.png)

![ip2](/images/应急响应web2/ip2.png)


伪qq号 在c盘能翻到腾讯的文件夹 里面有一串数字

![ip2](/images/应急响应web2/qq.png)

ok 破案了 应该是攻击者搞了个frp用来内网穿透的 进去寻找配置文件 可以找到服务器地址和端口

最后是问如何攻击方式 首先排除web攻击 因为上传了个文件之后就没继续了 看日志也没看出什么攻击

数据库攻击也不是 最后看到了ftp的日志文件 发现是暴力破解 是ftp攻击


![ip2](/images/应急响应web2/ftp.png)

用户 我们可以在lusrmgr.msc 来查看用户和组


![ip1](/images/应急响应web2/user.png)