# 1 代码下载
-------

```cpp
git clone git@gitee.com:gatieme/kernel-portal.git
git submodule update --init --recursive
```


```cpp
cd themes/mainroad
git checkout master
```

# 2 本地调试
-------

执行 hugo 后, 生成的 HTML 都放置在 public

```cpp
hugo
```

将代码带 public 目录一起提交之后, 重新更新部署  gitee page 即可.



# 3 Hugo 目录结构
-------

| 目录 | 描述 |
|:----:|:----:|
| archetypes | 包括内容类型，在创建新内容时自动生成内容的配置
| content    | 网站内容，全部使用markdown格式, 我们的博客放置到 post 目录下 |
| layouts    | 网站模板文件，决定内容如何呈现 |
| static | 图片、css、js 等静态资源 |
| themes | 存放主题 |
| config.toml | 是网站的主配置文件 |


content/post 目录下放置了所有要发博客的 MARKDOWN 文件, `content/post/YEAR/MONTH` 按照年份/月份 放置到对应目录.

image 放置到 static/img 目录下. 目录结构同博文类似.


# 4 新建博文
-------

catrgories 填写博文的类别. 类别建议一个
tags 填写博文相关的标签信息, 可以多个

```cpp
---

title: openEuler kernel 补丁合入规范
date: "2021-03-01"
author: openEuler kernel SIG
thumbnail:

categories:
  - "开发指南"
tags:
  - "openEuler"
  - "Document"

# thumbnail: "img/placeholder.jpg" # Thumbnail image
# lead: "Example lead - highlighted near the title" # Lead text
comments: false # Enable Disqus comments for specific page
authorbox: true # Enable authorbox for specific page
pager: true # Enable pager navigation (prev/next) for specific page
toc: true # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
sidebar: "right" # Enable sidebar (on the right side) per page
widgets: # Enable sidebar widgets in given order per page
  - "search"
  - "recent"
  - "taglist"
---
```
