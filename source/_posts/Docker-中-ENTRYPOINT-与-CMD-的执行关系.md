---
title: Docker 中 ENTRYPOINT 与 CMD 的执行关系
date: 2023-02-28 19:51:36
updated: 2023-03-01 09:34:11
permalink: '/docker-how-to-use-entrypoint-cmd/'
tags: ['docker']
---

## ENTRYPOINT 与 CMD 如何配合使用

|                            | No ENTRYPOINT              | ENTRYPOINT exec_entry p1_entry | ENTRYPOINT [“exec_entry”, “p1_entry”]          |
|----------------------------|----------------------------|--------------------------------|------------------------------------------------|
| No CMD                     | error, not allowed         | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry                            |
| CMD [“exec_cmd”, “p1_cmd”] | exec_cmd p1_cmd            | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry exec_cmd p1_cmd            |
| CMD exec_cmd p1_cmd        | /bin/sh -c exec_cmd p1_cmd | /bin/sh -c exec_entry p1_entry | exec_entry p1_entry /bin/sh -c exec_cmd p1_cmd |

**一句话抓重点：** 如果 ENTRYPOINT 设置没有使用数组方式，CMD不会被执行

---
---

## ENTRYPOINT 与 CMD 的各种覆写情况

### 在同一 buildstage 覆写时

```dockerfile
FROM busybox

CMD ["foo %d bar\n", "1"] # 被保留
ENTRYPOINT ["echo"] # 被覆盖
ENTRYPOINT ["printf"] # 被保留
```

```sh
# 构建
docker build --no-cache -t overridden .
```

```sh
# 运行
docker run overridden
foo 1 bar
```

运行镜像显示使用了：printf

### 在运行时覆写 （`--entrypoint`)

```sh
docker run --entrypoint="echo" overridden
# 没有输出 (CMD已被重置)
```

可以通过传参来设置新的CMD：

```sh
docker run --entrypoint="echo" overridden thisisthenewcommand
thisisthenewcommand
```
<!-- more -->
### 在子镜像覆写 （FROM ..)

构建扩展新镜像时，将重置：CMD

```dockerfile
FROM overridden
ENTRYPOINT ["echo"]
```

```sh
docker build --no-cache -t overridden2 .
```

运行显示 已重置CMD

```sh
docker run overridden2
# empty output (because CMD is reset)
```

### 在多阶段构建中覆盖

在多阶段构建中，情况与构建扩展镜像时情况相同（只是在单个 Dockerfile 中完成）;

```dockerfile
FROM busybox AS stage1

CMD ["foo %d bar\n", "1"]
ENTRYPOINT ["echo"]
ENTRYPOINT ["printf"]

FROM stage1
ENTRYPOINT ["echo"]
```

```sh
docker build --no-cache -t overridden3 .
```

运行镜像显示已使用，但已重置：`echo cmd`

```sh
docker run overridden3
# empty output (because CMD is reset)
```

---

**参考资料：**
<https://docs.docker.com/engine/reference/builder/#understand-how-cmd-and-entrypoint-interact>
<https://github.com/docker/docs/issues/6142#issuecomment-370368332>
