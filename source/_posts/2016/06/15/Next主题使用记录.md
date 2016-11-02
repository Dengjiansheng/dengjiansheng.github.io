---
title: Next主题使用记录
date: 2016-06-15 16:42:08
tags:
---
# 安装NexT
* 下载主题
* 启用主题
* 验证是否正确启用

然后duang，好了

# 主题设定
## 选择「Scheme」

主题配置文件`_config.yml`的Scheme:

* Muse 默认的Scheme
* Mist
* Pisces

## 选择「界面语言」

站点配置文件`_config.yml`的language
{% codeblock %}
language: zh-Huns	//我用的是这个
{% endcodeblock %}
## 选择「菜单」
{% blockquote iisnan http://theme-next.iissnan.com/getting-started.html#menu-settings NexT %}
菜单配置包括两个部分，第一是菜单项（名称和链接），第二是菜单项的显示文本，第三是菜单项对应的图标。 NexT 使用的是 Font Awesome 提供的图标， Font Awesome 提供了 600+ 的图标，可以满足绝大的多数的场景，同时无须担心在 Retina 屏幕下 图标模糊的问题。
{% endblockquote %}
编辑主题配置文件，修改以下内容：

### 设定菜单的内容

对应的字段是`menu`。菜单内容的设置格式是：`item name: link`。其中`item name`是一个名称，这个名称并不直接显示在页面上，它将用于匹配图标以及翻译。
{% codeblock %}
menu:
  home: /
  archives: /archives
  #about: /about
  #categories: /categories
  tags: /tags
  #commonweal: /404.html
{% endcodeblock %}
NexT 默认的菜单项有（标注!的项表示需要手动创建这个页面）：

键值	|设定值	|显示文本（简体中文）
:----|:----|:----
home	|home: /	|主页
archives	|archives: /archives	|归档页
categories	|categories: /categories	|分类页!
tags	|tags: /tags	|标签页! 
about	|about: /about	|关于页面! 
commonweal	|commonweal: /404.html	|公益 404! 

### 设置菜单项的显示文本

{% blockquote iisnan http://theme-next.iissnan.com/getting-started.html#menu-settings NexT %}
设置菜单项的显示文本。在第一步中设置的菜单的名称并不直接用于界面上的展示。Hexo 在生成的时候将使用 这个名称查找对应的语言翻译，并提取显示文本。这些翻译文本放置在 NexT 主题目录下的 languages/{language}.yml （{language} 为你所使用的语言）。
{% endblockquote %}
{% codeblock 例子 %}
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  commonweal: 公益404
  something: 有料
{% endcodeblock %}

### 设定菜单项的图标

对应的字段是`menu_icons`，设定的格式是`item name: icon name`，其中`item name`与上面配置的菜单名字对应，`icon name`是Font Awesonme图标的名字,`enable`可用于控制是否显示图标，可以设置成为`false`来去掉图标。
{% codeblock 菜单图标配置 %}
menu_icons:
  enable: true
  # Icon Mapping.
  home: home
  about: user
  categories: th
  tags: tags
  archives: archive
  commonweal: heartbeat
{% endcodeblock %}
>!!对键值的大小写敏感，严格匹配

## 选择「侧栏」

好开森，因为默认的让我头晕了=-=，那个出现的动画我都看到要吐了
{% blockquote iisnan http://theme-next.iissnan.com/getting-started.html#sidebar-settings NexT %}
默认情况下，侧栏仅在文章页面（拥有目录列表）时才显示，并放置于右侧位置。可以通过修改主题配置文件中的sidebar字段来控制侧栏的行为。侧栏的设置包括两个部分，其一是侧栏的位置，其二是侧栏显示的时机。
{% endblockquote %}

###  设置侧栏的位置

修改`sidebar.position`的值，支持的选项有`left/right`
>!!目前仅 Pisces Scheme 支持 position 配置。影响版本5.0.0及更低版本。

### 设置侧栏显示的时机

修改`sidebar.display`的值，支持的选项有：

* `post` - 默认行为，在文章页面（拥有目录列表）时显示
* `always` - 在所有页面中都显示
* `hide` - 在所有页面中都隐藏（可以手动展开）
* `remove` - 完全移除

## 选择「头像」

编辑站点配置文件，新增字段`avatar`，值设置为头像的链接地址，地址的类型有：

地址|	值
:----|:----
完整的互联网 URI	|http://example.com/avtar.png
站点内的地址	|将头像放置主题目录下的 source/uploads/ （新建uploads目录若不存在）<br>配置为：avatar: /uploads/avatar.png<br><br>或者放置在 source/images/ 目录下<br>配置为：avatar: /images/avatar.png



```
测试使用
```