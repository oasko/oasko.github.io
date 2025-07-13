看看简介 共八个题目
![image.png](https://s2.loli.net/2025/04/11/a9BlTFsu6PVRwSv.png)

大致看了一下八个题目 主要都是偏向Windows的事件

第一题

![image.png](https://s2.loli.net/2025/04/11/bwI1Di4ZSYhcjeX.png)

```
cut access.log  -d - -f 1 |uniq -c |sort -rn |head -100
```

![image.png](https://s2.loli.net/2025/04/12/mGfrwJ9HeQCs5RO.png)

扫描是有67和33这两个ip 但很遗憾 我是看了wp才知道的 花了五百积分 我是按照访问量的多少来决定 犯错误了 以为是67和1这两个 没怎么认真看日志

![image.png](https://s2.loli.net/2025/04/12/6vVKHarjcNIJhU7.png)


第二问 在事件中筛选4625 登录失败的事件 就能找到事件数

![image.png](https://s2.loli.net/2025/04/11/QcRtzLfdjyXJIDe.png)

找到事件数为2594

在lusrmgr.msc就能找到隐藏用户和隐藏的影子用户

![image.png](https://s2.loli.net/2025/04/11/Ry8gdzhBHpZkTLW.png)

hacker$ hackers$

由于用不了powershell 只能在事件查看器中 用xml语法来过滤出远程登录成功且登录类型为10的事件 一个一个查看 发现了有这些ip

```
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">
      *[System[(EventID=4624)]] 
      and 
      *[EventData[Data[@Name='LogonType']='10']]
    </Select>
  </Query>
</QueryList>
```

192.168.150.1&192.168.150.128&192.168.150.178



在计划任务能找到一个可疑的计划 任务是下载xiaowei.exe 所以说 这个有概率是个shell

![image.png](https://s2.loli.net/2025/04/11/O7bgMi9KDyYmsXW.png)


![image.png](https://s2.loli.net/2025/04/11/LvpZsFGxm5RN4IP.png)


在检查这个定时任务的时候 发现一个脚本 顺着路径 去查看一下这个脚本具体内容 

![image.png](https://s2.loli.net/2025/04/11/rbSqdzZG3FnV6lc.png)


真找到了个服务器和端口

好家伙 理解错意思了 在思考一番之后 突然另计 启动远程shell 然后netstat和tasklist里面查找


![image.png](https://s2.loli.net/2025/04/11/GbDKdlqctQaOrv3.png)

这个就是远程shell的连接ip和端口

185.117.118.21:4444