---
title: Cloudflare Pages很好用
tags:
    - Web
category: Web
published: 2025-12-08T12:34:16.000Z
slug: cloudflare-pages很好用
---

兜兜转转，之前只知道有个Github Pages，知道它很好用，就是不能被国内直连。

现在又知道了一个Cloudflare Pages，据说，据说国内访问速度还行，反正比访问Github Pages要稳定。

原理也很简单，就是可以上传静态页面让Cloudflare帮你渲染。特别的是，Cloudflare Pages支持导入Github Pages仓库！也就是说你在GitHub Pages正常写你的博客，然后Cloudflare Pages也会同步帮你搭建。这个玩法就很多了。

开始配置我的hexo博客到Cloudflare。对于hexo博客，需要确保设置以下内容，Build command填npm install && npx hexo generate。Output directory填public。然后有个变量声明，对于我的hexo博客需要指定Required Node.js version为20.19.0版本。

然后网站就可以通过它生成的域名访问了。这里主播一直出错，原因是Output directory没填public而是留空了。

网站成功上线。但是问题接踵而至。国内对Cloudflare Pages有点墙，至少限速了。不开代理访问网站速度堪忧，大概需要5s才能打开。

这时候有些博主提出了一个办法，就是给Cloudflare Pages生成的访问链接比如https://ludwekin.pages.dev/ ，再套一个CNAME，也就是域名。

最保险的还是买阿里云的域名。这里我尝试使用一个免费的digitalplat二级域名。

好的，那现在我问个问题，现在国内网络，访问ludwekin.pages.dev会发生什么？

