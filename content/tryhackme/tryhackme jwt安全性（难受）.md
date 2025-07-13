基于令牌的会话管理是一个相对较新的概念。它没有使用浏览器的自动 cookie 管理功能，而是依赖于客户端代码来完成该过程。身份验证后，Web 应用程序会在请求正文中提供令牌。然后使用客户端 JavaScript 代码，此令牌将存储在浏览器的 LocalStorage 中。

它通过 Authorization： Bearer 标头传递

## api的例子
```
对于身份验证，可以发出以下 cURL 请求：

curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "passwordX" }' http://10.10.248.106/api/v1.0/exampleX
```

```
对于用户验证，可以发出以下 cURL 请求：

curl -H 'Authorization: Bearer [JWT token]' http://10.10.248.106/api/v1.0/example2?username=Y

[JWT token] 组件必须替换为从第一个请求收到的 JWT。在这种情况下，Y 可以是 user 或 admin，具体取决于您的权限。
```

## JWT Structure  JWT 结构
JWT 由三个组件组成，每个组件都由 Base64Url 编码并由点分隔：
### 标头 
标头通常指示令牌的类型（即 JWT）以及使用的签名算法。
    
### 有效负载 
有效负载是包含声明的令牌的正文。索赔是为特定实体提供的一条信息。在 JWT 中，有已注册的声明，这些声明是由 JWT 标准预定义的声明以及公共或私有声明。public 和 private 声明是由开发人员定义的声明。了解公共索赔和私人索赔之间的区别是值得的，但不是为了安全目的，因此这不会是我们在这个房间里的重点。
    
### 签名 
签名是令牌的一部分，它提供验证令牌真实性的方法。签名是使用 JWT 标头中指定的算法创建的。让我们深入了解一下主要的签名算法。

## 签名算法
### 无 
无算法表示不用于签名的算法。实际上，这是一个没有签名的 JWT，这意味着无法通过签名验证 JWT 中提供的声明的验证。

### 对称签名 
对称签名算法（如 HS265）通过在生成哈希值之前将密钥值附加到 JWT 的标头和正文来创建签名。签名验证可以由任何知道密钥的系统执行。

### 非对称签名
非对称签名算法（如 RS256）通过使用私钥对 JWT 的标头和正文进行签名来创建签名。这是通过生成哈希值，然后使用私钥加密哈希值来创建的。签名验证可以由任何系统执行，只要系统知道与用于创建签名的私钥关联的公钥。

## JWT 可以加密（称为 JWE）
## 敏感信息泄露
### 练习1
让我们看一个实际示例。让使用以下 cURL 请求对 API 进行身份验证：

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password1" }' http://10.10.248.106/api/v1.0/example1
```

这将为您提供 JWT 令牌。恢复后，解码 JWT 的正文以发现敏感信息。您可以手动解码正文，也可以使用 JWT.io 等网站进行此过程。

![image.png](https://s2.loli.net/2025/05/12/vj3H5iy4wdTrNfB.png)


![image.png](https://s2.loli.net/2025/05/12/AyYv3BKjOcV9eM5.png)

### 修复
不应将密码或标志等值添加为声明，因为 JWT 将发送到客户端。

## 签名验证错误
### 未验证签名
签名验证的第一个问题是没有签名验证。如果服务器不验证 JWT 的签名，则可以将 JWT 中的声明修改为您希望的任何值。虽然不执行签名验证的 API 并不常见，但 API 中的单个端点可能省略了签名验证。根据终端节点的敏感度，这可能会对业务产生重大影响。

### 练习2
让我们对 API 进行身份验证：

```
curl -H 'Content-Type: application/json' -X POST -d '{ "username" : "user", "password" : "password2" }' http://10.10.248.106/api/v1.0/example2
```

通过身份验证后，让我们验证我们的用户：

```
curl -H 'Authorization: Bearer [JWT Token]' http://10.10.248.106/api/v1.0/example2?username=user
```

但是，让我们尝试在没有签名的情况下验证我们的用户，删除 JWT 的第三部分（只留下点）并再次发出请求。这意味着签名未被验证。将有效负载中的 admin 声明修改为 1，并尝试以 admin 用户身份进行验证以检索您的标志。


![image.png](https://s2.loli.net/2025/05/16/4gQLBG25DhMP9Nk.png)


![image.png](https://s2.loli.net/2025/05/16/QpY4GFgdjerN2wv.png)


#### 修复
降级为无

一个常见问题是签名算法降级。JWT 支持 None 签名算法，这实际上意味着 JWT 不使用签名。虽然这听起来很愚蠢，但标准中这背后的想法是用于服务器到服务器的通信，其中 JWT 的签名在上游进程中得到验证。因此，不需要第二个服务器来验证签名。但是，假设开发人员不锁定签名算法，或者至少拒绝 None 算法。在这种情况下，您只需将 JWT 中指定的算法更改为 None，这将导致用于签名验证的库始终返回 true，从而允许您再次伪造令牌中的任何声明。

JWT 支持 None 算法，该算法跳过签名验证。如果开发人员没有限制算法，攻击者可以将 JWT 标头中的 alg 声明更改为 None，从而完全绕过签名检查

### 练习三
向 API 进行身份验证以接收您的 JWT，然后验证您的用户。要执行此攻击，您需要手动将标头中的 alg 声明更改为 None。您可以使用 CyberChef 使用 URL 编码的 Base 64 选项。再次提交 JWT 以验证它是否仍被接受，即使签名不再有效，因为已进行更改。然后，您可以更改 admin 声明以恢复标志

![image.png](https://s2.loli.net/2025/05/16/2TFejqg7AudpmNv.png)

![image.png](https://s2.loli.net/2025/05/16/6SsfOP5jncxipJ8.png)


![image.png](https://s2.loli.net/2025/05/16/6dbyNAieLmghZvC.png)


如果应支持多个签名算法，则应将支持的算法作为数组列表提供给 decode 函数
![image.png](https://s2.loli.net/2025/05/16/vLf3gSYZxeM9XWU.png)


### 练习四
弱对称密钥
当使用弱 JWT 密钥时，会出现此问题。当开发人员匆忙或从示例中复制代码时，通常会发生这种情况。
![image.png](https://s2.loli.net/2025/05/16/y1Dxfzqr52pRoiW.png)

![image.png](https://s2.loli.net/2025/05/16/KFYHzUlGf2PBACL.png)

#### 修复
应选择 Secure secret 值。由于此值将在软件中使用，而不是由人类使用，因此应该使用一个长而随机的字符串作为密钥

