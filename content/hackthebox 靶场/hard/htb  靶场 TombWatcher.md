---
UID: 20250618221936
aliases: 
tags:
  - 靶场
  - 打靶
  - hackthebox
source: 
cssclasses: 
created: 2025-06-18
---

## 端口扫描
![image.png](https://s2.loli.net/2025/06/18/BQMbOKHSsdJ6XUn.png)


![image.png](https://s2.loli.net/2025/06/18/uZICfyEwxz7bLpT.png)

![image.png](https://s2.loli.net/2025/06/18/tUA2FX9yTQgCwEN.png)

![image.png](https://s2.loli.net/2025/06/18/SD6bsReYA2Mrgx7.png)

![image.png](https://s2.loli.net/2025/06/18/noRCat2IFYOMylP.png)

![image.png](https://s2.loli.net/2025/06/18/szUD3oXCQGIqAda.png)


## 信息枚举
### smb
![image.png](https://s2.loli.net/2025/06/18/Ko2rF3jq4uBvMLy.png)

smbclient 但没有东西

只能用 enumlinux 4 扫描
```
enum4linux -a 10.10.11.73

enum4linux -S 10.10.11.73

```
![image.png](https://s2.loli.net/2025/06/20/jy3Pq6LGvKDuri4.png)


![image.png](https://s2.loli.net/2025/06/20/p7rSGITXEca6Cwi.png)


