
## nmap
Nmap _Network mapper_ 开源的网络探测和安全审核的工具，该工具设计主要是为了分析网络主机、服务和弱点。能力包括：

- 端口扫描。包括 TCP、UDP、SYN 还有其他。
- 服务探测
- 操作系统指纹
- 脚本引擎：Nmap 提供了一个叫做 NSE（Nmap Scripting Engine，Nmap 脚本引擎），能够执行用户自定义的脚本。 ^e7b869
- 弱点探测
- 高性能和伸缩性：高效扫描大型网络
- 灵活的输出：Txt、XML 等
- 跨平台

| 扫描类型（Scan Type）          | 示例命令（Example Command）                       |
| ------------------------ | ------------------------------------------- |
| ARP扫描（ARP Scan）          | `sudo nmap -PR -sn MACHINE_IP/24`           |
| ICMP回声扫描（ICMP Echo Scan） | `sudo nmap -PE -sn MACHINE_IP/24`           |
| ICMP时间戳扫描（Timestamp）     | `sudo nmap -PP -sn MACHINE_IP/24`           |
| ICMP地址掩码扫描（Address Mask） | `sudo nmap -PM -sn MACHINE_IP/24`           |
| TCP SYN Ping扫描           | `sudo nmap -PS22,80,443 -sn MACHINE_IP/30`  |
| TCP ACK Ping扫描           | `sudo nmap -PA22,80,443 -sn MACHINE_IP/30`  |
| UDP Ping扫描               | `sudo nmap -PU53,161,162 -sn MACHINE_IP/30` |

### 端口扫描基础
端口扫描基础的直接链接

端口扫描是 Nmap 的核心功能。

传统端口扫描只会列出端口是开放或者关闭的，Nmap 有更多的信息粒度，分为 6 种：

1. open

2. closed
关闭状态也是有响应的（接受 Nmap 的探测报文并响应）但没有应用程序监听。

3. filters  
可能被防火墙过滤掉了，无法确定该端口是否开放。

4. unfiltered

5. open|filtered
无法确定是 open 还是 filtered

6. closed|filtered
无法确定是 closed 还是 filtered

### 用法
- IP 范围使用-：如果要扫描从 192.168.0.1 到 192.168.0.10 的所有 IP 地址，就写成192.168.0.1-10

- IP 子网使用/：如果要扫描子网，可以将其表示为192.168.0.1/24，这相当于192.168.0.0-255

- 主机名：您还可以通过主机名指定目标，例如，example.thm

### tcp扫描

nmap -sT
连接扫描  (扫描需要完成tcp三次握手)


| 描述             |     | wireshark 语法                                  |                |
| -------------- | --- | --------------------------------------------- | -------------- |
| 全局扫描           |     | tcp udp                                       |                |
| 只有syn flag     |     | tcp .flags.syn==1                             | tcp.flags == 2 |
| 只有ack flag     |     | tcp.flags.ack==1                              | tcp.flags ==16 |
| ack和ack的flag   |     | (tcp.flags.ack== 1) and (tcp .flags.syn== 1)  | tcp.flags ==16 |
| 只有rst的flag     |     | tcp.flags.reset == 1                          | tcp.flags == 1 |
| 只有rst和ack的flag |     | tcp.flags.ack== 1) and (tcp .flags.reset== 1) | tcp.flags == 1 |
| 只有fin标志        |     | tcp.flags.fin == 1                            | tcp.flags == 1 |

### syn扫描

^54e8dc

nmap -sS
SYN扫描(仅完成第一步) ^077f81


### udp扫描
nmap -sU
UDP扫描 (UDP扫描) ^713e2e

 ### 常用参数
 #### 1. -sV ^9702c1
 - 版本检测、使用版本检测扫描之前需要先扫描开放了哪些端口 ^389863
 #### 2. -PN
- 参数可以绕过PING命令，用于远程主机是存活在网络上的，目标主机禁ping的情况下
 #### 3 . -A
- 使用所有高级扫描选项；全端口扫描
 #### 4 . -O
- 启用操作系统检测
 #### 5 . -T
- 设置扫描速度，1-6 

 #### 6 . -sP ping 扫描
- 主机存活性扫描，arp直连方式 ^94d7c2
-  -sL 列表扫描
仅仅列出指定网络上的每台主机，不发送任何报文到目标主机，也并不确定主机是否存活，所以扫描速度很快。默认情况下，Nmap 会对主机进行反向域名解析以获取他们的名字。
- 此扫描仅列出要扫描的目标，而不会实际扫描它们

8. -sn 
- 目的是发现活动主机而不尝试发现其上运行的服务。

8. -F 代表快速模式，扫描 100 个最常见的端口

9. -p[range] 允许您指定要扫描的端口范围。例如，-p10-1024扫描从端口 10 到端口 1024，

10. -p 25 扫描 1 到 25 之间的所有端口。

11. -p- 扫描所有端口和相当于-p1-65535

12. -sV 启用版本检测。这非常方便您用更少的按键收集有关目标的更多信息。发现了几个开放端口，并想知道哪些服务正在监听它们
 ^8da186
13.  -A 有-O，-sV并且还有更多功能，此选项可实现操作系统检测、版本扫描和跟踪路由等功能。

14. -Pn 能将所有主机视为在线并对每个主机进行端口扫描，包括在主机发现阶段未响应的主机。

15. -T 可以按名称或编号选择计时模板。举例: -T0(或-T 0) 或-T paranoid(paranoid就是计时模板的名称)以选择最慢的计时。

16. -iL list_of_hosts.txt 提供一个文件作为目标列表的输入
### 高端点参数
更好地控制Nmap如何通过给定端口发现实时主机（例如-PS[portlist]、-PA[portlist]），
-PU[portlist]以进行TCP SYN、TCP ACK 和UDP发现。

并行探测的数量可以通过--min-parallelism (numprobes)和 来控制--max-parallelism (numprobes)。这些选项可用于设置主机组中同时活动的 TCP 和 UDP 端口探测数量的最小值和最大值。

类似有用的选项是--min-rate (number)和--max-rate (number)。顾名思义，它们可以控制nmap发送数据包的最小和最大速率。

--host-timeout (time)。此选项指定您愿意等待的最大时间，适用于速度较慢的主机或网络连接较慢的主机。

- -v 启用详细输出

- -d 使用调试级别输出 最高级别是-d9 您可以通过添加一个或多个“d”或直接指定调试级别来提高调试级别

- oN (filename)- 正常输出

- oX (filename)- XML输出

- oG (filename)- -able 输出 (对和grep有用)grepawk

- oA (basename)- 以所有主要格式输出

 --script=vuln 扫描漏洞

##  ryptsetup
- 用于管理加密设备的命令行工具。
### 常见参数
- 1.**open**：打开一个加密设备。
 
    cryptsetup open (device) (name)

    - `(device)`：加密设备的路径。
    
    - `(name)`：解密后的设备映射器名称。
    
- 2.**--type**：指定加密设备的类型。
  
    cryptsetup --type (type) open (device) (name)

    
    - `(type)`：加密设备的类型，如`luks`、`plain`等。
    
- 3.**luksOpen**：专门用于LUKS加密设备的打开命令。

    cryptsetup luksOpen (device) (name)


## SearchMap

## tcpdump
Tcpdump工具及其libpcap库是用 C 和 C++ 编写的

tcpdump的作用
- 捕获数据包并保存到文件

- 对捕获的数据包设置过滤器

- 控制捕获的数据包的显示方式

下表提供了我们所涵盖的命令行选项的摘要。

| 命令                     | 解释                          |
| ---------------------- | --------------------------- |
| `tcpdump -i INTERFACE` | 捕获特定网络接口上的数据包               |
| `tcpdump -w FILE`      | 将捕获的数据包写入文件                 |
| `tcpdump -r FILE`      | 从文件读取捕获的数据包                 |
| `tcpdump -c COUNT`     | 捕获特定数量的数据包                  |
| `tcpdump -n`           | 不解析 IP 地址                   |
| `tcpdump -nn`          | 不解析 IP 地址，也不解析协议号           |
| `tcpdump -v`           | `-vv`详细显示；详细程度可以通过和增加`-vvv` |

### 举例
设置捕获数据包的大小
- -s 设置捕获数据包的大小
```
tcpdump -i eth0 -s 0
# 0表示完整的数据包
```

