---
title: Nginx备忘
date: 2023-03-09 09:38:10
updated: 2023-03-09 09:39:33
permalink: '/nginx-memo/'
tags: ['Nginx']
---

### Nginx 使用注意事项

- Nginx 中有个配置 `underscores_in_headers on|off;` 用于允许 header 中出现下划线，这个配置默认是`off`，所以如果 header 中出现下划线，默认情况下是会丢失的，使用中如非必要，header 中不要出现下划线，否则会丢失。实在需要下划线的话要记得改这项配置。

> 相关文档：[http://nginx.org/en/docs/http/ngx_http_core_module.html#underscores_in_headers](http://nginx.org/en/docs/http/ngx_http_core_module.html#underscores_in_headers)