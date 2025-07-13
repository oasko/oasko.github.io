## 系统工具:
###  **Task Scheduler**

^f213c0

###  **Event Viewer**

^534268

###  **Shared Folders**

###  **Local Users and Groups**

###  **Performance**

### **Device Manager**
### **lusrmgr.msc** 
 本地用户和组
### **compmgmt**
 计算机管理 **老版才行

### mscconfig 
 系统配置
 
### perfmon 
 性能监视器

### Msinfo32.exe  
 Microsoft 系统信息
 
### resmon 资源监视器

 ### systeminfo 系统信息 

^d31370

 但在win11就闪退了 不知道为sha

## CMD命令

### set命令

^6e3ae6

### ver命令

^887c12

### ipconfig命令

^0cb877

顾名思义 ipconfig命令 或者后面加个/all。
### tracert
代表跟踪路由。该命令tracert target_name跟踪到达目标所经过的网络路
### nslook

###  netstat
netstat命令是Windows操作系统中的一个网络工具，用于显示当前计算机与网络之间的连接状态、协议、端口号等信息。 ^109b81

常用-ano参数

### cd命令

^52086c

### dir
可以用来列举文件夹中的文件 加/s /b能查找文件

^e4af7b



### chkdsk
检查文件系统和磁盘卷是否存在错误和坏扇区。

### driverquery
显示已安装的设备驱动程序列表。显示已安装的设备驱动程序列表。

### sfc /scannow
扫描系统文件是否有损坏，并在可能的情况下修复它们。


### shutdown

^4f4efe


## powershell

### **常见 CMD 命令**

