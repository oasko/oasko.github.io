---
UID: 20250527111949 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-05-27
---

在最开始是在8080端口扫描中获得文件路径，在流量表中发现了一个新的文件路径，其中有个登陆界面，输入任何东西都会报数据库的错，这里是不太信息收集了，没有搜报错的slogin_lib.inc.php的poc进行利用，从而爆出用户和密码

提权是rbash的逃逸，说实话，这个逃逸我是没用过echo os.system("/bin/bash")，就想着是交互式的终端用的，

然后用我们找到的账户密码su切换一下，发现有个vim和nano的权限很多，用vim提权到root用户

##note


