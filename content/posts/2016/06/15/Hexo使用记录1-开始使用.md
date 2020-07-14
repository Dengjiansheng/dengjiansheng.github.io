---
title: Hexo使用记录1-开始使用
date: 2016-06-15 11:28:52
tags:
---
官方首页这样描述Hexo的：快速、简洁且高效的博客框架。
>本文基于hexo官方文档以及其他网络文章及自己的使用感受

从接触前端开始，就久闻Github的大名，但是一直不懂它是做啥的，后来知道它可以作为代码仓库，并且有好多好多人在用它，苦于英文不好，跌跌碰碰的终于学会了使用github，随着前端的继续学习，总觉得需要有个记笔记以及分享自己想法的地方，于是又看到了github.io的个人博客，又开始鼓捣这些东西，又看到可以快速搭建网站的node应用，哦哈哈哈，又发现了hexo，就开始学习了，本篇记录的是Hexo使用过程中的记录以及之后遇到的问题及探索（百度或谷歌）的过程。

# 开始使用
## 安装
### 安装前提
* Node.js
* Git

*我用的是大众windows，所以后面的操作都是基于windows*

### 使用NPM安装Hexo
```
npm install -g hexo-cli
```
## 建站
### 文件夹相关
```
hexo init <文件名> 	//创建文件夹
cd <文件名>		//进入文件夹
npm install		//官方文档里有这个，但是我是么有用到，用了没反应
```
### _config.yml
站点的配置信息，例如标题、主题什么的在这都可以配置
### package.json
>应用程序的信息。EJS, Stylus 和 Markdown renderer 已默认安装，您可以自由移除。——hexo.io

### scaffolds
模板文件夹，里面默认是draft.md、page.md、post.md，反正我是直接生成post，等熟悉了以后可能会自己再创建些新的
### source
资源文件夹，存放我们资源的地方，里面只有_posts里的文会被解析，要找我们创建的文件可以在这里面找
### themes
主题文件夹，可以把github上下载的主题放在这里面，文件夹的名称要与`_config.yml`中的`theme: [themename]`的相同
## 配置
>您可以在 _config.yml 中修改大部份的配置。——hexo.io

### 网站`#Site`

 参数 | 描述 
 :------------- |:-------------
 title | 网站标题 
 subtitle | 网站副标题
 description | 网站描述 
 author | 作者名字 
 language | 网站使用的语言 
 timezone | 网站时区。Hexo 默认使用我们电脑所在的时区。时区列表。比如说：America/New_York, Japan, 和 UTC 。 

### 网址`#URL`

 参数 | 描述 | 默认值 
 :------------- |:-------------|:-------------
 url | 网址 |  
 root | 网站根目录 | 
 permalink | 文章的永久链接格式 | :year/:month:/day:/title/ 
 permalink_default | 永久链接中各部分的默认值 | 

>#### 网站存放在子目录
如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为 http://yoursite.com/blog 并把 root 设为 /blog/。——hexo.io

### 目录`#Directory`

 参数 | 描述 | 默认值 
 :------------- |:-------------|:-------------
 source_dir | 资源文件夹，这个文件夹用于存放内容（我们新建的.md文件就在这里面，用post生成的就在post，其他生成的在其他文件夹） | source 
 public_dir | 公共文件夹，这个文件夹用于存放生成的站点文件（=-=自己经常用hexo generate生成静态文件再用iis来进行本地网站测试） | public 
 tag_dir | 标签文件夹 | tags 
 archive_dir | 归档文件夹 | archives 
 category_dir | 分类文件夹 | categories 
 code_dir | include code文件夹 | downloads/code 
 i18n_dir | 国际化（i18n）文件夹 | :lang 
 skip_rend | 跳过指定文件的渲染，可以使用glob表达式来匹配路径 |

### 文章`#Writing`

参数|	描述	|默认值
 :------------- |:-------------|:-------------
new_post_name	|新文章的文件名称	|:title.md
default_layout	|预设布局	|post
auto_spacing	|在中文和英文之间加入空格（我在默认的`_config.yml`么有看到这个）	|false
titlecase	|把标题转换为 title case	|false
external_link	|在新标签中打开链接	|true
filename_case	|把文件名称转换为 (1) 小写或 (2) 大写	|0
render_drafts	|显示草稿	|false
post_asset_folder	|启动 Asset 文件夹	|false
relative_link	|把链接改为与根目录的相对位址	|false
future	|显示未来的文章	|true
highlight	|代码块的设置|

### 分类&标签`#Category & Tag`

参数	|描述	|默认值
 :------------- |:-------------|:-------------
default_category|	默认分类|	uncategorized
category_map|	分类别名	|
tag_map|	标签别名	|

### 日期/时间格式`#Date / Time format`

参数	|描述|	默认值
:------------- |:-------------|:-------------
date_format|	日期格式|	MMM D YYYY
time_format|	时间格式|	H:mm:ss

### 分页`#Pagination`

参数	|描述	|默认值
:------------- |:-------------|:-------------
per_page	|每页显示的文章量 (0 = 关闭分页功能)	|10
pagination_dir	|分页目录	|page

### 扩展 `#Extensions`
参数	|描述
:------------- |:-------------
theme	|当前主题名称。值为false时禁用主题

### 部署
*只使用GIT路过*

参数|描述
:-------------|:-------------
deploy	|部署部分的设置
type	|库的地址，例如：`https://github.com/Dengjiansheng/hexo.git`
branch	|分支名称。如果使用的是 GitHub 或 GitCafe 的话，程序会尝试自动检测。
message	|自定义提交信息

## 指令
*记录一些比较常用的，其他用了再记录*
### init
新建文件夹`hexo init <文件夹名称>`
### new
新建文章`hexo new [layout-布局] <文章名>`
### generate
生成静态文件`hexo generate`

选项|描述
:-------|:-------
-d,--deploy|文件生成后立即部署网站，需要deploy合作
-w,--watch|监视文件变动

### publish
发表草稿`hexo publish [layout] <文章名>`
### server
启动服务器`hexo server`，之后访问`http://localhost:4000/`

选项	|描述
:-------|:-------
-p, --port	|重设端口
-s, --static	|只使用静态文件
-l, --log	|启动日记记录，使用覆盖记录格式

### deploy
部署网站`hexo deploy`
### render
渲染文件`hexo render <file1>[file2]...`
### migrate
从其他博客系统迁移内容`hexo migrate <type>`
### clean
清楚缓存文件db.json以及public`hexo clean`
### list
列出网站资料`hexo list`
### version
显示Hexo当前版本`hexo version`或者`hexo -v`