|**新闻**|**作用**|
|---|---|
|`dir`|显示当前目录下的文件和文件夹|
|`cd 目录名`|进入目录，例如`cd C:\Users`|
|`cd ..`|返回上一级目录|
|`mkdir 目录名`|该网站，例如`mkdir test`|
|`rmdir 目录名 /s /q`|删除文件夹（`/s`删除子目录，`/q`静默模式）|
|`del 文件名`|删除文件，例如`del test.txt`|
|`copy`|复制文件，例如`copy a.txt d:\backup\`|
|`move`|移动文件，例如`move a.txt d:\backup\`|
|`ipconfig`|查看本机 IP 地址信息|
|`ping`|网络测试影响性，例如`ping www.baidu.com`|
|`tasklist`|查看当前运行的进程|
|`taskkill /F /IM 进程名`|结束进程，例如`taskkill /F /IM notepad.exe`|

- **`net user`**：用于管理本地用户账户。
    
    - 查看所有本地用户：
    
        net user
        
    - 查看特定用户的详细信息：
    
        net user <用户名>

        
        例如，查看用户 "John" 的信息：
       
        net user John

         ^2f1f3e
    - 创建新用户：

        net user <用户名> <密码> /add

        eg:
        创建用户 "John" 并设置密码为 "Password123"：
        
        net user John Password123 /add
        
    - 删除用户：
    - 
        net user <用户名> /delete
        
        eg
        删除用户 "John"：
    
        net user John /delete
        
    - 修改用户密码：
    
        net user <用户名> <新密码>

        eg
        将用户 "John" 的密码修改为 "NewPassword123"：

        net user John NewPassword123
     

### **常见 PowerShell 命令**

| **PowerShell 命令**                   | **作用**                           | **CMD 对应命令**       |
| ----------------------------------- | -------------------------------- | ------------------ |
| `Get-ChildItem`                     | 上市目录内容（类似`ls`）                   | `dir`              |
| `Set-Location`                      | 去目的地                             | `cd`               |
| `New-Item -ItemType File test.txt`  | 该计划文件                            | `echo. > test.txt` |
| `New-Item -ItemType Directory test` | 年份文件                             | `mkdir test`       |
| `Remove-Item`                       | 删除文件或目录，例如`Remove-Item test.txt` | `del`/`rmdir`      |
| `Copy-Item`                         | 复制文件或文件                          | `copy`             |
| `Move-Item`                         | 移动文件或文件夹                         | `move`             |
| `Get-Process`                       | 查看当前运行的进程                        | `tasklist`         |
| `Stop-Process -Name notepad`        | 结束进程                             | `taskkill`         |
| `Get-NetIPAddress`                  | 查看IP信息                           | `ipconfig`         |
| `Test-Connection www.baidu.com`     | 网络测试评价性（类似`ping`）                | `ping`             |

^069cfa


### 文件和文件夹操作

- `Get-ChildItem`：列出指定路径下的文件和文件夹。
     ^2b2102
- `New-Item`：创建新文件或文件夹。
     ^2eb53c
- `Remove-Item`：删除文件或文件夹。
    
- `Copy-Item`：复制文件或文件夹。
    
- `Move-Item`：移动文件或文件夹。
    
- `Rename-Item`：重命名文件或文件夹。
    

### 文件内容操作

- `Get-Content`：读取文件内容。
    
- `Set-Content`：写入内容到文件。
    
- `Add-Content`：追加内容到文件。
    
- `Clear-Content`：清空文件内容。

- Get-FileHash命令:看文件的hash值 ^e4184c

### 进程和任务管理

- `Get-Process`：获取当前运行的进程信息。
    
- `Start-Process`：启动新进程。
    
- `Stop-Process`：终止进程。
    
- `Get-Job`：获取后台作业信息。
    
- `Start-Job`：启动后台作业。
    
- `Stop-Job`：停止后台作业。
    
- `Receive-Job`：获取后台作业的输出。
    

### 网络操作

- `Get-NetIPConfiguration`：获取网络配置信息。
     ^402125
- `Test-Connection`：测试网络连接（类似 `ping`）。
    
- `Invoke-WebRequest`：发送 HTTP 请求。
    
- `Get-DnsClientCache`：获取 DNS 缓存信息。

- `Get-NetTCPConnection` 显示计算机上的 TCP 连接状态， ^cee3c2

### 用户和权限管理

- `Get-User`：获取用户信息。
     ^1c3585
- `New-User`：创建新用户。
    
- `Remove-User`：删除用户。
    
- `Get-Role`：获取角色信息。
    
- `New-Role`：创建新角色。

- `get-localuser` 获取本地用户信息 ^709495

### 注册表操作

- `Get-Item`：获取注册表项。
    
- `Set-Item`：设置注册表项。
    
- `New-Item`：创建新注册表项。
    
- `Remove-Item`：删除注册表项。
    

### PowerShell 模块管理

- `Get-Module`：获取已加载的模块。
    
- `Import-Module`：导入模块。
    
- `Remove-Module`：卸载模块。
    

### 脚本和函数

- `Invoke-Expression`：执行字符串形式的表达式。
    
- `Invoke-Command`：在本地或远程计算机上运行命令。
    
- `Get-Help`：获取命令的帮助信息。
    

### 其他常用命令

- `Get-Date`：获取当前日期和时间。
    
- `Get-Random`：生成随机数。
    
- `Write-Output`：输出信息。
    
- `Read-Host`：从控制台读取输入。
    
- `Clear-Host`：清除控制台屏幕。


**Get-Command** 是 PowerShell 中的一个命令，用于获取系统中可用的命令信息，包括 cmdlet、函数、可执行文件等。它可以帮助用户了解某个命令是否存在、它的类型、定义以及所在路径等。

以下是 `Get-Command` 的一些常见用法：

1. **查看所有可用命令**：
    Get-Command

    
2. **查看特定命令的详细信息**：
    Get-Command -Name 命令名称
     ^1aa148
3. **查看特定类型的命令**：
    - 查看所有 cmdlet：
        Get-Command -CommandType Cmdlet


Remove-Itemcmdlet也是Remove-Item 是 PowerShell 中的一个 cmdlet，用于删除文件系统中的文件或文件夹。它类似于命令行中的 `del` 或 `rmdir` 命令，但功能更强大且灵活。以下是 `Remove-Item` 的一些常见用法和参数：

Remove-Item [-Path] <string> [-Force] [-Recurse] [-WhatIf] [-Confirm]

- `-Path`：指定要删除的文件或文件夹的路径。可以是绝对路径或相对路径。
    
- `-Force`：强制删除，即使文件或文件夹具有只读属性。
    
- `-Recurse`：递归删除，用于删除文件夹及其所有内容（包括子文件夹和文件）。
    
- `-WhatIf`：显示删除操作将会执行的动作，但不实际执行删除。
    
- `-Confirm`：在删除之前提示确认。
    
eg:
1. **删除单个文件**：

    Remove-Item -Path "C:\Users\John\Documents\example.txt"

2. **删除多个文件**：

    Remove-Item -Path "C:\Users\John\Documents\*.tmp"

3. **删除文件夹及其内容**：

    Remove-Item -Path "C:\Users\John\Documents\OldFolder" -Recurse ^a78b0f
