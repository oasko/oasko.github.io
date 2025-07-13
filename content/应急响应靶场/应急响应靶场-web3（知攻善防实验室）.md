

![ip1](/images/应急响应web3/title.png)

本次目的是两个ip地址，隐藏的用户名。三个flag

还是先查看web服务的日志文件 可以发现有两个ip在搞事情

![ip1](/images/应急响应web3/ip.png)

用户在lusrmgr.msc中进行查看

![ip1](/images/应急响应web3/user.png)

flag一开始是没有思路的 但看了很多大佬的wp之后 原来是自己忘记去检查计划任务那一栏了

在计划任务中能找到flag1

![ip1](/images/应急响应web3/flagg1.png)

定时任务中有个可疑路径 指向一个脚本文件 进去查看一下

![ip1](/images/应急响应web3/flag2.png)

![ip1](/images/应急响应web3/flag33.png)

第二个flag到手

第三个flag想着登录网站 可能就有的 结果登录失败 去数据库逛逛 您猜怎么着 还真在里面 看了大佬wp发现 其实网站登录进后台也能找到flag3

![ip1](/images/应急响应web3/flag3.png)