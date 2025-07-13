---
UID: 20250529225454
aliases: 
tags:
  - 打靶
  - "#应急响应"
source: 
cssclasses: 
created: 2025-05-29
---
来看看问题
![image.png](https://s2.loli.net/2025/05/30/oQYfU9BvniOgJKM.png)


## 第一问
先去流量包找找看。直接导出对象选择 http，能找到一些有问题的命令

![image.png](https://s2.loli.net/2025/05/30/C1btjAiD5P3YJxm.png)

大概有点思路，在流量包中寻找 get 包

![image.png](https://s2.loli.net/2025/05/30/eDuNJhoUVXjtiE1.png)

能找到点疑似输入命令的地方

![image.png](https://s2.loli.net/2025/05/30/7rYp1uq3z5sfQ26.png)

发现这个 referer 有点问题，可能是个 base 64 加密

![image.png](https://s2.loli.net/2025/05/30/ICZVUo7eptf5xmX.png)

获得第一个 flag

## 第二问
利用 exp 来获得 shell
![image.png](https://s2.loli.net/2025/05/30/XAZyphekBRMcxUz.png)

![image.png](https://s2.loli.net/2025/05/30/1iPu4lXrQwHLegK.png)

获得 flag


## 第三问
![image.png](https://s2.loli.net/2025/05/30/Nnm14LoPGuOHtr3.png)


## 第四问
![image.png](https://s2.loli.net/2025/05/30/WNz1m4kEAwoPjus.png)

在 ps -aux 中发现有个可疑文件
![](https://s2.loli.net/2025/05/30/WGBHLjV2lkZrTaS.png)

有点眼瞎了 /tmp 目录一直没有看到

![image.png](https://s2.loli.net/2025/05/30/V6ANzMEdbGnRWl1.png)

获得木马之后，用取证工具 ftk imager 进行提取出木马文件，拉到微步云沙箱也能获得外联地址。

## 第五问
逆向，遇到木马的哈，搜搜 ip
在 ip 底下的就是秘钥
![image.png](https://s2.loli.net/2025/05/30/OWsSjR3NdnK4fpm.png)


第六问
其实是个新思路，导出 var 的文件夹，在其中寻找带. nginx 的字样的文件
![image.png](https://s2.loli.net/2025/05/30/qYx2QHUvoF5PzIV.png)

/var/register/system/startup/scripts/nat/File


应急响应靶场，除去正常操作之外，记得留意隐藏文件. 开头的文件，用 find 命令多找找。有镜像的话，直接可以用内存取证的工具来找木马文件，有时候找不到文件，就导出来用 vscode 寻找
##note