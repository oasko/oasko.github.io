这个靶场的思路还是蛮清楚的

来看看问题

![log](/images/玄机1webshell/title.png)

既然是webshell 就想想攻击者有在系统中应该是上传了文件 可能php 就用命令搜搜看 

![log](/images/玄机1webshell/find.png)

![log](/images/玄机1webshell/find1.png)

发现了一个php 虽说方法正确 但不太高效 看了别的师傅的方法 确实厉害啊

```
find / type f -name "*.php" | xargs grep "eval("

# 后面是将`find`输出的文件路径列表传递给`grep`，在文件中搜索包含`eval(`字符串的行。
```

![log](/images/玄机1webshell/findp.png)
可以找到三个webshell 我们进去看看

![log](/images/玄机1webshell/gz.png)

中间有段数字 疑似就是我们需要的flag 

科普一个小知识

哥斯拉 -webshell工具

 哥斯拉特征

- @session_start(); - 开启一个会话。
- @set_time_limit(0); - 设置脚本执行时间为无限。
- @error_reporting(0); - 关闭所有错误报告。

破案了 把github的哥斯拉项目的网站转成md5就能解第二个flag

第一问找出的第二个.开头的文件就是隐藏的webshell 把路径转成md5就能解决第三问

第四问有个不死马

![log](/images/玄机1webshell/log.png)

第一个方框的php文件 感觉有点可疑 进去看看

![log](/images/玄机1webshell/top.png)

​还真是**​PHP免杀WebShell​**​，其核心功能是通过多重加密和动态拼接绕过安全检测，最终执行任意恶意代码。 路径转成md5就能找到最后一个flag