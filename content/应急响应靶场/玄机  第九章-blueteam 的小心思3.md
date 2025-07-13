---
UID: 20250602230845
aliases: 
tags:
  - 打靶
  - 应急响应
source: 
cssclasses: 
created: 2025-06-02
---

![image.png](https://s2.loli.net/2025/06/02/tzPlDy7UjIfnoks.png)

## 第一问和第二问和第五问
![image.png](https://s2.loli.net/2025/06/02/ahH3QM67PlrWNkc.png)

找到木马的 post 包，在报文中能找到完整 uri

```
find / type f -name "*.php" | xargs grep "eval("
```

![image.png](https://s2.loli.net/2025/06/02/o7ZmdfwxWvLksT5.png)

木马的密码是 cmd

查看定时任务也能找到

![image.png](https://s2.loli.net/2025/06/02/Vmr6nQ2EYS81pky.png)

这个 wget 后面的是第五题答案
## 第三问
注意留意 post 包的值
![image.png](https://s2.loli.net/2025/06/02/Jcgd7CktuyDZNiG.png)


## 第四问
![image.png](https://s2.loli.net/2025/06/02/eknFxNO81vwgfpo.png)

直接搜索. so 后缀的文件第一个就是我们要找的。
