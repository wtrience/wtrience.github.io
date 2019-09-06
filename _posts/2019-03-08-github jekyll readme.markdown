---
layout:     post
title:      "jekyll"
subtitle:   "study note"
date:       2019-03-08 12:00:00
author:     "Tian"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - jekyll
    - BLOG
    stickie: true

---

## 发布文章
jekyll的所有文章都放在_posts目录下，分类的话暂时没涉及到，有兴趣的朋友可以先去看看。只需要在此目录内新建一个文件，后缀名为markdown即可。
我们新建一个文件，名为：first-post.markdown，内容如下：
```
---  
layout: post  
title: "First post"  
date: 2016-1-2 21:55:48
categories: mypost  
---  
  
>> Here is my first jekyll post  
  
+ Just for test  
* Just for Test  

I'm trying to write some code
```
注意，页面的日期由内容中的date来决定。

## jekyll目录结构分析：
生成的目录结构如下：
1、文件夹_layouts：用于存放模板的文件夹，里面有两个模板，default.html和post.html
2、文件夹_posts：用于存放博客文章的文件夹
3、文件夹css：存放博客所用css的文件夹
4、_site：jekyll自动生成的目录，为静态的页面
5、_coinfig.yml：jekyll的配置文件，里面可以定义相当多的配置参数，具体配置参数可以参照其官网

根据实际需要，可能还需要创建如下文件或文件夹：
1、 _includes:用于存放一些固定的HTML代码段，文件为.html格式，可以在模板中通过liquid标签引入，常用来在各个模板中复用如 导航条、标签栏、侧边栏 之类的在每个页面上都一样不变的内容，需要注意的是，这个代码段也可以是未被编译的，也就是说也可以使用liquid标签放在这些代码段中
2、 image和js等自定义文件夹：用来存放一些需要的资源文件，如图片或者javascript文件，可以任意命名
3、 CNAME文件：用来在github上做域名绑定的，将在后面介绍
4、 favicon.ico：网站的小图标