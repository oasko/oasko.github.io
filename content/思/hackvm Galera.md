---
UID: 20250609000207
aliases: 
tags:
  - 打靶
  - 靶场
  - 渗透
source: 
cssclasses: 
created: 2025-06-09
---
hackvm Galera
Galera 这个靶场的难点在于 Galera。这是个 mysql 的插件，在自己的 mysql 中配置好参数能直接利用这个数据库进行参数的修改。
另一个难点在于，php 代码，在 mysql 中插入 php 伪协议。用 base 64 的方式输出/etc/passwd，来给我们进行 ssh 的爆破
提权是没想到的，用 w 命令​**​实时显示当前登录用户信息及系统活动状态**。发现 root 权限是 tty 20 的终端，在 dev 中找到了 root 的密码

##note

