+++
date = '2025-08-07T09:08:29+08:00'
draft = false
title = 'Cloudflare搭建hugo踩坑记录'

+++

一、Typora与Hugo图片路径引用不一致问题
https://www.cnblogs.com/liumylay/articles/17936688.html

二、搭建教程（部分参考）

https://wander1ng.com/post/hugo+cloudflare%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%8C%87%E5%8D%97/

三、官方文档

https://gohugo.io/getting-started/quick-start/

四、踩坑

运行后文章不显示

每篇文章有一个`draft = true`的属性，true代表不显示，需要改成false。`hugo server -D`指令就能显示。

baseURL = 'https://hugo-8of.pages.dev/'要改成自己的域名，否则文章无法打开

五、`hugo.toml`配置文件

```toml
baseURL = 'https://hugo-8of.pages.dev/'
languageCode = 'en-us'
title = 'BINLOG'
theme = 'hugo-theme-tokiwa'
```

