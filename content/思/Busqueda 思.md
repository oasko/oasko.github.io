---
UID: 20250706232215 
aliases: 
tags: 
source: 
cssclass: 
created: 2025-07-06
---

扫描完端口之后，进 80 端口看看，发现就是一个 url 跳转的服务，我是在想会不会是 xss 注入或者别的，但是输入了好几个 xss 攻击语句但没有任何反应。可能是别的攻击手法。在底下找到程序和版本号，想着搜索可利用的漏洞，利用漏洞获得反弹 shell。
接着就是第二个难点，就是 gitea 仓库的代码审计，在找到 root 权限也就是 sudo -l 出现的脚本中，有个 full-checkup. sh 但他的路径是./full-checkup. sh，在任意文件夹中创建一个 full-checkup. sh 的反弹 shell 即可完成反弹，

##note


