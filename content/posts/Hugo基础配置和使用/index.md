+++
date = '2025-08-08T15:32:31+08:00'
draft = false
title = 'Hugo基础配置和使用'
categories = ['技术文章']
tags =  ["Hugo", "建站"]
+++

一、基本配置文件`hugo.toml`

```toml
baseURL = 'https://hugo-8of.pages.dev/'
languageCode = 'cn'
title = 'Binlog'
theme = 'hugo-theme-tokiwa'
# 隐藏菜单的RSS
disableKinds = ['rss']


[params]
bannerFont = "fonts/exampleFont" 
description = """
欲买桂花同载酒，终不似，少年游。
""" 

[menu]
# Shown in the side menu.
[[menu.main]]
identifier = "post"
name = "文章"
# 请求地址要与文章实际目录一致post和posts
url = "/posts/"
weight = 1
[[menu.main]]
name = "标签"
url = "/tags/"
weight = 2

[[menu.main]]
name = "分类"
url = "/categories/"
weight = 2

[[menu.main]]
identifier = "about"
name = "关于我"
url = "/about/"
weight = 3


[taxonomies]
category = "categories"
series = "series"
tag = "tags"


[markup.goldmark.renderer]
  unsafe = true


[params.social]
bilibili = "http://example.com/"
github = "https://github.com/"
gitlab = "https://gitlab.com/"
instagram = "http://example.com/"
mail = "mailto:anon@example.com"
twitter = "https://twitter.com/"
weibo = "http://example.com/"
youtube = "https://youtube.com/"
zhihu = "http://example.com/"
```



二、基本使用

1、新建文章

> hugo new content/posts/文章标题/index.md

2、编译项目

> hugo

3、git上传至github

> git add .
>
> git commit -m "修改"
>
> git push
