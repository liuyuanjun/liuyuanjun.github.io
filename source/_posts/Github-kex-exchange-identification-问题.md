---
title: Github kex_exchange_identification 问题
date: 2023-02-27 15:54:30
updated: 2023-02-27 19:20:44
tags: ['github','ssh']
---
**Github 报错如下:**

> kex_exchange_identification: Connection closed by remote host
> fatal: Could not read from remote repository.
>
> Please make sure you have the correct access rights
> and the repository exists.

**解决方法如下：**

编辑 `~/.ssh/config` 文件

```conf
Host github.com
 Hostname ssh.github.com
 Port 443
```

**参考文档：**

> <https://stackoverflow.com/a/60994276>

---

后面频繁出现 time out，解决方法，加代理

```sh
# 必须是 github.com
Host github.com
   HostName github.com
   User git
   # 走 HTTP 代理
   # ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=8080
   # 走 socks5 代理（如小飞机 or V2xxx）
   ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
```

**参考文档：**

> <https://blog.csdn.net/HD243608836/article/details/127869482>