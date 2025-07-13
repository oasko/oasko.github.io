---
UID: 20250530222713
aliases: 
tags:
  - 打靶
  - 应急响应
source: 
cssclasses: 
created: 2025-05-30
---
## 第一问

我们在进程中发现一个可疑文件

![](https://s2.loli.net/2025/05/30/fs5aO1pdtGKMYcn.png)

拉到微步沙箱中分析
![image.png](https://s2.loli.net/2025/05/30/OvKPIFwp2LHYCdf.png)

找到回连域名和 ip
flag{miscsecure. com: 192.168.116.130:443}

## 第二问
在定时计划中找到一个加密过的 flag
![image.png](https://s2.loli.net/2025/05/30/a37lONDy6KnWsbf.png)

用 cyberchef 能解出

![](https://s2.loli.net/2025/05/30/29pmS4Bz8Ggfrlw.png)

flag{AES_encryption_algorithm_is_an_excellent_encryption_algorithm}


## 第三问
找隐藏起来的 flag，用 everything
![image.png](https://s2.loli.net/2025/05/30/JAGnVYrhHtUp8XB.png)


![image.png](https://s2.loli.net/2025/05/30/IlaeNuiZyFcs6qM.png)


是个压缩包，得想别的办法找到密码

看了 wp，密码就是恶意进程的名字，输入密码即可获得 flag

flag{Timeline_correlation_is_a_very_important_part_of_the_digital_forensics_process}

## 第四问 
这个要思路了，是发现了邮件 app，有上千封邮件，不知道怎么下手

优先检查有附件的，比较新的邮件

![image.png](https://s2.loli.net/2025/05/31/JAFn6sbrOC5QWYg.png)


下载附件分析一下

![image.png](https://s2.loli.net/2025/05/31/1HLrInFmkiv4N3E.png)

真是恶意文件


![image.png](https://s2.loli.net/2025/05/31/kYJ6jdFZnxu7zim.png)



![image.png](https://s2.loli.net/2025/05/31/lO7ncTzp4EJ1ejR.png)


下面还有一串 base 64 编码

![image.png](https://s2.loli.net/2025/05/31/Ru6pvt7EVXc5BSh.png)


![image.png](https://s2.loli.net/2025/05/31/C81Zoy2cjOSiBxv.png)


![image.png](https://s2.loli.net/2025/05/31/vzwgKbut2xioTOq.png)



## 第五问
![image.png](https://s2.loli.net/2025/05/31/y4Jiap8WYqKdQ9R.png)

string | nl
![image.png](https://s2.loli.net/2025/05/31/sfDc8RuWdPUkVEg.png)

能找到一串 base 64 编码的文字

![image.png](https://s2.loli.net/2025/05/31/h2sR8JBxEMPXNnZ.png)

这个就是加密的密码。我们需要修复压缩包才能获得 flag. txt，因为没有软件只能看 wp 了

flag{a1b2c3d4e5f67890abcdef1234567890-2f4d90a1b7c8e2349d3f56e0a9b01b8a-CBC}
## 第五问
后面一题也是偏 ctf 需要找到密文，用第四问给出来的秘钥进行解密就能得到 flag