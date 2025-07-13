##  ryptsetup
- 用于管理加密设备的命令行工具。
### 常见参数
- 1.**open**：打开一个加密设备。
 
    ```bash
    cryptsetup open <device> <name>
    ```

    - `<device>`：加密设备的路径。
    
    - `<name>`：解密后的设备映射器名称。
    
- 2.**--type**：指定加密设备的类型。
  
    ```bash
    cryptsetup --type <type> open <device> <name>
    ```
    
    - `<type>`：加密设备的类型，如`luks`、`plain`等。
    
- 3.**luksOpen**：专门用于LUKS加密设备的打开命令。

    ```bash
    cryptsetup luksOpen <device> <name>
    ```


识别主机
可用于主机和用户识别的协议：

- 动态主机配置协议 ( DHCP ) 流量
- NetBIOS（NBNS）流量 
- Kerberos流量