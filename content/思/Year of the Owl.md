---
tags:
  - 打靶
  - 渗透
---
全是骚操作和不懂的工具

onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp.txt 10.10.44.22

一个地方要是各种办法都还不通就说明这个地方有点问题 得换个思路

或者开始的时候用广度搜索 nmap把udp和tcp端口都扫一遍 

进去一层先广泛收集

接着就是snmp枚举和域渗透 snmp枚举出用户（snmp-check） 刚好有开放rdp端口 就直接hydra测试

#注意 crackmapexec winrm爆破出密码[[靶场思路#crackmapexec破解密码]]  挺好的 学了新东西 这是Windows专用

#注意 evil-winrm来登录Windows 知道账户密码

#注意 在回收站中发现sam和system的备份资料 用impacket-secretsdump提取出hash值 hash值登录admin 获得权限