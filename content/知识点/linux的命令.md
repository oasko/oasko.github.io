### crontab
是 Linux 系统中用于设置周期性被执行的指令的命令
当安装完成操作系统之后，默认便会启动此任务调度命令。
Linux 任务调度的工作主要分为以下两类：
1. 系统执行的工作：系统周期性所要执行的工作，如备份系统数据、清理缓存
2. 个人执行的工作：某个用户定期要做的工作，例如每隔 10 分钟检查邮件服务器是否有新信，这些工作可由每个用户自行设置

#### 常见参数 

^42c19d

- -l :列出当前用户的定时任务。
- -e：编辑当前用户的定时任务。
- **-r**：删除当前用户的定时任务。
- **-u**：指定用户（仅限root用户使用）。

#### 举例
1. **-l**：列出当前用户的定时任务。
  
    ```bash
    crontab -l
    ```
    
2. **-e**：编辑当前用户的定时任务。

    ```bash
    crontab -e
    ```
    
3. **-r**：删除当前用户的定时任务。
 
    ```bash
    crontab -r
    ```
    
4. **-i**：在删除定时任务前进行确认。

    ```bash
    crontab -i -r
    ```
    
5. **-u**：指定用户（仅限root用户使用）。
 
    ```bash
    crontab -u username -l
    ```


**Cronjobs 是cron 守护进程**按照预定义的时间间隔自动执行的计划任务。cron 守护进程是一个后台进程，负责根据配置文件（称为 crontabs）管理 cronjobs。用户可以将自己的 crontab 文件存储在`/var/spool/cron/crontabs`目录中。主 crontab 文件`/etc/crontab`管理整个系统的 cronjobs。
### ps
ps 命令用于显示当前进程的状态，类似于 windows 的任务管理器。 ^c73690
#### 常见参数
- -**A** 列出所有的进程
- -**w** 显示加宽可以显示较多的资讯
- -**au** 显示较详细的资讯
- -**aux** 显示所有包含其他使用者的进程

#### 举例

1. -**a**：显示所有终端上的进程，不包括会话领导进程（session leaders）。

    ```bash
    ps -a
    ```
    
2. **-x**：显示没有控制终端的进程。
 
    ```bash
    ps -x
    ```
    
3. **-u**：按用户（User）格式显示进程信息，显示详细信息。

    ```bash
    ps -u username
    ```

4. **-f**：显示完整格式的进程信息，包括进程的父进程ID、启动时间等。
  
    ```bash
    ps -f
    ```
    
5. **-ef**：显示系统中所有进程的完整信息，结合了`-e`（显示所有进程）和`-f`（完整格式）。
 
    ```bash
    ps -ef
    ```

6. **-aux**：显示系统中所有进程的详细信息，结合了`-a`（所有终端上的进程）8、`-u`（详细信息）和`-x`（无控制终端的进程）。

    ```bash
    ps -aux
    ```
    
7. **-p**：显示指定进程ID的进程信息。
 
    ```bash
    ps -p 1234
    ```
    
8. **-C**：显示指定命令名称的进程信息。
 
    ```bash
    ps -C process_name
    ```
    
9. **-o**：自定义输出格式，可以指定需要显示的列。
  
    ```bash
    ps -o pid,ppid,cmd
    ```


### lsof
lsof命令是一个用于列出当前系统打开文件的命令行工具
#### 常见参数
- **-a**：与逻辑与操作符，用于组合多个条件，只有同时满足所有条件的文件才会被列出。
- **-c**：列出指定进程名相关的打开文件。
- **-d**：指定文件描述符，列出特定文件描述符的文件。
- **-g**：列出属于特定进程组的文件。
- **-i**：列出网络连接相关的文件。可以指定协议和端口。
- **-u**：列出指定用户相关的打开文件。
#### 举例

1. **列出所有打开的文件**：
    ```bash
    lsof
    ```

2. **列出特定用户打开的文件**：
 
    ```bash
    lsof -u username
    ```

3. **列出特定进程打开的文件**：
 
    ```bash
    lsof -p 1234
    ```

4. **列出特定端口上的网络连接**：
 
    ```bash
    lsof -i :80
    ```

5. **列出所有TCP连接**：

    ```bash
    lsof -i TCP
    ```

6. **列出所有UDP连接**：
    ```bash
    lsof -i UDP
    ```

7. **列出特定目录下的所有打开文件**：
    ```bash
    lsof +d /path/to/directory
    ```

8. **列出特定文件的打开情况**：
    ```bash
    lsof /path/to/file
    ```

