---
UID: 20250609103257
aliases: 
tags:
  - 渗透
  - 打靶
source: 
cssclasses: 
created: 2025-06-09
---

子域名

```
wfuzz -c -w /usr/share/wfuzz/wordlist/general/big.txt -u http://disguise.hmv -H 'host:FUZZ.disguise.hmv' --hl 890

```

