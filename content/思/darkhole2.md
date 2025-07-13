---
UID: 20250608133539
aliases: 
tags:
  - 打靶
  - 渗透
  - vulnhub
source: 
cssclasses: 
created: 2025-06-08
---

darkhole 是用到 git 泄露。拉取到 git 的文件之后，用 git log 和 git diff 看出作者更改的地方，这个确实我不熟悉。获得到账密直接登录。是个 sql 注入的界面，练练手注也好。
获得 ssh 账户，登录进去其实是只有定时任务那边有东西，发现用 curl 可以以 losy 的用户下执行命令，获得 losy 的账密，最后用 python 提权成 root
难是不难，对信息的搜索考究点，还有 git 命令和 curl 命令的使用

##note


