Samba 共享、纵易受攻击的 proftpd 版本

nmap扫描可以用samba的脚本扫描

```
--script=smb-enum-shares.nse

smb-enum-users.nse
```
[[linux工具#^e7b869]]

检查共享
```
smbclient //ip/anonymous

smb的匿名登录账户密码都是anonymous
```

port 111 is access to a nfs（ network file system）

nmap脚本也可以扫描
```
 --script=nfs-ls，nfs-statfs，nfs-showmount
```
[[linux工具#^e7b869]]