这个主要是看看你的日志分析的能力 但比较复杂的日志分析的命令只能依靠ai来写

来看看问题

![log](/images/玄机1/title.png)

第一问我是 过滤出failed和root的条件 然后就能找出爆破ssh的ip 一开始是在查看auth.log的ip 但发现一直没用 就转到auth.log.1去了


```
cat auth.log.1 | grep  "Failed password" -a
```

![log](/images/玄机1/fail.png)

在这里面能找到三个ip 192.168.200.2,192.168.200.31,192.168.200.32 这三个ip 看了大佬给出的命令 也可以参照一下

```
cat auth.log.1 | grep -a "Failed password for root" | awk '{print $11}' | sort | uniq -c | sort -nr | more

```

![log](/images/玄机1/fail.png)

第二问 

```
cat auth.log.1 | grep -a "Accepted "
```

直接过滤成功的登录 就能找到目标ip

![log](/images/玄机1/accept.png)

第三问


前面我是用过滤出failed和用户 但按照这个顺序但发现是错误

看了有的师傅的wp 之后

```
cat auth.log.1 | grep -a "Failed password" |perl -e 'while($_=<>){ /for(.*?) from/; print "$1\n";}'|uniq -c|sort -nr
```

![log](/images/玄机1/3rd.png)

可以降序排列出用户

第四问 过滤出第二问的ip的爆破的日志 之前我有点弱智了 显示了19条 就直接写19条 发现是错误的 后来发现只爆破了4个用户 四个用户共19次 

![log](/images/玄机1/guolv.png)

最后一问 是问后门账户 反手就是一个/etc/passwd 发现一个test2

