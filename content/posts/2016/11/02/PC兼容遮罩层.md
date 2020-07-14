---
title: PC兼容遮罩层
date: 2016-11-02 11:24:21
tags:
---
刚进公司时，做PC端的页面，涉及到一个遮罩，刚开始没有考虑到兼容，直接写了
```
/* css */
.pop{
    position:fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
    background:rgba(0,0,0,0.4);
}

/* html */
<div class="pop">
    /* Some Content */
</div>
```
发现ie7/ie8无法正确识别`rgba()`，之后看原先的`css`中有，是
```
/* css */
.pop{
    position:fixed;
    top:0;left:0;
    width:100%;
    height:100%;
    background:url(shade.png);
}

/* html */
<div class="pop">
    /* Some Content */
</div>
```
里面的`shade.png`是一张半透明的图片，这样的话可以兼容ie7/ie8，几个月后，想是否能够直接通过`css`直接解决，而不用加入图片，想到ie7/ie8下透明度可以使用`filter:alpha(opacity=40)`来处理，于是写了个新的
```
/* css */
.pop{
    position:fixed;
    top:0;
    left:0;
    width:100%;
    height:100%;
}
.pop .shade{
    position:absolute;
    top:0;
    left:0;
    width:100%;
    height:100%;
    background:#000;
    opacity:.4;
    filter:alpha(opacity=40);
}

/* html */
<div class="pop">
    <div class="shade"></div>
    /* Some Content */
</div>
```
经测试，在ie7-ie8都能够正常显示遮罩层