- -D 列出要捕获的接口
```
tcpdump -D
```

- -l 将捕获内容通过管道传输到grep
使我们能够快速检查捕获并 grep 我们怀疑可能存在的关键字或格式
```
sudo tcpdump -Ar http.cap -l | grep 'mailto:*'
# -l将输出传递给`grep`并查找短语的任何实例`mailto:*`
```

#### 过滤表达式

指定端口 主机 指定源地址和目标地址
1. 指定端口
- port
```
tcpdump -i eth0 port 80
```

2. 指定主机
- host
```
tcpdump -i eth0 host 192.168.10.1
```

3. 指定源地址和目标地址
- src dst
```
tcpdump -i eth0 src host 192.168.10.1

tcpdump -i eth0 dst host 192.168.10.1
```

4. 指定协议
- tcpdump 协议名
```
tcpdump -i eth0 arp
#捕获所有arp协议的数据包

tcpdump -i eth0 icmp
#捕获所有icmp协议的数据包

tcpdump -i eth0 tcp
#捕获所有tcp协议的数据包
```
5. less/greater()


6. 指定端口范围
- portrange
```
sudo tcpdump -i eth0 portrange 0-1024
```


#### tcp
可以使用tcp[tcpflags]来引用TCP标志字段。以下TCP标志可供比较：

- tcp-syn TCP SYN（同步）

- tcp-ack TCP ACK（确认）

- tcp-fin TCP FIN（完成）

- tcp-rst TCP RST（重置）

- tcp-push TCP推送

基于上述内容，我们可以写出：

- tcpdump "tcp[tcpflags] == tcp-syn"捕获仅设置了 SYN（同步）标志但所有其他标志均未设置的TCP数据包。

- tcpdump "tcp[tcpflags] & tcp-syn != 0"捕获至少设置了 SYN（同步）标志的TCP数据包。

- tcpdump "tcp[tcpflags] & (tcp-syn|tcp-ack) != 0"捕获至少设置了SYN（同步）或ACK（确认）标志的TCP数据包。

-  [ S] 表示客户端发送的 SYN 包。

-  [S.] 表示服务器发送的 SYN-ACK 包。

-  [`.`] 是客户端发送的 ACK 包，完成三次握手。

##  msf

###  **msfconsole**

`msfconsole` 是 Metasploit Framework 的主要命令行界面，提供了一个交互式 shell，用于访问和操作各种模块。

- **启动 msfconsole**：
    
    ```bash
    msfconsole
    ```

#### payload

#### auxiliary

^7fc635

### **常用命令**：
#### `search` 命令

`search` 命令用于在 Metasploit Framework 中搜索特定的模块。你可以根据模块的名称、描述、作者、平台等条件进行搜索。

- **基本语法**：
    
    ```bash
    search [options] [keywords]
    ```


- **常用选项**：
    
    - `app`：搜索客户端或服务器攻击的模块。
        
    - `author`：搜索特定作者编写的模块。
        
    - `bid`：搜索具有匹配的 Bugtraq ID 的模块。
        
    - `cve`：搜索具有匹配 CVE ID 的模块。
        
    - `edb`：搜索具有匹配 Exploit-DB ID 的模块。
        
    - `name`：搜索具有匹配描述性名称的模块。
         ^8d544d
    - `platform`：搜索影响特定平台的模块。
        
    - `ref`：搜索具有匹配参考的模块。
        
    - `type`：搜索特定类型的模块（如 `exploit`、`auxiliary` 或 `post`）。
        
- **示例**：
    
    - 搜索所有 Windows 平台的漏洞利用模块：
        
        ```bash
        search type:exploit platform:windows
        ```
        
    - 搜索特定 CVE ID 的模块：
        
        ```bash
        search cve:2024-1234
        ```
        
    - 搜索特定作者编写的模块：
        
        ```bash
        search author:aaron
        ```
        
    - 多条件搜索：
        
        ```bash
        search name:mysql type:aux author:aaron
        ```
    - `search [term]`：搜索特定的漏洞利用模块或辅助模块。例如：
        ```bash
        search type:exploit apache
        ```
    
#### use命令
- **基本语法**：
    ```bash
    use [module_path]
    ```
     ^328906
- **示例**：
    
    - 选择并加载一个特定的漏洞利用模块：
    
        ```bash
        use exploit/windows/smb/ms08_067_netapi
        ```
        
    - 选择并加载一个辅助模块：
    
        ```bash
        use auxiliary/scanner/http/dir_scanner
        ```
        
    - 选择并加载一个后渗透模块：
    
        ```bash
        use post/windows/gather/enum_logged_on_users
        ```


- **查看模块信息**：
    
    - 查看当前模块的详细信息：
        
        ```bash
        info
        ```
        
    - 查看当前模块的配置选项：
    
        ```bash
        show options
        ```
        
    - 查看当前模块的高级选项：
        
        ```bash
        show advanced
        ```
	-  显示当前模块可用的载荷:
		```
		show payloads
		```

#### use命令
- **基本语法**：
    ```bash
    use [module_path]
    ```

    - `set [option] [value]`：设置模块的参数：
        ```bash
        set RHOSTS 192.168.1.100
        set RPORT 445
        ```
        
    - `set payload [payload_name]`：设置载荷：
        
        ```bash
        set payload windows/meterpreter/reverse_tcp
        ```
        
    - `exploit` 或 `run`：执行攻击。

### **Meterpreter命令**

^3d6d9f

核心命令将有助于导航和与目标系统交互。以下是一些最常用的命令。Meterpreter 会话启动后，请记住运行 help 命令检查所有可用命令。

#### 核心命令

- `background`：当前会话的背景
- `exit`：终止Meterpreter会话
- `guid`：获取会话 GUID（全局唯一标识符）  
    
- `help`：显示帮助菜单
- `info`：显示有关 Post 模块的信息
- `irb`：在当前会话中打开交互式 Ruby shell
- `load`：加载一个或多个Meterpreter扩展
- `migrate`：允许您将Meterpreter迁移到另一个进程
- `run`：执行Meterpreter脚本或 Post 模块
- `sessions`：快速切换到另一个会话

#### 文件系统命令

- `cd`：将更改目录
- `ls`：将列出当前目录中的文件（dir 也可以）
- `pwd`：打印当前工作目录
- `edit`：将允许您编辑文件
- `cat`：将在屏幕上显示文件的内容
- `rm`：将删除指定文件
- `search`：将搜索文件
- `upload`：将上传文件或目录
- `download`：将下载文件或目录

#### 网络命令

- `arp`：显示主机ARP（地址解析协议）缓存
- `ifconfig`：显示目标系统上可用的网络接口  
    
- `netstat`：显示网络连接
- `portfwd`：将本地端口转发到远程服务
- `route`：允许您查看和修改路由表

#### 系统命令

- `clearev`：清除事件日志
- `execute`：执行命令
- `getpid`：显示当前进程标识符
- `getuid`：向用户显示Meterpreter正在以以下身份运行
- `kill`：终止进程
- `pkill`：通过名称终止进程
- `ps`：列出正在运行的进程
- `reboot`：重新启动远程计算机
- `shell`：进入系统命令 shell
- `shutdown`：关闭远程计算机
- `sysinfo`：获取有关远程系统的信息，例如操作系统

#### 其他命令（这些命令将在帮助菜单中的不同菜单类别下列出）

- `idletime`：返回远程用户空闲的秒数
- `keyscan_dump`：转储击键缓冲区
- `keyscan_start`：开始捕获击键
- `keyscan_stop`：停止捕获按键
- `screenshare`：允许您实时观看远程用户的桌面
- `screenshot`：抓取交互式桌面的屏幕截图
- `record_mic`：从默认麦克风录制 X 秒的音频
- `webcam_chat`：开始视频聊天
- `webcam_list`：列出网络摄像头
- `webcam_snap`：从指定的网络摄像头拍摄快照
- `webcam_stream`：播放指定网络摄像头的视频流
- `getsystem`：尝试将您的权限提升至本地系统权限
- `hashdump`：转储 SAM 数据库的内容

####  `ps`命令
将列出正在运行的进程。

#### **哈希转储**

`hashdump` 命令将列出 SAM 数据库的内容。SAM（安全帐户管理器）数据库存储 Windows 系统上的用户密码。这些密码以NTLM（新技术 LAN 管理器）格式存储。

