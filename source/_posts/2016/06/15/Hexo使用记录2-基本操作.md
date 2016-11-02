---
title: Hexo使用记录2-基本操作
date: 2016-06-15 15:41:51
tags:
---
# 基本操作(算是自己的进阶篇=-=)
## Front-matter
>Front-matter 是文件最上方以 --- 分隔的区域，用于指定个别文件的变量——hexo.io
```
title: Hexo使用记录2-基本操作
date: 2016-06-15 15:41:51
---
```
### 预定义参数

参数	|描述|	默认值
:----|:----|:----
layout	|布局	|
title	|标题	|
date	|建立日期|	文件建立日期
updated	|更新日期|	文件更新日期
comments	|开启文章的评论功能	|true
tags	|标签（不适用于分页）|	
categories	|分类（不适用于分页）|	
permalink	|覆盖文章网址|

### 分类和标签
>只有文章支持分类和标签，您可以在 Front-matter 中设置。在其他系统中，分类和标签听起来很接近，但是在 Hexo 中两者有着明显的差别：分类具有顺序性和层次性，也就是说 Foo, Bar 不等于 Bar, Foo；而标签没有顺序和层次。——hexo.io
```
categories:
- Diary
tags:
- PS3
- Games
```

### JSON Front-matter
可以用JSON来编写Font-matter，但是要将`---`换成`;;;`
```
//将上面的用JSON来编写
"title":"Hexo使用记录2-基本操作"
"date":"2016-05-15 15:41:51"
;;;
```
## 标签插件（Tag Plugins）
呵呵哒，没学这个前我都是用markdown的语法来写引用以及代码块，有了这个又多了一个途径,这里记录我最常用的两种标签插件
{% blockquote hexo.io https://hexo.io/zh-cn/docs/tag-plugins.html Hexo %}
标签插件和 Front-matter 中的标签不同，它们是用于在文章中快速插入特定内容的插件。
{% endblockquote %}

### 引用块
在文章中插入引用，可以包括作者、来源以及标题

别名：quote
```
{% blockquote[author[,source]] [link] [source_link_title] %}
content
{% endblockquote %}
```

### 代码块
在文章中插入代码

别名：code
```
{% codeblock [title]] [lang:language] [url] [link text] %}
code snippet
{% endcodeblock %}
```