9. **列出特定进程名相关的打开文件**：
    ```bash
    lsof -c process_name
    ```
    
10. **列出特定进程组的文件**：
    ```bash
    lsof -g 1234
    ```

### journalctl

是一个用于查询和管理 systemd 日志的命令行工具
#### 常见参数
- **-u, --unit**：指定服务单元名称，显示该服务的日志。
- **-b, --boot**：指定启动次数，显示该启动次数的日志。
	- - `-b 0`：当前启动的日志。
    
	- `-b -1`：上一次启动的日志。
    
	- `-b 1`：下一次启动的日志（通常用于调试）。

- **--since, --until**：按时间范围筛选日志。
- **-p, --priority**：按日志优先级筛选日志。
	- `err`：错误级别。
	- `warn`：警告级别。
	- `info`：信息级别。
#### 举例

1. **显示所有日志**
    ```bash
    journalctl
    ```

2. **显示特定服务的日志**
    ```bash
    journalctl -u service_name
    ```
    
    例如，显示`sshd`服务的日志：
    ```bash
    journalctl -u sshd
    ```

3. **显示特定优先级的日志**：
    ```bash
    journalctl -p priority
    ```

    例如，显示错误级别（error）的日志：
    ```bash
    journalctl -p err
    ```

4. **显示特定单元的日志**：
    ```bash
    journalctl -b boot_id
    ```

    例如，显示当前启动的日志：
    ```bash
    journalctl -b 0
    ```

5. **显示特定用户的日志**：
    ```bash
    journalctl _UID=user_id
    ```

    例如，显示用户ID为1000的日志
    ```bash
    journalctl _UID=1000
    ```

6. **显示特定主机的日志**
    ```bash
    journalctl _HOSTNAME=hostname
    ```

    例如，显示主机名为`server1`的日志
    ```bash
    journalctl _HOSTNAME=server1
    ```



## tail

### 常见参数



## mount
用于挂载文件系统的命令

### 常见参数
- **mount**：挂载文件系统。

    ```bash
    mount <source> <target>
    ```
    
    - `<source>`：要挂载的设备或文件系统。
    
    - `<target>`：挂载点目录。
    
- **-t**：指定文件系统类型。

    ```bash
    mount -t <type> <source> <target>
    ```
    
    - `<type>`：文件系统类型，如`ext4`、`ntfs`、`xfs`等。
        
- **-o**：指定挂载选项。
 
    ```bash
    mount -o <options> <source> <target>
    ```
    
    - `<options>`：挂载选项，如`ro`（只读）、`rw`（读写）、`noexec`（不允许执行文件）等。








## iptables

`iptables` 是一个用户空间工具，用于配置 `netfilter` 的规则。它允许管理员定义规则链（chains）和规则（rules），这些规则在数据包通过 `netfilter` 钩子点时被应用。

#### 常见参数

- **`-A`**：追加（Append）规则到指定链的末尾。

    ```bash
    sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
    ```

- **`-I`**：插入（Insert）规则到指定链的指定位置。

    ```bash
    sudo iptables -I INPUT 1 -p tcp --dport 80 -j ACCEPT
    ```

- **`-D`**：删除（Delete）规则。

    ```bash
    sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT
    ```

- **`-R`**：替换（Replace）指定位置的规则。
  
    ```bash
    sudo iptables -R INPUT 1 -p tcp --dport 80 -j DROP
    ```

- **`-L`**：列出（List）规则。

    ```bash
    sudo iptables -L
    ```


## nftables

`nftables` 是 `netfilter` 的新一代用户空间工具，旨在替代 `iptables`，提供更灵活、更高效的规则管理机制。

#### 常见参数

- **`add table`**：添加表（Table）。

    ```bash
    sudo nft add table ip filter
    ```

- **`add chain`**：添加链（Chain）。

    ```bash
    sudo nft add chain ip filter INPUT { type filter hook input priority 0 \; }
    ```

- **`add rule`**：添加规则（Rule）。

    ```bash
    sudo nft add rule ip filter INPUT tcp dport 80 accept
    ```

- **`delete rule`**：删除规则。
 
    ```bash
    sudo nft delete rule ip filter INPUT tcp dport 80
    ```

- **`list ruleset`**：列出所有规则。
 
    ```bash
    sudo nft list ruleset
    ```

- **`list table`**：列出表中的规则。

    ```bash
    sudo nft list table ip filter
    ```




## ss


进入ftp服务器能使用的命令有哪些