#### `search` 命令
可用于查找可能包含有用信息的文件。

### msfvenom

#### **生成参数​**​：

- `-p, --payload`：指定 payload 类型（如 `windows/meterpreter/reverse_tcp`）。
- `-f, --format`：定义输出格式（如 `exe`、`elf`、`raw`、`python`）。
- `-o, --out`：保存生成的 payload 到指定路径（如 `-o shell.exe`）。


## hydra
Hydra 是一款支持暴力破解的工具，它可以用于测试弱密码、验证口令策略等场景。 ^71d08f

```
hydra [options] [protocol]://[host][:port]
```

### 基本参数

- `-l <username>`：指定单个用户名进行破解。

- `-L <file>`：指定包含用户名的文件，用于字典攻击。

- `-p <password>`：指定单个密码进行破解。

- `-P <file>`：指定包含密码的文件，用于字典攻击。

- `-s <port>`：指定目标服务的端口号（非默认端口）。

- `-t <tasks>`：指定同时运行的任务数，默认为16。

- `-vV`：显示详细执行过程。

- `-o <file>`：将结果保存到指定文件。

- `-f`：找到第一对有效的用户名和密码后立即停止。

- `-R`：从上次进度继续破解（适用于中断的任务）。

- `-M <file>`：指定目标列表文件，用于批量破解。


### 示例命令

1. #### **破解 SSH 服务**：

```
    hydra -l root -P /path/to/passwords.txt ssh://192.168.1.100
```
    ^82e021
    
    - `-l root`：指定用户名为 `root`。
        
    - `-P /path/to/passwords.txt`：从密码字典文件中读取密码。
        
    - `ssh://192.168.1.100`：指定目标主机和协议。
 ^495457
 ^39f055 ^d8a8c8

2. #### **破解 FTP 服务**：
    ```bash
    hydra -L /path/to/usernames.txt -P /path/to/passwords.txt ftp://192.168.1.100
    ```
    
    - `-L /path/to/usernames.txt`：从用户名字典文件中读取用户名。
        
    - `-P /path/to/passwords.txt`：从密码字典文件中读取密码。
        
    - `ftp://192.168.1.100`：指定目标主机和协议。


3. #### **破解 HTTP 表单**：

```
    hydra -l admin -P /path/to/passwords.txt 192.168.1.100 http-post-form "/login.php:username=^USER^&password=^PASS^:F=incorrect"
```
     ^ea11bf
    
    - `http-post-form`：指定服务类型为 HTTP 表单。
        
    - `/login.php:username=^USER^&password=^PASS^:F=incorrect`：指定表单路径、参数和错误消息。



4. #### 破解 HTTP 表单认证

假设目标是 `http://localhost/login.php`，表单数据为 `username=^USER^&password=^PASS^&Login=Login`，错误消息为 `Login failed`，密码字典文件为 `rockyou.txt`。

1. ##### **创建表单数据文件**： 创建一个名为 `login-form.txt` 的文件，内容如下：
    `username=^USER^&password=^PASS^&Login=Login`

1. ##### **运行 Hydra**：
    
    ```bash
    hydra -l admin -P rockyou.txt -s http://localhost/login.php -f login-form.txt -m "username=^USER^&password=^PASS^&Login=Login:Login failed" -V
    ```

- `-l admin`：指定用户名为 `admin`。
    
- `-P rockyou.txt`：指定密码字典文件为 `rockyou.txt`。
    
- `-s http://localhost/login.php`：指定目标主机的 URL。
    
- `-f login-form.txt`：指定表单数据文件为 `login-form.txt`。
    
- `-m "username=^USER^&password=^PASS^&Login=Login:Login failed"`：指定表单数据和错误消息。
    
- `-V`：显示详细输出。

 5. HTTP 表单认证（POST 请求） ^ac6c70

假设目标是 `http://localhost/login.php`，表单数据为 `username=^USER^&password=^PASS^`，错误消息为 `Invalid username or password`，密码字典文件为 `rockyou.txt`。

```bash
hydra -l admin -P rockyou.txt -s http://localhost/login.php -m "username=^USER^&password=^PASS^:Invalid username or password" -V
```

^995249

- `-l admin`：指定用户名为 `admin`。
    
- `-P rockyou.txt`：指定密码字典文件为 `rockyou.txt`。
    
- `-s http://localhost/login.php`：指定目标主机的 URL。
    
- `-m "username=^USER^&password=^PASS^:Invalid username or password"`：指定表单数据和错误消息。
    
- `-V`：显示详细输出。



4. #### 破解pop3

```
hydra -L d.txt -P rockyou.txt -s 55007 61.139.2.168 pop3
```

^9335c2



## wpscan

WPScan 是一个专门用于扫描 WordPress 网站安全漏洞的开源工具，广泛用于安全测试和渗透测试。

### **2. WPScan 的基本用法**

WPScan 主要用于扫描 WordPress 站点的漏洞，包括插件、主题、用户等信息。

#### **2.1 扫描 WordPress 站点**


`wpscan --url http://example.com` ^ebcdc0

- `--url` 指定要扫描的目标网站。
### **3. 常用参数**

#### **3.1 用户名枚举**

枚举 WordPress 站点的用户名：

bash

复制编辑

`wpscan --url http://example.com --enumerate u`

- `--enumerate u`：枚举用户。

---

#### **3.2 插件枚举**

1. 枚举 WordPress 站点安装的插件：

`wpscan --url http://example.com --enumerate p`

- `--enumerate p`：枚举插件。

2. 枚举所有插件（包括未激活的）：

`wpscan --url http://example.com --enumerate ap`

- `--enumerate ap`：枚举所有插件。

---

#### **3.3 主题枚举**

1. 枚举 WordPress 站点使用的主题：

`wpscan --url http://example.com --enumerate t`

- `--enumerate t`：枚举主题。

2. 枚举所有主题（包括未激活的）：

`wpscan --url http://example.com --enumerate at`

- `--enumerate at`：枚举所有主题。

---

#### **3.4 WordPress 版本检测**

`wpscan --url http://example.com --wp-version`

- `--wp-version`：检测 WordPress 的版本。

---

#### **3.5 漏洞扫描**

`wpscan --url http://example.com --detection-mode aggressive`

- `--detection-mode aggressive`：进行更深入的漏洞扫描（可能会被拦截）。

---

#### **3.6 账号密码暴力破解**

1. 如果已经枚举到了 WordPress 用户名，可以使用字典进行密码破解：

`wpscan --url http://example.com --passwords passwords.txt --usernames admin`

- `--passwords passwords.txt`：指定密码字典文件。
- `--usernames admin`：指定用户名。


2. 也可以同时使用多个用户名：

`wpscan --url http://example.com --passwords passwords.txt --usernames usernames.txt`

- `--usernames usernames.txt`：指定包含多个用户名的字典文件。


## wireshark搜索语法
### 符号规则

| 符号  |     | 描述   |     | 例子                  |
| --- | --- | ---- | --- | ------------------- |
| ==  |     | 等于   |     | ip.src==10.10.10.10 |
| !=  |     | 不等于  |     | ip.src!=10.10.10.10 |
| <   |     | 小于   |     | ip.ttl<20           |
| >   |     | 大于   |     | ip.ttl>20           |
| <=  |     | 小于等于 |     | ip.ttl<=233         |
| >=  |     | 大于等于 |     | ip.ttl>=233         |

### 捕获过滤器
| 过滤规则                 | 说明                         |
| -------------------- | -------------------------- |
| `host 192.168.1.1`   | 仅捕获来自或发送到 192.168.1.1 的数据包 |
| `net 192.168.1.0/24` | 仅捕获 192.168.1.0/24 子网的数据包  |
| `port 80`            | 仅捕获端口 80（HTTP）的流量          |
| `tcp`                | 仅捕获 TCP 数据包                |
| `udp`                | 仅捕获 UDP 数据包                |
| `tcp port 443`       | 仅捕获 TCP 端口 443（HTTPS）的流量   |
| `src host 10.0.0.1`  | 仅捕获源 IP 为 10.0.0.1 的流量     |
| `dst port 53`        | 仅捕获目标端口为 53（DNS）的数据包       |

### 显示过滤器

