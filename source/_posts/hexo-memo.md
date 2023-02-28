---
title: Hexo使用备忘
date: 2023-02-27 15:51:19
updated: 2023-02-27 15:51:19
permalink: '/hexo-memo/'
tags: ['Hexo']
---
### 去除yilia主题中失效的统计访问

使用yilia主题打开网站会有个 `https://litten.me:9005/badjs/?id=1&uin=xxxxxx` 的访问一直转圈，主题作者最后维护已经是N年前，这个统计链接也早已失效
查找了一下，代码在`/themes/yilia/source-src/js/report.js`，但这里是源码，实际使用的是编译后的文件，所以更改这里并不能解决问题

#### 解决方法

查找

```javascript
192:function(e,t,n){"use strict";function o(e){var t=new RegExp("(^|&)"+e+"=([^&]*)(&|$)","i"),n=window.location.search.substr(1).match(t);return null!=n?unescape(n[2]):null}var r=n(388);if(n(197),window.BJ_REPORT){BJ_REPORT.init({id:1}),BJ_REPORT.init({id:1,uin:window.location.origin,combo:0,delay:1e3,url:"//litten.me:9005/badjs/",ignore:[/Script error/i],random:1,repeat:5e5,onReport:function(e,t){},ext:{}});var i=window.location.host,a=top===window,u=!(/localhost/i.test(i)||/127.0.0.1/i.test(i)||/0.0.0.0/i.test(i));a&&u&&BJ_REPORT.report("yilia-"+window.location.host);var l=o("f"),c="yilia-from";l?(a&&BJ_REPORT.report("from-"+l),r.set(c,l)):document.referrer.indexOf(window.location.host)>=0?(l=r.get(c),l&&a&&BJ_REPORT.report("from-"+l)):r.remove(c)}e.exports={init:function(){}}},
```

替换为

```javascript
192:function(e,t,n){},
```
