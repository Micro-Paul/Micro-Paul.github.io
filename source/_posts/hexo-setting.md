---
title: 个人博客搭建（hexo+GitHub）
date: 2018-10-19 21:47:44
categories:  # 这里写的分类会自动汇集到 categories 页面上，分类可以多级
- 实用技术 # 一级分类
- 个人博客 # 二级分类 
tags:   # 这里写的标签会自动汇集到 tags 页面上
- 实用 # 可配置多个标签，注意格式
- 个人博客 # 可配置多个标签，注意格式
---
在使用hexo+GitHub搭建个人博客的时候可以参考这个[链接](https://www.jianshu.com/p/380290deb8f0)，和这个[链接](https://asdfv1929.github.io/categories/GitBlog/)在这篇文章中作者详细介绍了搭建博客的详细过程，我写这片博客主要是以后迁移方便，以及还有一些需要设置的地方。下面详细介绍：

## 本地环境配置

### 需要软件

搭建博客首先需要基本的环境与软件，分别是:
> 1. Git
> 2. Node.js
> 3. hexo

网上关于这三种工具的安装有很多例子，并且在上面提到的连接里面也有介绍，本文就不多加叙说。

参考这个[链接](https://www.jianshu.com/p/380290deb8f0)

## 主题配置

### Next主题配置

hexo启动以后默认的主题是landscape，我这边选用Next主题，主要是因为这个主题看着美观大方。大部分的配置在参考文件中已经说明了，我这里只说一些需要注意的地方，以及高级的一些配置。

#### 图片的存放于使用

图片的使用只在基本上在三个地方使用，a.头像，b.支付图片，c.文章图片。

因为主题的配置文件_config.yml是在注意文件夹里面的一级目录，与source文件夹属于同一级别。但是，在配置图片的时候，图片的配置路径与source里面的目录是一致的

将头像放置主题目录下的 source/uploads/ （新建 uploads 目录若不存在） 配置为：
`
avatar: /uploads/avatar.png
`

或者 放置在 source/images/ 目录下配置为：
`
avatar: /images/avatar.png
`

#### 头像设置为圆形，且可以旋转

设置头像:
打开`themes/next/_config.yml`找到`avatar: /uploads/images/avatar.jpg;`其中images文件在`themes/nextsource/uploads`中,将你的头像图片放到images中,一般默认命名为avatar,记得改下后缀就可以了。

设置旋转效果:
打开`themes\next\source\css\_common\components\sidebar\sidebar-author.styl,`
添加以下注释代码:
	```css
		.site-author-image {
		  display: block;
		  margin: 0 auto;
		  padding: $site-author-image-padding;
		  max-width: $site-author-image-width;
		  height: $site-author-image-height;
		  border: $site-author-image-border-width solid $site-author-image-border-color;
		
		   /* 头像圆形 */
		  border-radius: 80px;
		  -webkit-border-radius: 80px;
		  -moz-border-radius: 80px;
		  box-shadow: inset 0 -1px 0 #333sf;
		
		  /* 鼠标经过头像旋转时间 */
		  -webkit-transition: -webkit-transform 1.0s ease-out;
		  -moz-transition: -moz-transform 1.0s ease-out;
		  transition: transform 1.0s ease-out;
		
		
		}
		
		 img:hover {
		  /* 鼠标经过停止头像旋转 
		  -webkit-animation-play-state:paused;
		  animation-play-state:paused;*/
		
		  /* 鼠标经过头像旋转360度 */
		  -webkit-transform: rotateZ(360deg);
		  -moz-transform: rotateZ(360deg);
		  transform: rotateZ(360deg);
		}
	```
#### 为首页文章添加阴影边框效果
打开`next\source\css\_custom\custom.styl`文件,添加以下代码:
	```css
		// 主页文章添加阴影效果
		 .post {
		   margin-top: 60px;
		   margin-bottom: 60px;
		   padding: 25px;
		   -webkit-box-shadow: 0 0 5px rgba(202, 203, 203, .5);
		   -moz-box-shadow: 0 0 5px rgba(202, 203, 204, .5);
		  }
	```

#### 在每篇文章末尾统一添加“本文结束”标记	
效果如下:

![0.png](/uploads/images/0.png)

实现方法如下:

1.  在`\themes\next\layout\_macro`下新建 `passage-end-tag.swig` 文件,并添加以下代码：

	```javascript
			<div>
			{% if not is_index %}
				<div style="text-align:center;color: #ccc;font-size:14px;">
					-------------本文结束<i class="fa fa-heart"></i>感谢阅读-------------
			   	</div>
			{% endif %}
			</div>
	```

2. 打开`\themes\next\layout\_macro\post.swig`文件，在post-body 之后， post-footer （post-footer之前有两个DIV）之前添加如下代码：

	```javascript
			<div>
			  {% if not is_index %}
			    {% include 'passage-end-tag.swig' %}
			  {% endif %}
			</div>
	```

   ![1.png](/uploads/images/1.png)

3. 打开主题配置文件_config.yml在末尾添加以下字段：

		passage_end_tag:
		  enabled: true

到此,就大功告成了.
#### 实现文章字数统计和阅读预估时间
效果如下:

![2.png](/uploads/images/2.png)

实现方法如下:
1. 在站点根目录下使用GitBash命令安装 hexo-wordcount插件:

    > $ npm install hexo-wordcount --save

2. 在全局配置文件_config.yml中激活插件:
		
	```css
		ymbols_count_time:
		    symbols: true
		    time: true
		    total_symbols: true
		    total_time: true
	```
3. 在主题的配置文件_config.yml中进行如下配置:
	```css
  		ymbols_count_time:  
			time_minutes: true
			separated_meta: true
			item_text_post: true
			item_text_total: true
			awl: 4
			wpm: 275
	```

到此,我们就实现了文章字数统计和预估时间的显示功能

#### 集成gitalk评论系统
集成gitalk系统参考这个[链接](https://asdfv1929.github.io/2018/01/20/gitalk/)

参考这个[链接](https://www.jianshu.com/p/380290deb8f0)

## hexo 配置与使用

### 发表文章
hexo主题配置好以后，就剩下发表文章了。打开命令行，定位到 xxx.github.io 目录下：

1. 新建普通页面命名为hexo-setting
 > $ hexo new hexo-setting

这时在_post文件夹下生成一个hexo-setting.md文件
2. 新建文件夹，文件夹命名为tags
 > $ hexo new page "tags"

这时在source目录下生成一个tags文件夹，且默认在此文件夹下生成 index.md 文件

### 基本配置简析

hexo 默认有categories、tags等目录，但是在source目录下没有此文件夹，所以用使用categories时，要先使用`hexo new page "categories" 命令建立此文件夹，然后在默认的此文件夹下的index.md文件中添加一下内容：

	title: 所有分类
	date: 2018-10-22 17:16:58
	type: "categories" # 将页面的类型设置为 categories
	comments: false # 如果有启用多说 或者 Disqus 评论，默认页面也会带有评论。需要关闭的话，设置为 false
同理于tags,只需要把categories换成tags就可。如果想在添加其他文件夹或者菜单的话，需要现在配置文件中配置菜单，然后建立文件夹，再去操作。