## find
`find` 命令是 Linux 和类 Unix 系统中用于查找文件和目录的强大工具，它支持非常丰富的搜索条件和选项。

### 基本语法
```bash
   find (路径) (查找条件) (操作) 
```
### 常见参数

1. 路径
- 可以从指定的路径查找
```
find /home/user
```
2. 查找条件
- -name(文件名)：根据文件名查找
```
find -name (路径) 'x.txt'
```
-  -type(类型)：根据文件类型查找
	- f   普通文件 
	- d  目录
	- l   符号链接
	- b  快符号
	- c   字符符号
```
find /path -type f #普通文件

find /path -type d #目录
```

- -size(大小)：按文件大小查找
```
find /path -size +100M #大于100mb的文件
```

- -mtime(时间)：按修改时间查找
- -atime(时间)：按访问时间查找
- -ctime(时间)：按状态改变时间查找

3. 操作逻辑符
- -and 与操作 就是并列
```
find /path -type f -and -name "*.txt" #要满足以下条件
```

- -or 或操作 就是其中一者成立
```
find /path -type f -or -name "*.txt" #要满足其中一个条件就行
```

- ! 或 -not 非操作 就是不成立
```
find /path -type f -or -name "*.txt" #要不符合这个条件的
```
4. 执行命令
- -exec (命令)  {}  \ ：对每个文件执行命令
- 基本语法
```
find /path -exec {} \
```

- 举例
- 1. 查找.txt文件并显示内容
```
find /path -name "*.txt" -exec cat {} \
#在/path路径下查找*.txt 对每个*.txt执行cat查看命令
```

- 2.查找 `.log` 文件并删除它们
```
find /path -name "*.log" -exec rm -f {} \
#在/path路径下查找*.log 对每个*.txt执行rm -f 删除命令
```

4. 按照用户名，组名查找
- -user (用户名) 按照指定用户名查找
```
find /path -user username
```

- -group(用户名) 按照指定组名查找
```
find /path -group groupname
```

5. 按照权限查找
- -perm(权限)：按照权限查找
```
find /path -perm 777 #查找权限为777的文件
```

^454b9b

## echo
1. -e：启用反斜杠转义
`-e` 选项让 `echo` 解释特殊的转义字符（如 `\n`、`\t`、`\b` 等）。默认情况下，`echo` 并不会解释这些字符，使用 `-e` 选项可以启用这一功能。
 ^54ceff
2. -E：禁用反斜杠转义（默认行为）
`-E` 显式地禁用反斜杠转义。这是 `echo` 命令的默认行为。如果不使用 `-e` 或 `-E`，`echo` 会按普通字符串输出。

3. 重定向
```
echo "This is a log entry" > log.txt
```
将 `"This is a log entry"` 写入 `log.txt` 文件。如果文件存在，内容会被覆盖；如果文件不存在，会创建该文件。

4. 追加
```
echo "Appended line" >> log.txt
```
将 `"Appended line"` 追加到 `log.txt` 文件末尾。
## sudo

`sudo -l` 是一个用于列出当前用户可以通过 `sudo` 执行的命令的命令。


## blkid

## wc
对于快速分析和统计信息收集非常有用

`wc`的输出提供有关日志文件中的行数、字数和字符数的信息。

wc 日志 可以判断文件包含几行、几 个**单词和几**个字符 ^382674

## cut
### **参数:**

#### - -b ：
以字节为单位进行分割。这些字节位置将忽略多字节字符边界，除非也指定了 -n 标志。
#### - -c ：
以字符为单位进行分割。
#### - -d ：
自定义分隔符，默认为制表符。
#### - -f ：
与-d一起使用，指定显示哪个区域。
#### - -n ：
取消分割多字节字符。仅和 -b 标志一起使用。如果字符的最后一个字节落在由 -b 标志的 List 参数指示的范围之内，该字符将被写出；否则，该字符将被排除

cut -d ' ' -f 1 apache.log
可以用来过滤出指定一个`space`字符的分隔符并仅选择返回的第一个字段。 ^248876


## snrt

-n 依照数值的大小排序。

-r 反转顺序

## `uniq`命令

^b7e319

可识别并删除排序输入中的相邻重复行。

加`-c`选项
以输出唯一行，并在每行前面添加出现次数。这对于快速确定流量异常高的 IP 地址非常有用。

## awk

^599482

对于该`awk`命令，一个常见的用例是基于特定字段值的条件操作。

## grep

^94b231
### 1.`-E`选项
以表示我们正在搜索模式而不仅仅是字符串，这使我们能够使用正则表达式。

## losf