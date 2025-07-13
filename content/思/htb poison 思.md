---
UID: 20250615201615 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-06-15
---

思路很清晰的一个靶场，主页是一个输入命令可以查看 php 文件。bp 抓包发现有文件包含的漏洞，用伪协议找到 passwd 的 txt 文件，解密之后登录进去。找到两个端口
这个是个难点，ssh 转发端口，发现是 vnc。这个实在是不会，只能看了 wp

##note


