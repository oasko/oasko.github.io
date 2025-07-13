来看看apache日志分析

![image.png](https://s2.loli.net/2025/04/11/enU42oFMYhsC8Z1.png)

这个是题目

来看第一问 

```
awk '{print $1}' access.log.1 | sort | uniq -c | sort -nr
```

![image.png](https://s2.loli.net/2025/04/11/56U3b2BMjlQhkEq.png)


第二问是看浏览器指纹 其实下面那个指纹占绝大部分 我们就试试 结果成了

```
grep "192.168.200.2" access.log.1 | cut -d '"' -f6
```

![image.png](https://s2.loli.net/2025/04/11/toEXdHcCz5IwevN.png)

第三问是问/index.php被访问的次数

```
cat access.log.1 | grep '/index.php'
```

能找出是27次 看了wp也是27次 我就是交不成功

第四问 一开始我是一直在6556这里 看了大佬的wp才发现 我们需要去过滤一下 因为有些可能是撞名了或者啥的假数据 得加个空格或者正则表达式来过滤


![image.png](https://s2.loli.net/2025/04/11/SGjHzI48LOgmEMl.png)

第五问 需要正则表达式过滤出

![image.png](https://s2.loli.net/2025/04/11/VHDLaKUJ2osi7Bf.png)