| 过滤规则                                 | 说明                                   |
|------------------------------------------|----------------------------------------|
| `ip.addr == 192.168.1.1`                | 过滤与 192.168.1.1 相关的所有流量       |
| `ip.src == 10.0.0.1`                    | 仅显示源 IP 为 10.0.0.1 的数据包        |
| `ip.dst == 8.8.8.8`                     | 仅显示目标 IP 为 8.8.8.8 的数据包       |
| `tcp.port == 80`                        | 仅显示 TCP 端口 80 的流量              |
| `udp.port == 53`                        | 仅显示 UDP 端口 53（DNS）的流量        |
| `http`                                  | 仅显示 HTTP 流量                       |
| `dns`                                   | 仅显示 DNS 流量                        |
| `tcp.flags.syn == 1 && tcp.flags.ack == 0` | 仅显示 TCP SYN 包（新建连接）          |
| `frame contains "password"`             | 仅显示数据包内容包含 "password" 的数据包 |
| `eth.addr == aa:bb:cc:dd:ee:ff`         | 仅显示特定 MAC 地址的流量              |

### 筛选器
#### ip筛选器

| 筛选                     |     | 描述                       |
| ---------------------- | --- | ------------------------ |
| ip                     |     | 显示所有的数据包                 |
| ip.addr==10.10.10.10   |     | 显示ip包含10.10.10.10的数据包    |
| ip.addr==10.10.10.0/24 |     | 显示10.10.10子网的所有ip的数据包    |
| ip.src==10.10.10.10    |     | 显示来自10.10.10.10地址的所有数据包  |
| ip.dst==10.10.10.10    |     | 显示发送到10.10.10.10地址的所有数据包 |
#### tcp udp筛选器

| 筛选              |     | 描述                 |     | 筛选              |     | 描述                 |
| --------------- | --- | ------------------ | --- | --------------- | --- | ------------------ |
| tcp.port==80    |     | 显示80端口的所有tcp数据包    |     | udp.port==80    |     | 显示80端口的所有udp数据包    |
| tcp.srcport==80 |     | 显示来自80端口的所有tcp数据包  |     | udp.port==80    |     | 显示来自80端口的所有udp数据包  |
| tcp.dstport==80 |     | 显示发送到80端口的所有tcp数据包 |     | udp.dstport==80 |     | 显示发送到80端口的所有udp数据包 |

#### http和dns过滤器

| 过滤                          | 描述                 |     | 过滤                     | 描述           |
| --------------------------- | ------------------ | --- | ---------------------- | ------------ |
| http                        | 显示所有的http数据包包      |     | dns                    | 显示所有的dns数据包  |
| http.response.method==200   | 显示响应码200的所有http数据包 |     | dns.response.method==0 | 显示所有的dns数据请求 |
| http.request.method=="GET"  | 显示所有http GET请求     |     | dns.flags.response==1  | 显示所有dns响应    |
| http.request.method=="post" | 显示所有http post请求    |     | dns.qry.type==1        | 显示所有的dns a级  |

#### http分析补充
| 描述                                                                                                                                                                                                                                                                                                                                                                                                        | wireshark                                                                                                                                                                                                              |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| “ HTTP 响应状态代码” 可用于获取唾手可得的成果：<br><br>- **200 OK：**请求成功。<br>- **301 永久移动：**资源被移动到新的 URL/路径（永久）。<br>- **302 暂时移动：**资源被移动到新的 URL/路径（临时）。<br>- **400 错误请求：**服务器不理解该请求。<br>- **401 未授权：** URL 需要授权（登录等）。<br>- **403 禁止：**无法访问所请求的 URL。 <br>- **404 未找到：**服务器找不到请求的 URL。<br>- **405 方法不允许：**使用的方法不合适或被阻止。<br>- **408 请求超时：**  请求时间超过服务器等待时间。<br>- **500 内部服务器错误：**请求未完成，意外错误。<br>- **503 服务不可用：**请求未完成服务器或服务已关闭。 | - `http.response.code == 200`<br><br>- `http.response.code == 401`<br><br>- `http.response.code == 403`<br><br>- `http.response.code == 404`<br><br>- `http.response.code == 405`<br><br>- `http.response.code == 503` |
| “ HTTP 参数” 可用于获取容易实现的成果：<br><br>- **用户代理：**浏览器和操作系统对 Web 服务器应用程序的识别。<br>- **请求URI：**指向服务器所请求的资源。  <br>    <br>- **完整*URI：**完整的 URI 信息。<br><br>***URI：**统一资源标识符。                                                                                                                                                                                                                                           | - `http.user_agent contains "nmap"`<br><br>- `http.request.uri contains "admin"`<br><br>- `http.request.full_uri contains "admin"`                                                                                     |
| “ HTTP 参数” 可用于获取容易实现的成果：<br><br>- **服务器：**服务器服务名称。  <br>    <br>- **主机：**服务器的主机名<br>- **连接：**连接状态。  <br>    <br>- **基于行的文本数据：**服务器提供的明文数据。<br>- **HTML 表单 URL 编码：** Web 表单信息。                                                                                                                                                                                                                             | - `http.server contains "apache"`<br><br>- `http.host contains "keyword"`<br><br>- `http.host == "keyword"`<br><br>- `http.connection == "Keep-Alive"`<br><br>- `data-text-lines contains "keyword"`                   |

HTTPS 的附加信息 ：  

| 笔记                                                                                                                                                                           | Wireshark 过滤器                                                                                                         |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| 获取唾手可得的成果的“HTTPS 参数” ：<br><br>- **请求：**列出所有请求  <br>    <br>- **TLS：**全局 TLS 搜索<br>- TLS 客户端请求<br>- TLS 服务器响应<br>- 本地简单服务发现协议 (SSDP)<br><br>**注意：** SSDP 是一种提供网络服务通告和发现的网络协议。 | - `http.request`<br><br>- `tls`<br><br>- `tls.handshake.type == 1`<br><br>- `tls.handshake.type == 2`<br><br>- `ssdp` |

#### 高级过滤
- 过滤条件 contains 包含
```
http.server contains "iis"  
#包含iis的http服务器
```

- 过滤条件 matches 匹配
```
http.host matches "\.(php\|html)" 
#列出所有.php和html的匹配的http数据包
```

- 过滤条件 in 在里面
```
tcp.port in {333 444 5556}
#列出所有的端口为333 444 5556的tcp数据包
```

- 过滤条件 string 字符串
```
strings (frame.number) matches "[13579]$"
#列出帧的结尾是基数的
```


### tcp扫描

| 描述             |     | wireshark 语法                                  |                |
| -------------- | --- | --------------------------------------------- | -------------- |
| 全局扫描           |     | tcp udp                                       |                |
| 只有syn flag     |     | tcp .flags.syn==1                             | tcp.flags == 2 |
| 只有ack flag     |     | tcp.flags.ack==1                              | tcp.flags ==16 |
| ack和ack的flag   |     | (tcp.flags.ack== 1) and (tcp .flags.syn== 1)  | tcp.flags ==16 |
| 只有rst的flag     |     | tcp.flags.reset == 1                          | tcp.flags == 1 |
| 只有rst和ack的flag |     | tcp.flags.ack== 1) and (tcp .flags.reset== 1) | tcp.flags == 1 |
| 只有fin标志        |     | tcp.flags.fin == 1                            | tcp.flags == 1 |

### arp分析

