## linux的攻击面
- Open ports 开放端口
- Running services 运行的服务
- Running software or applications with vulnerabilities 运行着的软件和应用的漏洞
- Network communication 网络交流

### 修复方法
- Identifying and patching the vulnerabilities   识别修复漏洞
- Minimizing the usage of unwanted services 减少不必要的服务的使用
- Check the interfaces where the user interacts 检查用户交互的界面
- Minimizing the publically exposed services, applications, ports, etc 尽量减少公开的服务、应用程序、端口等

### 查找的关键点
	- System logs 系统日志
	- auth.log, syslog, krnl.log, etc 三个日志
	- Network traffic 网络流量
	- Running processes 运行中的进程
	- Running services 运行着的服务
	- The integrity of the files and processes 文件和进程的完整性

## 系统日志：

- 在/var/log/syslog`
- 这对于识别系统范围内的事件、错误和警告非常有用。可以深入了解系统组件或服务的问题。
- 它包含一般系统消息，包括内核消息、系统服务和应用程序日志。
- 该日志文件对于识别系统范围的事件、错误和警告很有用。
- 它可以提供有关系统组件或服务问题的见解
##  消息

- 地点：`/var/log/messages`
- 与类似`syslog`，该文件包含系统消息和内核日志。
- 有助于诊断系统问题和跟踪系统活动。
- 发现与硬件或内核错误相关的异常条目可能表明有人试图篡改系统组件。
- 例如，重复的内核崩溃消息可能表明存在针对系统稳定性的拒绝服务攻击

##  身份验证日志

- 在/var/log/auth.log`
- 该文件记录身份验证尝试，包括成功和失败的登录尝试。
- 它是检测未经授权的访问尝试和暴力攻击的重要日志文件。
- 例如，发现来自陌生 IP 地址的多次登录尝试失败或登录时间不寻常，可能表示存在暴力攻击或试图获取未经授权的访问。


## **配置文件：**

从安全角度来看，一些可能至关重要的常见配置是：

- **/etc/passwd**：此文件包含有关用户帐户的信息。

- **/etc/shadow**：此文件包含用户账户的散列密码。

- **/etc/group**：此文件定义组及其关联的用户。组用于管理权限并组织具有相似权限的用户。

- **/etc/sudoers**：配置sudo权限，可以作为权限提升的目标。