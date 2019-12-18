---
title: Hexo写作命令大全
date: 2019-12-12 16:27:54
tags: 
- hexo常用命令
- 学习笔记
categories: hexo
top_img: https://raw.githubusercontent.com/Ricardoltao/Photo/master/nihonorway.jpg
---


# Hexo写作命令大全

## 前言

作为一名前端程序员当然要有自己的博客存，以前用 markdown 写一些自己的学习笔记，但是写完之后都不会去回顾，因为存放在电脑上回顾起来比较麻烦。所以最近在着手自己的博客项目，但是对于 hexo 的一些写作命令并不是很熟悉，所以在网上找到了一些牛人前辈写的博客，下面开始练练手吧。

## 简单介绍

在博客目录下执行如下命令新建一篇文章

```shell
$ hexo new [layout] <title>
```

如果未指定文章的布局（layout），则默认使用 `post` 布局，生成的文档存放于 `source\_posts\` 目录下，打开后使用 Markdown 语法进行写作，保存后刷新浏览器即可看到文章。

### 布局

布局可以理解为新建文档时的一个模块，基于布局生成的文档将会继承布局的样式。

Hexo 默认有三种布局：`post`、 `page` 和 `draft`，用户可以在 `scaffolds` 目录下新建文档来自定义布局格式，还可以修改站点配置文件中的 `default_layout`参数来指定生成文档时的默认布局。

![image-20191212151710263](http://q2e38owot.bkt.clouddn.com/image/study/hexo1.png)

### 文章（post）

基于 `post` 布局生成的文档存在于 `source\_posts\` 目录下，该目录下的文档会作为博客正文显示在网站中。

### 页面（page）

`page` 布局用于生成类似 **首页** 和 **归档** 这样的页面。

### 草稿（draft）

`draft` 布局用于创建草稿，生成的文档存在于 source\_drafts\ 目录中，默认配置下将不会把该目录下的文档渲染到网站中。

通过以下命令将草稿发布为正式文章：

```
$ hexo publish <title>
```

---

## 常用命令

### 简写

`hexo n "我的博客"` == `hexo new "我的博客"` #新建文章
`hexo p` == `hexo publish`
`hexo g` == `hexo generate`#生成
`hexo s` == `hexo server` #启动服务预览
`hexo d` == `hexo deploy`#部署

### 服务器

`hexo server` #Hexo 会监视文件变动并自动更新，您无须重启服务器。
`hexo server -s` #静态模式
`hexo server -p 5000` #更改端口
`hexo server -i 192.168.1.1` #自定义 IP

`hexo clean` #清除缓存 网页正常情况下可以忽略此条命令
`hexo g` #生成静态网页
`hexo d` #开始部署

---

## 插入图片

Markdown 并不会保存插入的图片资源本身，只是记录了获取资源的链接。
在 Markdown 中插入一张图片的步骤：

1. 将图片上床到图床上
2. 获取图片的连接
3. 插入到 Markdown 中

> 引用连接:
> <http://yearito.cn/posts/hexo-writing-skills.html>
> <https://segmentfault.com/a/1190000002632530>