| 描述      | wireshark                                                              |
| ------- | ---------------------------------------------------------------------- |
| 全局搜索    | arp                                                                    |
| ARP请求   | arp.opcode == 1                                                        |
| ARP要求   | arp.opcode == 2                                                        |
| arp 扫描  | arp.dst.hw_mac == 00.00.00.00                                          |
| 检测arp投毒 | arp.duplicate-address-detected or arp.duplicate-address-frame`         |
| 检测arp泛洪 | ((arp) && (arp.opcode == 1)) && (arp.src.hw_mac == target-mac-address) |

### DHCP分析

| 描述           | wireshark                                                                                                                                                   |                                               |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- |
| 全局搜索         | dhcp或bootp                                                                                                                                                  |                                               |
| request      | dhcp.option.dhcp == 3                                                                                                                                       |                                               |
| ack          | dhcp.option.dhcp == 5                                                                                                                                       |                                               |
| nak包         | dhcp.option.dhcp == 6                                                                                                                                       |                                               |
| dhcp request | - **Option 12:** Hostname.<br>- **Option 50:** Requested IP address.<br>- **Option 51:** Requested IP lease time.<br>- **Option 61:** Client's MAC address. | - dhcp.option.hostname contains "keyword"     |
| dhcp ack     | - **Option 15:** Domain name.<br>- **Option 51:** Assigned IP lease time.                                                                                   | - dhcp.option.domain _name contains "keyword" |
| dhcp nak     | - **Option 56:** Message (rejection details/reason).                                                                                                        |                                               |

netbios 分析

| 描述                                                                                                    | wireshark                    |
| ----------------------------------------------------------------------------------------------------- | ---------------------------- |
| 全局                                                                                                    | nbns                         |
| - **Queries:** Query details.<br>- Query details 能有 "name, Time to live (TTL) and IP address details" | nbns.name contains "keyword" |
Kerberos 分析  

**Kerberos** 是 Microsoft Windows 域的默认身份验证服务。它负责在不受信任的网络上对两台或多台计算机之间的服务请求进行身份验证。最终目的是安全地证明身份。  

**Kerberos 调查概述：**

| **笔记**                                                                                                                                                                                     | **Wireshark 过滤器**                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------- |
| 全局搜索。                                                                                                                                                                                      | - `kerberos`                                                                                                                      |
| 用户帐户搜索：<br><br>- **CNameString：**用户名。<br><br>**注意：** 某些数据包可能在此字段中提供主机名信息。为避免这种混淆，请过滤**“$”值。以****“$”**结尾的值是主机名，不以“$”结尾的值是用户名。                                                               | - `kerberos.CNameString contains "keyword"` <br>- `kerberos.CNameString and !(kerberos.CNameString contains "$" )`                |
| “ Kerberos ” 选项可帮助您轻松获得所需成果：<br><br>- **pvno：**协议版本。<br>- **realm：**生成的票证的域名。  <br>- **sname：**生成的票证的服务和域名。 <br>- **addresses：**客户端 IP 地址和 NetBIOS 名称。  <br>    <br>**注意：**“地址”信息仅在请求包中可用。 | - `kerberos.pvno == 5`<br><br>- `kerberos.realm contains ".org"` <br><br>`kerberos.SNameString == "krbt`<br>kerberos.addresses == |

#### icmp 分析

| 描述                                                                  | wireshark              |
| ------------------------------------------------------------------- | ---------------------- |
| 全局                                                                  | icmp                   |
| ICMP” 选项：<br><br>- 数据包长度。<br>- ICMP 目标地址。  <br>- ICMP 有效负载中封装的协议标志。 | data.len > 64 and icmp |
#### icmp的type和code

##### icmp的code
- icmp.code == 0(网络不可达)
- icmp.code == 1(主机不可达)
- icmp.code == 2(协议不可达)
- icmp.code == 3(目的地不可达)
- icmp.code == 4(需要分片但 DF 标志置位)
- icmp.code == 5(源站隔离或源路由失败)
#### dns分析


| **笔记**                                                                                                                                                    | **Wireshark                                                          |
| --------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| 全局搜索                                                                                                                                                      | - `dns`                                                              |
|  DNS ”选项：<br><br>- 查询长度。<br>- DNS地址中的异常和非常规名称。<br>- 带有编码子域地址的长DNS地址。<br>- 已知模式如 dnscat 和 dns2tcp。<br>- 统计分析，例如特定目标的DNS请求异常量。<br><br>**!mdns：**禁用本地链接设备查询。 | - `dns contains "dnscat"`<br><br>- `dns.qry.name.len > 15 and !mdns` |

#### ftp分析


| **笔记**                                                                                                                               | **Wireshark 过滤器**                                                                                                                                                                             |
| ------------------------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 全局搜索                                                                                                                                 | - `ftp`                                                                                                                                                                                       |
| **“ FTP ”**选项可帮助您轻松获得目标：<br><br>- **x1x系列：**信息请求响应。<br>- **x2x系列：**连接消息。<br>- **x3x系列：**认证消息。<br><br>**注：** “200”表示命令成功。             | **---**                                                                                                                                                                                       |
| “x1x” 系列选项可帮助您轻松获得低垂的果实：<br><br>- **211：**系统状态。<br>- **212：**目录状态。<br>- **213：**文件状态                                                 | - `ftp.response.code == 211`                                                                                                                                                                  |
| “x2x” 系列选项可帮助您轻松获得收益：<br><br>- **220：**服务就绪。<br>- **227：**进入被动模式。<br>- **228：**长被动模式。<br>- **229：**扩展被动模式。                           | - `ftp.response.code == 227`                                                                                                                                                                  |
| “x3x” 系列选项可帮助您轻松获得低垂的果实：<br><br>- **230：**用户登录。<br>- **231：**用户注销。<br>- **331：**有效的用户名。<br>- **430：**用户名或密码无效<br>- **530：**未登录，密码无效。 | - `ftp.response.code == 230`                                                                                                                                                                  |
| 用于获取低垂果实的“ FTP ”命令：<br><br>- **用户：**用户名。<br>- **PASS：**密码。<br>- **CWD：**当前工作目录。<br>- **清單：**清單。                                      | - `ftp.request.command == "USER"`<br><br>- `ftp.request.command == "PASS"`<br><br>- `ftp.request.arg == "password"`                                                                           |
| 抓住低垂果实的高级用法示例：<br><br>- **暴力破解信号：**列出失败的登录尝试。<br>- **暴力破解信号：**列出目标用户名。<br>- **密码喷洒信号：**列出静态密码的目标。                                    | - `ftp.response.code == 530`<br><br>- `(ftp.response.code == 530) and (ftp.response.arg contains "username")`<br><br>- `(ftp.request.command == "PASS" ) and (ftp.request.arg == "password")` |

# sqlmap
 常见参数
 ## --dbs
 列出所有数据库

##   --columns -T "user" -D "mysql" 
列出mysql数据库中的user表的所有字段

##  --dump-all           
列出所有数据库所有表 
 
## --dump -T "" -D "" -C ""   
列出指定数据库的表的字段的数据(--dump -T users -D master -C surname) 
 
## --is-dba      
是否是数据库管理员 
 
## --data 
POST方式提交数据(--data "page=1&id=2") 
 
## --cookie "用;号分开"      
cookie注入(--cookies=”PHPSESSID=mvijocbglq6pi463rlgk1e4v52; security=low”) 
 
## --referer ""     
使用referer欺骗(--referer "http://www.baidu.com") 

 ## --columns        
 列出字段 
 
## --current-user   
获取当前用户名称 
 
## --current-db     
获取当前数据库名称 
 
## --users          
列数据库所有用户 
 
## --passwords      
数据库用户所有密码 
 
##--privileges     
查看用户权限(--privileges -U root)

# osquery命令
Osquery)是Facebook于 2014 年创建的[开源])代理。它将操作系统转换为关系数据库。它允许我们使用SQL查询从表中提出问题，例如返回正在运行的进程列表、在主机上创建的用户帐户以及与某些可疑域通信的过程。

### 启动osquery
osqueryi

### 帮助
.help

### 列出所有可查询的表
.tables命令

### 列出包含该术语的所有表格
.table 加表名

### 了解每个表的列和类型（称为架构）。 

.schema table_name

### **用户帐户 **

以下搜索查询使用该`users`表来检索有关在主机上创建的用户帐户的信息。
  
搜索查询：** `Select username, uid, description from users;`**

### **流程信息**

使用以下查询获取有关正在运行的进程的信息。  
**查询：** `Select pid, name, parent,path from processes;`

# **CeWL **

自定义词表生成器,是一个 ruby​​ 程序，可将特定 URL 爬取到定义的深度并返回关键字列表，密码破解者如John the Ripper、Medusa和 WFuzz 可以使用这些关键字来破解密码。Cewl 还有一个相关的命令行应用程序 FAB，它使用相同的元数据提取技术，使用 CeWL 等信息提取算法从已下载的文件中生成作者/制作者列表。

将此单词表存储在文件中
cewl -w word.txt

