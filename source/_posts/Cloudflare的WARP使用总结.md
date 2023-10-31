---
title: Cloudflare的WARP使用总结备忘
date: 2023-04-26 13:26:21
updated: 2023-04-26 13:28:56
permalink: '/cloudflare-warp/'
tags: ['cloudflare','WARP']
---

[Cloudflare WARP](https://1.1.1.1/) 可以做什么就不赘述了，这里记录下使用过程中的一些问题。

### 1. 流量的获取

流量的获取有3种方法

1. 通过这个 [TG机器人](https://t.me/generatewarpplusbot) 获取，按照这个机器人的提示操作即可，拿到Key后到WARP里换上，有24PB流量，几乎等于无限了。
2. 使用 [Zero Trust](https://one.dash.cloudflare.com/)， 开通Zero Trust，免费方案就可以，设置好团队域名，进入 My Team 设置邮箱后缀，然后在WARP里面登录账号即可。
   一个团队可以免费50个成员，流量无限。
   附：团队域名修改 `/settings/general`; 邮箱后缀修改 `/settings/devices/edit`;
3. 用分享的方式来获得，这个量少且费劲。

### 2. 客户端的使用

WARP有安卓、IOS、Windows、MacOS、Linux、等客户端，目前试过 MacOS、IOS、安卓，前两者非常简单，直接设置使用即可，安卓麻烦点。

#### 安卓客户端遇到的问题

1. 报错后无法连接
   猜测是由于无法拉取配置信息导致的，网上看到解决方法是扶墙，首次打开时将WIFI的代理设置为电脑上的代理，然后再打开WARP，大家可以试下，我试过不行，虽然没再报错闪退，但一直卡在获取信息。
   最终是改为使用 Zero Trust 的方式，然后就可以正常使用了。
2. 无法使用YouTube，这个是WARP本身的限制，安卓版可以排除应用，YouTube属于软件固定写死被排除的，所以无法使用。
   大家可以自行尝试使用三方客户端绕过限制。
   这个问题IOS上没有，应该是因为IOS上的网络安全策略更高无法实现，反而少了这个限制。

就这些。
