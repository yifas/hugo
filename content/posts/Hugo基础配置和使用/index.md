+++
date = '2025-08-08T15:32:31+08:00'
draft = false
title = 'Hugo基础配置和使用'
categories = ['技术文章']
tags =  ["Hugo", "建站"]

+++

### 一、基本配置文件`hugo.toml`

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



### 二、基本使用

##### 1、新建文章

> hugo new content/posts/文章标题/index.md

##### 2、编译项目

> hugo

##### 3、git上传至github

> git add .
>
> git commit -m "修改"
>
> git push

### 三、其它注意事项[后面补充]

##### 1、分类和标签

> categories  配置文章分类
>
> tags 配置文章标签

```toml
date = '2025-08-08T15:32:31+08:00'
draft = false
title = 'Hugo基础配置和使用'
categories = ['技术文章']
tags =  ["Hugo", "建站"]
```

##### 2、hugo指令执行目录

> 由于太久没发布文章，忘记了执行目录 /dog
>
> 因为目前hugo项目是托管在github上的，所以需要在`.git文件夹所在目录`运行创建文章指令`hugo new content/posts/文章标题/index.md`
> hugo 当时应该是配置到了环境变量，所以直接能使用

##### 3、github的token失效，导致git push失败

> #### 1. 生成 Personal Access Token（如果还没生成）
>
> 1. 打开 https://github.com/settings/tokens
> 2. 点击 **Generate new token (classic)**
> 3. 给 Token 起个名字（比如 git-push-token）
> 4. 勾选权限（至少勾选 repo）
> 5. 点击 **Generate token**
> 6. **立即复制** 这个 token（离开页面后就再也看不到了）
>
> #### 2. 配置 Git 使用 Token
>
> **（推荐 - 让 Git 记住 Token）：**
>
> ```bash
> git remote set-url origin https://<你的用户名>:<你的PAT>@github.com/yifas/hugo.git
> ```
>
> 例如：
>
> ```bash
> git remote set-url origin https://yifas:ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@github.com/yifas/hugo.git
> ```
