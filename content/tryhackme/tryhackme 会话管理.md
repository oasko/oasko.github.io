## Learning Objectives  学习目标

了解什么是会话管理
    
了解身份验证和授权之间的区别，以及它们如何在会话管理中发挥作用
  
了解两种主要的会话管理方法

了解会话管理生命周期
    
了解如何实际利用易受攻击的会话管理实施
## 会话管理
会话管理是管理这些会话并确保它们保持安全的过程。

## iaaa
### Identification  鉴定
身份识别是验证用户身份的过程。这从用户声称是特定身份开始。在大多数 Web 应用程序中，这是通过提交您的用户名来完成的。您声称您是与特定用户名关联的人员。一些应用程序使用唯一创建的用户名，而其他应用程序将使用您的电子邮件地址作为用户名。

### Authentication  认证
身份验证是确保用户是他们所说的用户的过程。在身份证明中，您提供用户名，用于身份验证，您需要提供证明您是您所说的那个人。例如，您可以提供与已申请的用户名关联的密码。如果此信息有效，则 Web 应用程序可以确认此信息;这就是会话创建开始的地方。

### Authorisation  授权
授权是确保特定用户具有执行所请求作所需的权限的过程。例如，虽然所有用户都可以查看数据，但只有少数用户可以修改数据。在会话管理生命周期中，会话跟踪在授权中起着关键作用。

### Accountability  问 责
问责制是创建用户所执行作记录的过程。我们应该跟踪用户的会话并记录使用特定会话执行的所有作。在发生安全事件时，此信息对于拼凑所发生的事情起着关键作用。


## cookie和token
### cookie
基于 Cookie 的会话管理通常被称为管理会话的老式方式。一旦 Web 应用程序想要开始跟踪，在响应中，将发送 Set-Cookie 标头值。您的浏览器将解释此标头以存储新的 Cookie 值。

### token
基于令牌的会话管理是一个相对较新的概念。它没有使用浏览器的自动 cookie 管理功能，而是依赖于客户端代码来完成该过程。身份验证后，Web 应用程序会在请求正文中提供令牌。然后使用客户端 JavaScript 代码，此令牌将存储在浏览器的 LocalStorage 中。

# 容易出现漏洞
## 会话创建
### Weak Session Values  弱会话值

### Controllable Session Values 可控的会话值

### Session Fixation  会话固定

### Insecure Session Transmission 不安全的会话传输

## Session Tracking  会话跟踪

### Authorisation Bypass
当没有对是否允许用户执行他们请求的作执行足够的检查时，就会发生授权绕过。从本质上讲，这无法正确跟踪用户的会话及其相关权限。还值得讨论两种类型的授权绕过

#### 垂直旁路

#### 水平旁路 

### Insufficient Logging  日志记录不足

## Session Expiry  会话过期

## Session Termination  会话终止

## 练习


![image.png](https://s2.loli.net/2025/05/12/Mit7JR5moBnAOrq.png)


![image.png](https://s2.loli.net/2025/05/12/6CPUXxKHVRoOEcj.png)



![image.png](https://s2.loli.net/2025/05/12/JaRBolAM3pyv6iI.png)



![image.png](https://s2.loli.net/2025/05/12/6aiED4L1wYzlqvR.png)


![image.png](https://s2.loli.net/2025/05/12/kjOBKxqSzuDVFw4.png)

会发现这里的cookie是一样的

即使已达到 cookie 过期时间，也会使用相同的 cookie 标头集将 cookie 刷新为完全相同的值。这可能指向持久性 Cookie。

删掉cookie会发现没有数字了

![image.png](https://s2.loli.net/2025/05/12/4ruBcoNjORGPla2.png)


![image.png](https://s2.loli.net/2025/05/12/BqYKaMWTcs82AuG.png)




![image.png](https://s2.loli.net/2025/05/12/bJx9Pq4Tl8Sw7Zr.png)


![image.png](https://s2.loli.net/2025/05/12/LMDigXHS3ABk47Y.png)


![image.png](https://s2.loli.net/2025/05/12/rloOGqcwU9D6sKi.png)


![image.png](https://s2.loli.net/2025/05/12/BY9cgxS1aE8dNAO.png)
