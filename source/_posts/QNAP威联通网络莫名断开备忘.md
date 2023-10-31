---
title: QNAP威联通网络莫名断开备忘
date: 2023-10-31 10:51:57
date: 2023-10-31 10:51:57
permalink: '/qnap-network-memo/'
tags: ['QNAP','威联通','网络']
---

## 问题描述

突然发现通过cf的tunnel访问的威联通nas无法访问了，通过ssh登录nas发现网络断开，ping不通外网，但是内网可以ping通。

## 解决方法

上网随便查了下，手动添加了网关，问题解决。
不知道重启后会不会有问题，暂时记录下。

```bash
cloudflared  | 2023-10-31T02:27:43Z INF Retrying connection in up to 1m4s connIndex=0 event=0 ip=198.41.192.67
# ping 198.41.200.63
PING 198.41.200.63 (198.41.200.63): 56 data bytes
ping: sendto: Network is unreachable
# ping baidu.com
PING baidu.com (39.156.66.10): 56 data bytes
ping: sendto: Network is unreachable
# ping 192.168.2.1
PING 192.168.2.1 (192.168.2.1): 56 data bytes
64 bytes from 192.168.2.1: seq=0 ttl=64 time=0.247 ms
64 bytes from 192.168.2.1: seq=1 ttl=64 time=0.288 ms
# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
10.0.3.0        *               255.255.255.0   U     0      0        0 lxcbr0
10.0.5.0        *               255.255.255.0   U     0      0        0 docker0
10.0.7.0        *               255.255.255.0   U     0      0        0 lxdbr0
127.0.0.0       *               255.0.0.0       U     0      0        0 lo
172.29.0.0      *               255.255.252.0   U     0      0        0 br-24716c1a29aa
172.29.4.0      *               255.255.252.0   U     0      0        0 br-c289739b34e0
172.29.8.0      *               255.255.252.0   U     0      0        0 br-3366894659cb
172.29.12.0     *               255.255.252.0   U     0      0        0 br-8ead73783fd8
172.29.16.0     *               255.255.252.0   U     0      0        0 br-98fb6438c86a
172.29.20.0     *               255.255.252.0   U     0      0        0 br-643339fca491
172.29.24.0     *               255.255.252.0   U     0      0        0 br-f4308d77c332
172.29.28.0     *               255.255.252.0   U     0      0        0 br-c4202cae4285
172.29.32.0     *               255.255.252.0   U     0      0        0 br-7926f60bf5c4
172.29.68.0     *               255.255.252.0   U     0      0        0 br-6afa60eff756
172.30.0.0      *               255.255.252.0   U     0      0        0 br-ba18fe12dc39
192.168.2.0     *               255.255.255.0   U     0      0        0 br0
# route add default gw 192.168.2.1
# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         RT-AC86U        0.0.0.0         UG    0      0        0 br0
10.0.3.0        *               255.255.255.0   U     0      0        0 lxcbr0
10.0.5.0        *               255.255.255.0   U     0      0        0 docker0
10.0.7.0        *               255.255.255.0   U     0      0        0 lxdbr0
127.0.0.0       *               255.0.0.0       U     0      0        0 lo
172.29.0.0      *               255.255.252.0   U     0      0        0 br-24716c1a29aa
172.29.4.0      *               255.255.252.0   U     0      0        0 br-c289739b34e0
172.29.8.0      *               255.255.252.0   U     0      0        0 br-3366894659cb
172.29.12.0     *               255.255.252.0   U     0      0        0 br-8ead73783fd8
172.29.16.0     *               255.255.252.0   U     0      0        0 br-98fb6438c86a
172.29.20.0     *               255.255.252.0   U     0      0        0 br-643339fca491
172.29.24.0     *               255.255.252.0   U     0      0        0 br-f4308d77c332
172.29.28.0     *               255.255.252.0   U     0      0        0 br-c4202cae4285
172.29.32.0     *               255.255.252.0   U     0      0        0 br-7926f60bf5c4
172.29.68.0     *               255.255.252.0   U     0      0        0 br-6afa60eff756
172.30.0.0      *               255.255.252.0   U     0      0        0 br-ba18fe12dc39
192.168.2.0     *               255.255.255.0   U     0      0        0 br0
# ping baidu.com
PING baidu.com (39.156.66.10): 56 data bytes
64 bytes from 39.156.66.10: seq=0 ttl=52 time=6.765 ms
64 bytes from 39.156.66.10: seq=1 ttl=52 time=6.822 ms
```
