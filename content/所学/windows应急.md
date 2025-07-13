# 步骤
## 1. 确认安全事件

监控系统日志: 检查Windows事件日志（Event Viewer），寻找异常事件、错误消息或警告，特别关注安全事件、登录异常等

## 2. 切断系统与网络连接
物理隔离
## 3. 收集证据和分析
镜像磁盘: 制作系统的磁盘镜像，以便后续离线分析和取证，确保原始数据不被篡

## 4.隔离受影响系统

## 5.恢复系统和监控

## 6. 事后总结

# Windows Forensics  Windows取证

## The File Allocation Table (FAT):

### Clusters:  集群
集群是 FAT 文件系统的基本存储单元。

### Directory:  目录：
目录包含有关文件标识的信息，例如文件名、起始集群和文件名长度。

### File Allocation Table:  文件分配表

File Allocation Table 是所有集群的链接列表。它包含集群的状态和指向链中下一个集群的指针。


## 总结
组成文件的位存储在 cluster 中。

文件系统上的所有文件名、它们的起始集群及其长度都存储在目录中。每
个集群在磁盘上的位置都存储在 File Allocation Table 中。

可以看到，我们从由位组成的原始磁盘开始，并对其进行组织以定义哪些位组引用磁盘上存储的文件。

## FAT12, FAT16, and FAT32
FAT 最初是使用 8 位集群寻址开发的，它被称为 FAT 结构。后来，由于需要增加存储量，引入了 FAT12、FAT16 和 FAT32

FAT12 对最多 4096 个集群 （2^12） 使用 12 位集群寻址。


FAT16 对最多 65,536 个群集 （2^16） 使用 16 位群集寻址。

在 FAT32 的情况下，用于寻址集群的实际位数为 28，因此最大集群数实际上是 268,435,456 或 2^28。

##   NTFS 文件系统
### Journaling  日记
NTFS 文件系统保留卷中元数据更改的日志。此功能可帮助系统从由于碎片整理而导致的崩溃或数据移动中恢复。此日志存储在卷根目录中的 $LOGFILE 中。因此，NTFS 文件系统称为日志文件系统。

### Access Controls  访问控制
FAT 文件系统没有基于用户的访问控制。NTFS 文件系统具有访问控制，用于定义文件/目录的所有者和每个用户的权限。

### Volume Shadow Copy  卷影复制
NTFS 文件系统使用称为卷影副本的功能来跟踪对文件所做的更改。使用此功能，用户可以还原以前的文件版本以进行恢复或系统还原。

### Alternate Data Streams  备用数据流

##  Master File Table  主文件表
NTFS 中也有一个主文件表。但是，主文件表 （MFT） 比文件分配表要广泛得多。

### $MFT
$MFT 是卷中的第一条记录。卷引导记录 （VBR） 指向它所在的集群。$MFT 存储有关卷上所有其他对象所在的集群的信息。此文件包含卷上存在的所有文件的目录。

### $LOGFILE
$LOGFILE 存储文件系统的事务日志记录。它有助于在发生崩溃时保持文件系统的完整性。

### $UsnJrnl
它代表更新序列号 （USN） 日志。它存在于 $Extend 记录中。它包含有关文件系统中已更改的所有文件的信息以及更改的原因。它也称为更改日志。


### MFT Explorer  MFT 资源管理器

#### MFTECmd.exe工具
MFTECmd 解析 NTFS 文件系统创建的不同文件（如 $MFT、$Boot 等）中的数据。
需要管理员权限

```
MFTECmd.exe -f <path-to-$MFT-file> --csv <path-to-save-results-in-csv>
```

##  已删除的文件和数据恢复

### autopsy

## Evidence of Execution 取证
### Windows Prefetch files  Windows 预回迁文件
此信息存储在位于 `C：\Windows\Prefetch` 目录中的预取文件中。

预取文件的扩展名为 `.pf`。预取文件包含应用程序的上次运行时间、应用程序运行的次数以及文件使用的任何文件和设备句柄。因此，它形成了有关最后执行的程序和文件的极好信息来源。

### Windows 10 时间表
可以使用 Eric Zimmerman 的 WxTCmd.exe 来解析 Windows 10 时间线

```
WxTCmd.exe -f <path-to-timeline-file> --csv <path-to-save-csv>
```

### Windows 跳转列表
JLECmd.exe 来解析 Jump List
```
 JLECmd.exe 的 Jumplist：

`JLECmd.exe -f <path-to-Jumplist-file> --csv <path-to-save-csv>`

在这个目录中
C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\AutomaticDestinations
```

## Shortcut Files  快捷方式文件

快捷方式文件可以在以下位置找到：

`C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\`

```
LECmd.exe -f <path-to-shortcut-files> --csv <path-to-save-csv>
```

## ie edge历史记录
访问历史记录：

`C:\Users\<username>\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat`

## USB 设备的 Setupapi 开发日志

当任何新设备连接到系统时，与该设备的设置相关的信息将存储在 `setupapi.dev.log`

```
C:\Windows\inf\setupapi.dev.log
```

## 快捷方式文件
它可以为我们提供有关卷名称、类型和序列号的信息。回顾上一个任务，可以在以下位置找到此信息：

`C:\Users\<username>\AppData\Roaming\Microsoft\Windows\Recent\`

`C:\Users\<username>\AppData\Roaming\Microsoft\Office\Recent\`