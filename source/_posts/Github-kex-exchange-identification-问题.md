---
title: Github kex_exchange_identification 问题
date: 2023-02-27 15:54:30
tags:
---
**Github 报错如下:**

> ssh_dispatch_run_fatal: Connection to 20.205.243.166 port 22: Operation timed out
> fatal: Could not read from remote repository.
> 
> Please make sure you have the correct access rights
> and the repository exists.



**解决方法如下：**

编辑 `~/.ssh/config` 文件

```
Host github.com
 Hostname ssh.github.com
 Port 443
```

**参考文档**

> https://stackoverflow.com/a/60994276