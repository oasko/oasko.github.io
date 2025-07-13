来看看web1的靶场
![ip1](/images/应急响应web1/title.png)
这次要找的东西还是不太多

第一步是打开小皮的web服务日志 检查一下有什么可疑ip和攻击吗 

![ip1](/images/应急响应web1/shell.png)

果然 发现了一个192.168.126.1的ip传了一个webshell 刚好有显示路径 顺着路径去找找看 shell.php里面也许就有密码

![ip1](/images/应急响应web1/mima.png)

发现密码是rebeyond

隐藏的用户名就在lusrmgr.msc里面能找到

![ip1](/images/应急响应web1/user.png)

攻击者挖矿程序的矿池域名 就有点知识盲区了

看了很多师傅的wp 通过图标可以判断发现是用`pyinstaller`打包的`exe`文件 

利用 (pyinstxtractor)[https://github.com/extremecoders-re/pyinstxtractor] 进行反编译

```
python .\pyinstxtractor.py .\Kuang.exe
```

![ip1](/images/应急响应web1/fan.png)

在里面有个kuang.pyc 用在线pyc转pc搞一下

![ip1](/images/应急响应web1/yuming.png)

能得出域名 wakuang.zhigongshanfang.top