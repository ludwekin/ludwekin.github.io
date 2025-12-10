---
title: Cloudflare配置帮助手册
date: 2025-12-12T09:48:42.000Z
tags:
    - Web
category: Web
published: 2025-12-09T12:38:19.000Z
slug: cloudflare配置帮助手册
---

问题1: 用Cloudflare管理域名的时候，会发现有个橙色的选项，它打开不打开的区别？

这个橙色选项就Cloudflare 的“代理”功能”（橙色云 vs 灰色云）。

灰色云：仅 DNS，也就是仅解析域名，不走 Cloudflare 的网络。用户访问你的域名时，会直接访问你原本的服务器 IP。Cloudflare 不会提供缓存、加速、DDoS 防护等服务。适用场景就是，你希望纯粹用 Cloudflare 做 DNS 管理，但不希望流量经过它。

橙色云：代理，也就是访问请求先经过 Cloudflare 的网络。用户看不到你服务器真实 IP。优势就是开启了CDN加速，还可以缓存静态资源。