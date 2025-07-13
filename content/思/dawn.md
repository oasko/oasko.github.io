---
UID: 20250516144332
aliases: 
tags:
  - 打靶
  - "#vulnhub"
source: 
cssclasses: 
created: 2025-05-16
---

## ✍内容

在日志分析还是不到位 看得出来有着两个文件 但没想到定时执行这两个文件 看到定时计划 上传同名文件的反弹shell思路没想到 mysql提权也是 主要是没碰过

查看历史记录也是没想到. bash_history 找到曾经输入过的密码

mysql 获得权限
\! /bin/bash
##note
