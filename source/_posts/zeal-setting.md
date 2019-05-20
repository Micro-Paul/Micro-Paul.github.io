---
title: zeal-setting
date: 2019-05-20 09:58:20
categories:  # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
- 工具 # 一级分类
tags:   # 这里写的标签会自动汇集到 tags 页面上
- zeal # 可配置多个标签，注意格式
---
作为一名程序开发者，翻查文档成为了每天都必须去做的事情。然而，由于使用到的知识和工具不相同，所以几乎每次都需要重新打开官网的文档进行搜索，而且不同知识和工具的文档还需要切换浏览器标签来浏览，尤其是不能连接外网或者无网的情况下想要查看一些API就变得十分不方便了。

那么有没有一个工具，即可以让我们方便查阅文档，又可以把自己日常用到的资料存起来的工具呢，下面就来介绍一下 Zeal 这一离线文档工具。Mac机对应软件为：Dash。

## 关于zeal

对于zeal这款软件网上有很多介绍，这儿有一个不错的文章推介给大家，本文就不多加叙说。
参考这个[链接](https://juejin.im/post/5c6b757b6fb9a049d74840cf)

## Zeal 创建自己专属的文档项目

上边推介的文章也有对于Zeal创建自己的专属文档部分，但是苦于自己自己太浅有部分跳跃太快，没看懂，所以自己亲自把步骤分解，进行搭建。同样Zeal 官方也写了一个教我们如何去编写自己的 Docsets 的文档[链接](https://kapeli.com/docsets)，但是该文档写得比较简单，并且没有详细地操作指引，操作起来比较复杂。
>Zeal 的 Docsets 其实是 html 的集合，那么我们可以先用文档工具，生成一些静态的 html 文档。然后通过 Docsets 官方提供的 Docsets 生成器来把 html 生成 Docsets，这样就可以生成出属于我们自己的 Docsets 了。

## 合适的文档生成器

目前各种开发语言都有文档生成器，我所知道的 Node.js 生成器就有数十个像 Gitbook、Docsify、Vuepress 等等。但是并不是每一个都适合用来制作 Docsets，举个例子：
>Docsify 是一个很棒的生成器，但是用于 Docsets 的话就会有问题。原因是因为 Docsify 是通过 js 读取 Markdown 来实现的，而 Zeal 内部是一个浏览器，并没有静态服务器，所以制作出来的 Docsets 会出现跨域的问题。

最终我选择了使用 Gitbook 来制作 Docsets，它能生成静态的 Html 文件，并且能够通过本地双击打开，能够跟 Zeal 完美融合。

## 编写文档

确定了使用 Gitbook 之后，先安装 Gitbook： `npm install gitbook-cli -g`
GitBook 准备工作做好之后，我们进入一个你要写书的目录，输入如下命令。
```
$ gitbook init
warn: no summary file in this book
info: create README.md
info: create SUMMARY.md
info: initialization is finished
```

可以看到他会创建 README.md 和 SUMMARY.md 这两个文件，README.md 应该不陌生，就是说明文档，而 SUMMARY.md 其实就是书的章节目录，其默认内容如下所示：
```
# Summary

* [Introduction](README.md)
```

接下来，我们输入 `$ gitbook serve `命令，然后在浏览器地址栏中输入` http://localhost:4000` 便可预览书籍。
效果如下所示：

![gitbook_init.png](/uploads/images/gitbook_init.png "Gitbook初始化") 

运行该命令后会在书籍的文件夹中生成一个 _book 文件夹, 里面的内容即为生成的 html 文件，我们可以使用下面命令来生成网页而不开启服务器。
```
$ gitbook build
```
对于gitbook的使用就介绍到这儿，顺便给一个参考[链接](https://www.jianshu.com/p/421cc442f06c)大家可以看看gitbook的相关文档。

## 把 html 生成 Docsets

有了文档对应的 html 之后，需要把 html 生成 Docsets。我使用 Node.js 生成，在 npm 上面找了一个叫 [docset-generator](https://www.npmjs.com/package/docset-generator) 的插件。新建index.js，写入以下代码
```
let DocSetGenerator = require("docset-generator").DocSetGenerator;
let docSetGenerator = new DocSetGenerator({
  destination: "./output/",
  name: "Micro-Paul",
  documentation: "./_book",
  icon: "./images/note.png",
  entries: [ // entries 可以设置 Docsets 的分类，一般一个分类对应一个 html
    {
      name: "个人笔记",
      type: "Guide",
      path: "./index.html"
    }
  ]
});
docSetGenerator.create();
```
通过命令：
```
$ node index.js
```

即可在对应的目录下生成对应的 Docsets。接下来就是最后的一步，把这个文件夹放进 Zeal 里面。在 Zeal 里面有一个叫 docsets 的文件夹，进去之后会看到下载的 docsets 都在里面，把刚刚生成的 docsets 放进去，重启一下 Zeal 就可以看到自己的文档了。

## 结束语

以上就是我在工作过程中，根据自己的开发习惯所做出来的一个小文档，如果大家有其他方便的工具，可以一起讨论一下，有任何问题欢迎骚扰~



