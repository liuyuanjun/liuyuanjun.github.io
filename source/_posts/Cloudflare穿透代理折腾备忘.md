---
title: Cloudflare内网穿透代理折腾备忘
date: 2023-03-04 19:24:00
updated: 2023-03-06 11:30:54
permalink: '/cloudflare-memo/'
tags: ['cloudflare','内网穿透','CDN','Tunnel','Workers']
---
## 用cloudflare尝试穿透中国移动宽带内网搭建外网可访问服务折腾记录

移动宽带价格较便宜，但是没有公网IPv4地址，所以一般的DDNS服务难以解决外网访问问题，经过一番设置，设置了IPv6 DDNS，外网可以访问了，但是访问需要客户侧网络支持IPv6，所以还是不够完美，于是开始了cloudflare折腾之旅。

### 1. 用cloudflare的免费CDN服务解决IPv4用户访问问题

这个很简单，只要把域名托管到cloudflare，然后在cloudflare的DNS设置里面添加一条A记录，指向服务器IP地址，然后在cloudflare的CDN设置里面开启CDN服务，就可以了。
折腾过程中有个坑，就是cloudflare的回源端口是有限制的，只支持几个固定的端口，如下：
|  **HTTP:** |  80 | 8080 | 8880 | 2052 | 2082 | 2086 | 2095 |
|:----------:|:---:|:----:|:----:|:----:|:----:|:----:|:----:|
| **HTTPS:** | 443 | 8443 |      | 2053 | 2083 | 2087 | 2096 |

### 2. 尝试将服务端口转为正常的HTTPS服务端口 443

国内的民用宽带80，443端口都是被封的，cloudflare CDN服务也是同端口访问，所以考虑到搞个反向代理，将服务端口转为正常的HTTPS服务端口 443。

#### 2.1 尝试使用cloudflare的Workers

这里用到了 [reflare](https://reflare.js.org/) 这个项目，这个项目是一个cloudflare的worker，一番折腾做好路由后，以失败告终。本次失败粗略得出两个结论（可能有误，求指正）：

1. cloudflare的worker不支持访问IPv6
2. cloudflare的worker不支持访问cloudflare的CDN服务

#### 2.2 使用cloudflare的Tunnel  (推荐)

没想到这个Tunnel居然非常顺利，而且速度感觉也比CDN快。
附上步骤链接：<https://zhuanlan.zhihu.com/p/591320825>
教程是群晖的，我直接用的docker-compose，大同小异自己理解下即可
还有一个差别就是cloudflare 的 Tunnel 入口有所改变，面板独立到这里<https://one.dash.cloudflare.com/>了，但操作不影响

**注意点：**

- 如果服务器内是用 nginx 的 server_name 区分服务，记得在nginx的配置里加入相应域名
