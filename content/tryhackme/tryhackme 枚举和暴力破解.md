## 常见地方
### 注册页面
### 密码重置功能
### 详细错误
###  数据泄露信息

## 诱发详细错误
### 无效的登录尝试 
这就像敲每扇门，看看哪扇门会打开。通过故意输入不正确的用户名或密码，攻击者可以触发有助于区分有效和无效用户名的错误消息。例如， 输入不存在的用户名可能会触发与输入不存在的用户名不同的错误消息，从而显示哪些用户名处于活动状态。

### SQL 注入 
这种技术涉及将恶意 SQL 命令滑入输入字段，希望系统会绊倒并泄露有关其数据库结构的信息。例如，在登录字段中 p 连接单个引号 （ '） 可能会导致数据库引发错误，从而无意中暴露有关其架构的详细信息。

### 文件包含/路径遍历 
通过纵文件路径，攻击者可以尝试访问受限制的文件，诱使系统出现泄露内部路径的错误。例如，using 目录遍历序列，如 ../../ 可能会导致泄露受限文件路径的错误。

### 表单作 
调整表单字段或参数可能会诱使应用程序显示泄露后端逻辑或敏感用户信息的错误。例如， 通过隐藏表单字段来触发验证错误可能会揭示对预期数据格式或结构的见解。

### 应用程序模糊测试 
向应用程序的各个部分发送意外输入以查看其反应，这有助于识别弱点。例如， 像 Burp Suite Intruder 这样的 t ools 用于自动化该过程，用不同的有效负载轰炸应用程序，以查看哪些有效负载会引发信息错误。

## 密码重置流漏洞
### 流程
#### 基于电子邮件的重置
当用户重置其密码时，应用程序会向用户的注册电子邮件地址发送一封包含重置链接或令牌的电子邮件。然后，用户单击此链接，该链接会将他们定向到一个页面，他们可以在其中输入新密码并确认它，或者系统将自动为用户生成新密码。此方法在很大程度上依赖于用户电子邮件帐户的安全性和发送的链接或令牌的机密性。

#### 基于安全问题的重置
这涉及用户回答他们在创建账户时设置的一系列预配置的安全问题。如果答案正确，系统允许用户继续重置其密码。虽然此方法通过要求只有用户应该知道的信息来增加一层安全性，但如果攻击者获得了对个人身份信息 （PII） 的访问权限，则可能会受到威胁，这些信息有时很容易找到或猜到。

#### 基于 SMS 的重置
此功能类似于基于电子邮件的重置，但使用 SMS 将重置代码或链接直接发送到用户的移动电话。用户收到代码后，他们可以在提供的网页上输入代码以访问密码重置功能。此方法假定对用户手机的访问是安全的，但它可能容易受到 SIM 卡交换攻击或拦截。

### 这些方法中的每一种都有其漏洞
#### 可预测令牌 
如果链接或 SMS 消息中使用的重置令牌是可预测的或遵循顺序模式，则攻击者可能会猜测或暴力破解以生成有效的重置 URL。

#### 令牌过期问题 
令牌有效期过长或使用后未立即过期，这为攻击者提供了机会窗口。令牌迅速过期以限制此窗口至关重要。

#### 验证不足 
验证用户身份的机制（如安全问题或基于电子邮件的身份验证）可能很弱，如果问题太常见或电子邮件帐户被盗用，则容易受到利用。

#### 信息泄露 
任何指定电子邮件地址或用户名是否已注册的错误消息都可能无意中帮助攻击者进行枚举工作，确认帐户的存在。

#### 不安全传输 
通过非 HTTPS 连接传输重置链接或令牌可能会使这些关键元素暴露于网络窃听者的拦截。


### 练习
token暴力破解 三位数
![image.png](https://s2.loli.net/2025/05/11/L1KQmGYEohw4Vl5.png)


![image.png](https://s2.loli.net/2025/05/11/4jnYopkCPuQcZwA.png)


## 利用 HTTP 基本身份验证
### 

### 练习
爆破账户密码 输入账户密码抓包之后 是base64编码
![image.png](https://s2.loli.net/2025/05/11/BLZ2dsIwYAG8Wnf.png)

bp有解码工具 能解出编码
![image.png](https://s2.loli.net/2025/05/11/dyCjVzcHBEuNPl2.png)

设置payload 加上规则 前面加上前缀admin：不然的话爆破只有密码
![image.png](https://s2.loli.net/2025/05/11/jdr3ohEni56MPwS.png)

加上base64编码
![image.png](https://s2.loli.net/2025/05/11/nEkyvoju4GYZNSM.png)

最后的url符号记得去掉等于号

开始破解 找到答案了
![image.png](https://s2.loli.net/2025/05/11/8UGSaImHojlBWPi.png)

![image.png](https://s2.loli.net/2025/05/11/EhagCtcIpJAWzRj.png)

解码发现密码

## Wayback URL 和 Google Dorks
### waybackurls工具
将 Internet Archive 的 Wayback Machine （https://archive.org/web/） 想象成一台时间机器。它允许您返回并探索旧版本的网站，发现不再可见但可能仍徘徊在服务器上的文件和目录。这些遗迹有时可以为当前系统提供后门。

### Google Dorks 

To find administrative panels: site:example.com inurl:admin
要查找管理面板：site:example.com inurl：admin
    
To unearth log files with passwords: 
filetype:log "password" site:example.com
要挖掘带有密码的日志文件： filetype:log "password" site:example.com

To discover backup directories: intitle:"index of" "backup" site:example.com
要发现备份目录： intitle:"index of" "backup" site:example.com