# tshark
|   |   |
|---|---|
|**工具/实用工具**|**目的和利益**|
|**capinfos**|提供指定捕获文件详细信息的程序。建议在开始调查之前查看捕获文件的摘要。|
|**grep**|帮助搜索纯文本数据。|
|**cut**|帮助从指定数据源剪切部分线条。|
|**uniq**|过滤重复的行/值。|
|**nl**|查看显示的行数。|
|**sed**|流编辑器。|
|**awk**|有助于模式搜索和处理的脚本语言。|


命令行界面和参数  

TShark 是一个基于文本（命令行）的工具。因此，对获得的结果进行深入和连续的分析很容易。有多个内置选项可供使用，以帮助分析师进行此类调查。但是，学习参数是必不可少的；您将需要内置选项和相关参数来控制输出，而不会被 TShark 的详细输出淹没。下表解释了最常用的参数。请注意，TShark 需要超级用户权限才能嗅探实时流量并列出所有可用接口。 

|   |   |
|---|---|
|**范围**|**目的**|
|-h|- 显示包含最常用功能的帮助页面。<br>- `tshark -h`|
|-v|- 显示版本信息。<br>- `tshark -v`|
|-D|- 列出可用的嗅探接口。<br>- `tshark -D`|
|-我|- 选择一个接口来捕获实时流量。<br>- `tshark -i 1`<br>- `tshark -i ens55`|
|**无参数**|- 像 tcpdump 一样嗅探流量。<br>- `tshark`|


没有接口参数是 的别名 `-i 1`。您还可以使用参数 设置不同的嗅探接口 `-i`。TShark 总是在嗅探开始时回显使用的接口名称。


让我们继续探索 TShark 的主要参数。 

|   |   |
|---|---|
|**范围**|**目的**|
|-r|- 读取/输入函数。读取捕获文件。<br>- `tshark -r demo.pcapng`|
|-c|- 数据包计数。捕获指定数量的数据包后停止。<br>- 例如，捕获/过滤/读取 10 个数据包后停止。<br>- `tshark -c 10`|
|-w|- 写入/输出功能。将嗅探到的流量写入文件。<br>- `tshark -w sample-capture.pcap`|
|-V|- 冗长。<br>- 提供**每个数据包的**详细信息。此选项将提供类似于Wireshark的“数据包详细信息窗格”的详细信息。<br>- `tshark -V`|
|-q|- 静音模式。<br>- 抑制终端上的数据包输出。<br>- `tshark -q`|
|-x|- 显示数据包字节数。<br>- 以十六进制和 ASCII 转储形式显示每个数据包的详细信息。<br>- `tshark -x`|

### 捕获条件参数

作为网络嗅探器和数据包分析器，TShark 可以配置为计数数据包并在特定点停止或以循环结构运行。下面解释了最常见的参数。

|   |   |
|---|---|
|**范围**|**目的**|
||定义单次运行/循环的捕获条件。 完成条件后停止 。也称为“自动停止”。|
|-一个|- **持续时间：** 嗅探流量并在 X 秒后停止。创建一个新文件并将输出写入其中。  <br>    <br><br>- `tshark -w test.pcap -a duration:1`<br><br>- **文件大小：** 定义最大捕获文件大小。达到 X 文件大小（KB）后停止。<br><br>- `tshark -w test.pcap -a filesize:10`<br><br>- **文件：** 定义输出文件的最大数量。输出 X 个文件后停止。<br><br>- `tshark -w test.pcap -a filesize:10 -a files:3`|
||环形缓冲区控制选项。 定义多次运行/循环的捕获条件。  （无限循环）。|
|-b|- **持续时间：** 嗅探流量 X 秒，创建一个新文件并将输出写入其中。   <br>    <br><br>- `tshark -w test.pcap -b duration:1`<br><br>- **文件大小：** 定义最大捕获文件大小。达到文件大小 X (KB) 后，创建一个新文件并将输出写入其中。<br><br>- `tshark -w test.pcap -b filesize:10`<br><br>- **文件：** 定义输出文件的最大数量。创建 X 个文件后重写第一个/最旧的文件。<br><br>- `tshark -w test.pcap -b filesize:10 -b files:3`|

### 捕获条件参数

作为网络嗅探器和数据包分析器，TShark 可以配置为计数数据包并在特定点停止或以循环结构运行。下面解释了最常见的参数。

|   |   |
|---|---|
|**范围**|**目的**|
||定义单次运行/循环的捕获条件。 完成条件后停止 。也称为“自动停止”。|
|-a|- **持续时间：** 嗅探流量并在 X 秒后停止。创建一个新文件并将输出写入其中。  <br>    <br><br>- `tshark -w test.pcap -a duration:1`<br><br>- **文件大小：** 定义最大捕获文件大小。达到 X 文件大小（KB）后停止。<br><br>- `tshark -w test.pcap -a filesize:10`<br><br>- **文件：** 定义输出文件的最大数量。输出 X 个文件后停止。<br><br>- `tshark -w test.pcap -a filesize:10 -a files:3`|
||环形缓冲区控制选项。 定义多次运行/循环的捕获条件。  （无限循环）。|
|-b|- **持续时间：** 嗅探流量 X 秒，创建一个新文件并将输出写入其中。   <br>    <br><br>- `tshark -w test.pcap -b duration:1`<br><br>- **文件大小：** 定义最大捕获文件大小。达到文件大小 X (KB) 后，创建一个新文件并将输出写入其中。<br><br>- `tshark -w test.pcap -b filesize:10`<br><br>- **文件：** 定义输出文件的最大数量。创建 X 个文件后重写第一个/最旧的文件。<br><br>- `tshark -w test.pcap -b filesize:10 -b files:3`|

### 数据包过滤参数 | 捕获和显示过滤器  

