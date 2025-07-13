对 tiki的漏洞利用 和Windows中恶意权限这两个很关键 

前期的端口信息收集也很关键 漏掉了一个61777端口



#### 注册表键值的作用​​

当 ​**​HKEY_LOCAL_MACHINE (HKLM)​**​ 和 ​**​HKEY_CURRENT_USER (HKCU)​**​ 的以下注册表项均被设置为 `1` 时：

- ​**​HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated​**​
- ​**​HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer\AlwaysInstallElevated​**​  
    这表示系统启用了 ​**​AlwaysInstallElevated​**​ 策略，允许 ​**​任何用户（包括低权限用户）​**​ 以 ​**​SYSTEM 权限​**​ 执行 Windows Installer（MSI）安装包

此配置通常由管理员通过组策略（`gpedit.msc`）开启，用于简化软件部署，但会带来严重安全风险。