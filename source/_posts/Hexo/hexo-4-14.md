---
title: 使用hexo搭建个人博客
date: 2018-04-14 15:00:03
tags: 
- hexo
- next
- gitpage
categories: hexo个人博客
description: 使用hexo+next主题，分别在Github pages和Coding Pages上搭建个人博客。
---

## 前言

之前在简书写过学习笔记。但技术博客嘛，还是自己搭一个比较有逼格。

博客是一些静态网页，所以GitHub提供免费的git pages就能胜任。

但从头搭建一个功能丰富的博客很是麻烦。比如实现常用侧边栏的分类、标签功能等就够喝一壶了。

于是需要自动构建博客框架的工具，就能从配置、生成等杂活中解放出来，把重点放在写上。

常见的工具有hexo、jekyll等。本文使用hexo+next主题。

## Hexo介绍
> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

安装Hexo：`$ npm install -g hexo-cli`

## 常用命令 
### 创建项目

```
$ hexo init <folder>
$ cd <folder>
$ npm install
```
在_config.yml中配置网站。本文将主题配置为Next（需要下载）。

### 写作

#### 新建文章：
```
$ hexo new [layout] <title>  //layout默认为 post
```

#### 新建草稿：
```
$ hexo new draft <title>  //创建草稿
$ hexo publish [layout] <title> //发布草稿
```

#### 设置分类和标签：

Front-matter 是文件最上方以 --- 分隔的区域，用于指定个别文件的变量。
分类和标签可以在 Front-matter 中设置：
```
categories:
- Diary
tags:
- PS3
- Games
```

#### 设置文章摘要“查看更多”（Next主题适用）：

在文中使用&lt;-- more --&gt;分隔，或者在front-matter中配置description。

#### 生成、发布：
```
hexo g // 生成网页
hexo d // 部署网站，在_config.yml中配置过后可以自动部署到git上

// 上面两个命令可以简写为:
$ hexo g -d
$ hexo d -g
```
#### 本地测试：
可本地启动服务器，测试网站：
```
$ hexo s
```
启动服务器后，会监视文件变动并自动更新，刷新浏览器即可看到变化。

## 参考资料：

hexo官方文档：https://hexo.io/zh-cn/docs/
hexo常用命令笔记：https://blog.csdn.net/qq_26975307/article/details/62447489
next官方文档：http://theme-next.iissnan.com/