TShark 中的数据包过滤有两种方式：实时（捕获）过滤和捕获后（显示）过滤。这两个维度可以用两种不同的方法进行过滤：使用预定义语法或 Berkeley 数据包过滤器 ( BPF )。TShark 支持这两种方式，因此您可以使用 Wireshark 过滤器和BPF来过滤流量。如前所述，TShark 是 Wireshark 的命令行版本，因此我们需要使用不同的过滤器来捕获和过滤数据包。Wireshark  [：数据包操作](https://tryhackme.com/r/room/wiresharkpacketoperations)室的简要回顾：

|   |   |
|---|---|
|**捕获过滤器**|实时过滤选项。目的是仅**保存**流量的特定部分。它是在捕获流量之前设置的，并且在实时捕获期间不可更改。|
|**显示过滤器**|捕获后过滤选项。目的是通过**减少可见数据包的数量**来调查数据包，这在调查过程中是可以改变的。|

|   |   |
|---|---|
|**范围**|**目的**|
|-f|捕获过滤器。与BPF语法和 Wireshark 的捕获过滤器相同。|
|-Y|**显示过滤器。与Wireshark 的显示过滤器**相同。|

### 捕获过滤器  

这里使用了 Wireshark 的捕获过滤器语法。捕获/ BPF过滤器的基本语法如下所示。 您可以[在此处](https://www.wireshark.org/docs/man-pages/pcap-filter.html) 和 [此处](https://gitlab.com/wireshark/wireshark/-/wikis/CaptureFilters#useful-filters)阅读有关捕获过滤器语法的更多信息 。布尔运算符也可用于这两种类型的过滤器。 [](https://www.wireshark.org/docs/man-pages/pcap-filter.html)[](https://gitlab.com/wireshark/wireshark/-/wikis/CaptureFilters#useful-filters)

|   |   |
|---|---|
|**限定符**|**详细信息和可用选项**|
|**类型**|目标匹配类型。您可以过滤 IP 地址、主机名、IP 范围和端口号。请注意，如果您未设置限定符，则默认使用“主机”限定符。<br><br>- 主机 \| 网络 \| 端口 \| 端口范围<br>- 过滤主机<br><br>- `tshark -f "host 10.10.10.10"`<br><br>- 过滤网络范围 <br><br>- `tshark -f "net 10.10.10.0/24"`<br><br>- 过滤端口<br><br>- `tshark -f "port 80"`<br><br>- 过滤端口范围<br><br>- `tshark -f "portrange 80-100"`|
|**方向**|目标方向/流向。请注意，如果不使用方向运算符，它将等于“任一”并涵盖两个方向。<br><br>- 源地址 \| 目的地址<br>- 过滤源地址<br><br>- `tshark -f "src host 10.10.10.10"`<br><br>- 过滤目标地址<br><br>- `tshark -f "dst host 10.10.10.10"`|
|**协议**|目标协议。<br><br>- arp \| 以太 \| icmp \| ip \| ip6 \| tcp \| udp<br>- 过滤 TCP<br><br>- `tshark -f "tcp"`<br><br>- 过滤 MAC 地址<br><br>- `tshark -f "ether host F8:DB:C5:A2:5D:81"`<br><br>- 您还可以使用 IANA 分配的 IP 协议号来过滤协议。<br>- 过滤 IP 协议 1 (ICMP)<br><br>- `tshark -f "ip proto 1"`<br>- [**已分配的互联网协议号码**](https://www.iana.org/assignments/protocol-numbers/protocol-numbers.xhtml)|

|   |   |
|---|---|
|**捕获过滤器类别**|**细节**|
|**主机过滤**|捕获 发往或来自特定主机的流量。<br><br>- 使用 cURL 生成流量。此命令将默认HTTP查询发送到指定地址。<br><br>- `curl tryhackme.com`<br><br>- 主机的 TShark 捕获过滤器<br><br>- `tshark -f "host tryhackme.com"`|
|**IP 过滤**|捕获 特定端口的流量。我们将使用 Netcat 工具在特定端口上创建噪声。<br><br>- 使用 Netcat 生成流量。此处指示 Netcat 提供详细信息（详细程度），并将超时设置为 5 秒。<br><br>- `nc 10.10.10.10 4444 -vw 5`<br><br>- 特定 IP 地址的 TShark 捕获过滤器<br><br>- `tshark -f "host 10.10.10.10"`|
|**端口过滤**|捕获 特定端口的流量。我们将使用 Netcat 工具在特定端口上创建噪声。<br><br>- 使用 Netcat 生成流量。此处指示 Netcat 提供详细信息（详细程度），并将超时设置为 5 秒。<br><br>- `nc 10.10.10.10 4444 -vw 5`<br><br>- 针对端口 4444 的 TShark 捕获过滤器<br><br>- `tshark -f "port 4444"`|
|**协议过滤**|捕获 特定协议的流量。我们将使用 Netcat 工具在特定端口上创建噪声。<br><br>- 使用 Netcat 生成流量。此处指示 Netcat 使用 UDP、提供详细信息（详细程度），并将超时设置为 5 秒。<br><br>- `nc -u 10.10.10.10 4444 -vw 5`<br><br>- TShark 捕获过滤器<br><br>- `tshark -f "udp"`|


|   |   |
|---|---|
|**显示过滤器类别**|**详细信息和可用选项**|
|**协议：IP**|- 过滤IP 而不指定方向。  <br>    <br><br>- `tshark -Y 'ip.addr == 10.10.10.10'`<br><br>- 过滤网络范围 <br><br>- `tshark -Y 'ip.addr == 10.10.10.0/24'`<br><br>- 过滤源 IP<br><br>- `tshark -Y 'ip.src == 10.10.10.10'`<br><br>- 过滤目标 IP<br><br>- `tshark -Y 'ip.dst == 10.10.10.10'`|
|**协议：TCP**|- 过滤TCP 端口  <br>    <br><br>- `tshark -Y 'tcp.port == 80'`<br><br>- 过滤源TCP端口<br><br>- `tshark -Y 'tcp.srcport == 80'`|
|**协议：HTTP**|- 过滤HTTP 数据包  <br>    <br><br>- `tshark -Y 'http'`<br><br>- 过滤 响应代码为“200”的HTTP数据包<br><br>- `tshark -Y "http.response.code == 200"`|
|**协议：DNS**|- 过滤DNS 数据包  <br>    <br><br>- `tshark -Y 'dns'`<br><br>- 过滤所有DNS “A” 数据包<br><br>- `tshark -Y 'dns.qry.type == 1'`|

#### 例子

1.具有“65.208.228.223”IP 地址的数据包数量是多少？
```
tshark -r demo.pcapng -Y 'ip.addr == 65.208.228.223' |nl
```

2.带有“TCP 端口 3371”的数据包数量是多少 ？
```
tshark -r 演示。pcapng -Y'tcp.port == 3371' | nl
```

3. 以“145.254.160.237”IP 地址作为源地址的数据包数量是多少？
```
tshark -r demo.pcapng -Y 'ip.src == 145.254.160.237' |nl
```

## 统计信息

|   |   |
|---|---|
|**范围**|**目的**|
|- 颜色|- 类似 Wireshark 的彩色输出。<br>- `tshark --color`|
|-z|- 统计数据<br>- 此参数下有多个可用选项。您可以使用以下选项查看此参数下的可用过滤器：<br><br>- `tshark -z help`<br><br>- 示例用法。<br><br>- `tshark -z filter`<br><br>- 每次过滤统计信息时，首先显示数据包，然后显示提供的统计信息。您可以使用`-q`参数抑制数据包并专注于统计信息。|


### 彩色输出

颜色选项可以通过 `--color`参数激活

```
tshark -r colour.pcap --color
```

### 协议层次结构

用`-z io,phs -q`参数查看协议层次结构。


查看协议层次结构

```shell-session
user@ubuntu$ tshark -r demo.pcapng -z io,phs -q
===================================================================
Protocol Hierarchy Statistics
Filter: 

  eth                                    frames:43 bytes:25091
    ip                                   frames:43 bytes:25091
      tcp                                frames:41 bytes:24814
        http                             frames:4 bytes:2000
          data-text-lines                frames:1 bytes:214
            tcp.segments                 frames:1 bytes:214
          xml                            frames:1 bytes:478
            tcp.segments                 frames:1 bytes:478
      udp                                frames:2 bytes:277
        dns                              frames:2 bytes:277
===================================================================
```


查看整个数据包树后，您可以关注特定协议，如下所示。将`udp` 关键字添加到过滤器以关注UDP协议。


```
user@ubuntu$ tshark -r demo.pcapng -z io,phs,udp -q =================================================================== Protocol Hierarchy Statistics Filter: udp eth frames:2 bytes:277 ip frames:2 bytes:277 udp frames:2 bytes:277 dns frames:2 bytes:277 ===================================================================
```

###  数据包长度树
```
user@ubuntu$ tshark -r demo.pcapng -z plen,tree -q ========================================================================================================================= Packet Lengths: Topic / Item Count Average Min val Max val Rate (ms) Percent Burst rate Burst start ------------------------------------------------------------------------------------------------------------------------- Packet Lengths 43 583.51 54 1484 0.0014 100 0.0400 2.554 0-19 0 - - - 0.0000 0.00 - - 20-39 0 - - - 0.0000 0.00 - - 40-79 22 54.73 54 62 0.0007 51.16 0.0200 0.911 80-159 1 89.00 89 89 0.0000 2.33 0.0100 2.554 160-319 2 201.00 188 214 0.0001 4.65 0.0100 2.914 320-639 2 505.50 478 533 0.0001 4.65 0.0100 0.911 640-1279 1 775.00 775 775 0.0000 2.33 0.0100 2.984 1280-2559 15 1440.67 1434 1484 0.0005 34.88 0.0200 2.554 2560-5119 0 - - - 0.0000 0.00 - - 5120 and greater 0 - - - 0.0000 0.00 - - -------------------------------------------------------------------------------------------------------------------------
```


### 端点

端点统计视图可帮助分析人员概览各个端点。它还显示与每个端点关联的数据包数量。

使用 `-z endpoints,ip -q`参数可以查看 IP 端点

|   |   |
|---|---|
|**Filter**|**Purpose**|
|eth|- - 以太网地址|
|ip|- IPv4 addresses|
|ipv6|- IPv6 addresses|
|tcp|- TCP addresses<br>- Valid for both IPv4 and IPv6|
|udp|- UDP addresses<br>- Valid for both IPv4 and IPv6|
|wlan|- IEEE 802.11 addresses|

```
tshark -r demo.pcapng -z endpoints,ip -q ================================================================================ IPv4 Endpoints Filter: | Packets | | Bytes | | Tx Packets | | Tx Bytes | | Rx Packets | | Rx Bytes | 145.254.160.237 43 25091 20 2323 23 22768 65.208.228.223 34 20695 18 19344 16 1351 216.239.59.99 7 4119 4 3236 3 883 145.253.2.203 2 277 1 188 1 89 ================================================================================
```

### 对话

对话视图可帮助分析师概览两个特定连接点之间的流量。与端点过滤类似，对话可以以多种格式查看。此过滤器使用与“端点”选项相同的参数。使用这些`-z conv,ip -q`参数可查看 IP 对话。

```
user@ubuntu$ tshark -r demo.pcapng -z conv,ip -q ================================================================================ IPv4 Conversations Filter: | <- | | -> | | Total | Relative | Duration | Frames Bytes | | Frames Bytes | | Frames Bytes | Start | 65.208.228.223 <-> 145.254.160.237 16 1351 18 19344 34 20695 0.000000000 30.3937 145.254.160.237 <-> 216.239.59.99 4 3236 3 883 7 4119 2.984291000 1.7926 145.253.2.203 <-> 145.254.160.237 1 89 1 188 2 277 2.553672000 0.3605 ================================================================================
```

### 专家信息

-z expert -q参数


# gobuster

## 目录爆破

```
gobuster dir -u <目标URL> -w <字典路径> [参数]
```

[[tryhackme-Gobuster：基础知识#task3]]

​### 常用参数​：

|参数|说明|示例|
|---|---|---|
|`-u`|目标 URL|`-u http://example.com`|
|`-w`|字典文件路径|`-w /usr/share/wordlists/dirb/common.txt`|
|`-x`|扩展名过滤|`-x php,html`（搜索 `.php` 和 `.html` 文件）|
|`-t`|线程数（默认 10）|`-t 50`（提高扫描速度）|
|`-s`|仅显示指定状态码|`-s 200,301`（过滤有效结果）|
|`-b`|排除状态码|`-b 404,403`（减少干扰）|
|`--proxy`|代理设置|`--proxy http://127.0.0.1:8080`|

eg:
```
gobuster dir -u http://example.com -w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt -x php -t 50 -s 200,301
```

### ​DNS 子域名枚举（dns 模式）​

​**​用途​**​：发现目标域名的子域名（支持泛解析绕过）

```bash
gobuster dns -d <目标域名> -w <字典路径> [参数]
```

​**​常用参数​**​：

|参数|说明|示例|
|---|---|---|
|`-d`|目标域名|`-d example.com`|
|`-w`|子域名字典路径|`-w subdomains-top1mil-5000.txt`|
|`--resolver`|指定 DNS 服务器|`--resolver 8.8.8.8`|
|`-i`|显示子域名 IP 地址|`-i`|

​**​示例​**​：
[[tryhackme-Gobuster：基础知识#task5]]

```bash
gobuster dns -d example.com -w /usr/share/wordlists/subdomains-top1mil-5000.txt --resolver 1.1.1.1 -t
```
## 高级用法

###​ 递归扫描与深度控制​

- ​**​目录模式递归扫描​**​：

```bash
gobuster dir -u http://example.com -w wordlist.txt -r -R 2  # 最大递归深度为2[3](@ref)
```

### **自定义请求头与认证​*

- ​**​添加 HTTP 头部​**​：

```bash
gobuster dir -u http://example.com -w wordlist.txt -H 'X-Forwarded-For: 127.0.0.1' -H 'Cookie: sess
```

####  ​**​模糊测试（fuzz 模式）​**​

​**​用途​**​：替换 URL 中的 `FUZZ` 关键字进行动态测试

```bash
gobuster fuzz -u http://example.com/FUZZ -w /path/to/fuzz-words.txt
```


# feroxbuster

## **基础扫描​**​

| 参数   | 说明          | 示例                                                                  |
| ---- | ----------- | ------------------------------------------------------------------- |
| `-u` | 目标 URL（必填）  | `feroxbuster -u http://example.com`                                 |
| `-w` | 指定字典路径      | `-w /usr/share/seclists/Discovery/Web-Content/raft-large-words.txt` |
| `-x` | 扩展名扫描（支持多组） | `-x php,html -x js`                                                 |
| `-t` | 线程数（默认 50）  | `-t 100`（高风险，可能触发 WAF）                                              |

##  **过滤与优化​**​

|参数|说明|示例|
|---|---|---|
|`--filter-status`|排除状态码|`--filter-status 404,403`（减少噪音）|
|`-s`|仅显示指定状态码|`-s 200,301`|
|`--auto-tune`|自动调整速率避免封禁|`--auto-tune`|
|`--proxy`|代理设置（支持 HTTP/Socks）|`--proxy http://127.0.0.1:8080`|


## **高级功能​**​

|参数|说明|示例|
|---|---|---|
|`-r`|递归扫描子目录|`-r`（默认最大深度 4）|
|`--depth`|设置递归深度|`--depth 2`（限制扫描层级）|
|`-H`|自定义请求头|`-H "Cookie: session=abc"`|
|`--json`|输出 JSON 格式报告|`--json -o report.json`|



## 实战

### **1. 基本目录爆破​**​

```bash
feroxbuster -u http://example.com -w raft-large-words.txt -x php,html -s 200,301 --auto-tune
```

- ​**​用途​**​：快速发现 Web 根目录下的敏感文件（如 `admin.php`、`backup.zip`）。

### **2. 递归扫描子目录​**​


```bash
feroxbuster -u http://example.com/blog -r --depth 3 -x txt,json --filter-status 404
```

- ​**​用途​**​：深入扫描嵌套目录，排除 404 干扰。

### **3. 多目标批量扫描​**​

```bash
cat targets.txt | feroxbuster --stdin -w wordlist.txt -s 200 -o multi_scan.log
```

- ​**​用途​**​：从文件读取多个 URL 进行并行扫描


# hashcat
## **哈希类型（-m）​**​

|常用类型|对应算法|
|---|---|
|0|MD5|
|1000|NTLM|
|22000|WPA-PBKDF2-PMKID|
|10000|Django SHA256|
|1700|SHA2-512|
|13600|ZIP加密文件|

​## **​攻击模式（-a）​**​

|模式|说明|应用场景|
|---|---|---|
|0|字典攻击（直接匹配）|已知常见密码库|
|1|组合攻击（两字典笛卡尔积）|如"123"+"abc"→123abc|
|3|掩码暴力破解|已知密码结构|
|6/7|混合攻击（字典+掩码组合）|部分已知密码结构|


#  Enum4Linux 

| 参数 | 描述                             |
|------|----------------------------------|
| -U   | 获取用户列表                     |
| -M   | 获取机器列表                     |
| -N   | 获取名称列表转储（不同于 -U 和-M）|
| -S   | 获取共享列表                     |
| -P   | 获取密码策略信息                 |
| -G   | 获取群组和成员列表               |
| -a   | 以上全部（完整基本枚举）         |

# wfuff
模糊测试
## 用法
### 1. ​**​目录与文件扫描​**​
通过字典模糊测试目标路径或文件，常用于发现隐藏资源
```
# 扫描目标URL的路径（FUZZ为占位符）
wfuzz -c -w /usr/share/wfuzz/wordlist/general/common.txt -u http://target.com/FUZZ

# 指定文件后缀（如.php）
wfuzz -c -w wordlist.txt -u http://target.com/FUZZ.php
```

### 2. **子域名发现​**​
通过字典枚举目标子域名
```
wfuzz -c -w subdomains.txt -u http://FUZZ.target.com
```

^59efba

## 参数
### 1. **`--hc`：按 HTTP 状态码过滤​**​

- ​**​作用​**​：隐藏或显示特定状态码的响应。
- ​**​适用场景​**​：
    - 隐藏默认错误状态码（如 `--hc 404`）。
    - 显示特定状态码（如 `--sc 200`），但在此问题中无效。

### 2.  **`--hw`：按响应单词数过滤​**​

- ​**​作用​**​：根据响应正文中的单词数量筛选结果。
- ​**​关键点​**​：
    - 无效参数可能返回固定模板（如错误页面的单词数固定为 500）。
    - 有效参数响应内容不同，单词数会变化。

###  3. `--hl`：按响应行数过滤​​

- ​**​作用​**​：根据响应正文的行数筛选结果。
- ​**​适用场景​**​：
    - 错误页面可能包含固定行数（如 7 行）。

### 4. **`--hh`**：隐藏响应内容字符数
- 进一步过滤无效或重复响应

### 5. --fw 过滤响应中单词数

^bf5ac4

- 过滤固定字数