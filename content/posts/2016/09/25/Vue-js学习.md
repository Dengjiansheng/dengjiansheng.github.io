---
title: Vue.js学习
date: 2016-09-25 20:28:45
tags:
---
　　前端学习也有一段时间了，虽然知道自己`html+css+js`并没有完全吃透，但是总不能一直呆在原地，而且自己也想多学点新的东西，前端工程师应该学习的东西真的好多。最先看到的是`react`，之后在知呼看到有人提到了`Vue`，了解了下感觉尤大的`Vue`的学习曲线更低一些，也更加轻巧，毕竟我现在的公司对这些都没有任何兴趣=。=，工作中除了上面的三个工具，自己会使用node上面的一些工具来协助开发，真的没啥需要，总不能固步自封是吧。所以先把尤大的`Vue`先学习清楚了再说。
> 这个学习是根据 [`Vue`](vue.js/org/guide/)官方文档来进行的
> 
> 可以下载Chrome`Vue.js devtools`插件，让我们直接观察到渲染后的数据

***
## 基本实例
### 1、Hello Vue.js

这是最基本的实例了吧
```
<div id="app">
    {{message}}
</div>

<script>
var vm=new Vue({
    el:'#app',
    data:{
        message:'Hello Vue.js!'
    }
})
</script>
```
### 2、Two-way(双向) Binding
```
<div id="app">
    <p>{{message}}</p>
    <input type="text" v-model="message">
</div>

<script>
var vm=new Vue({
    el:'#app',
    data:{
        message:'Hello Vue.js!'
    }
})
</script>
```
### 3、Render a List
```
<div id="app">
    <ul>
        <li v-for="item in items">
            {{item.text}}
        </li>
    </ul>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        items:[
        {text:'One'},
        {text:'Two'},
        {text:'Three'}
        ]
    }
})
</script>
```
### 4、Handle User Input
```
<div id="app">
    <p>{{message}}</p>
    <button v-on:click="reverseMessage">Reverse Message</button>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        message:'Hello Vue.js'
    },
    methods:{
        reverseMessage:function(){
            this.message=this.message.split('').reverse().join('');
        }
    }
})
</script>
```
### 5、All Together Now
实现填写确定添加，点击X删除
```
<div id="app">
    <input type="text" v-model="newItem" v-on:keyup.enter="addItem">
    <ul>
        <li v-for="item in items">
            <span>{{item.text}}</span>
            <button v-on:click="removeItem">X</button>
        </li>
    </ul>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        newItem:'',     //虽然开始的时候没有用到，但是需要提前在data中体现
        items:[
            {text:'one'}
        ]
    },
    methods:{
        addItem:function(){
            var text=this.newItem.trim();
            if (text) {
                this.items.push({text:text});
                this.newItem='';    //clear model-newItem
            }
        },
        removeItem:function(index){
            this.items.splice(index,1);
        }
    }
})
</script>
```
## Data Binding Syntax（数据绑定语法）
### 1、Text
```
<span>Message:{{message}}</span>
// 如果只想执行一次数据插入
<span>This message will never change:{{* message}}</span>
```
### 2、Raw Html
```
<div>{{{raw_html}}}</div>   //并不建议使用，可能会被恶意使用
```
### 3、Attributes
```
<div id="item-{{id}}"></div>
```
### 4、JavaScript Expressions
类似此类的“内联js”只能有一行
```
{{number+1}}
{{ok?'yes':'no'}}
{{message.split('').reverse().join('')}}
```
### 5、Filters *这个还不是很清楚=。=留着后面看看实际的代码效果*
```
{{ message | capitalize }}
{{ message | filterA 'arg1' arg2 }}
```
### 6、Directives

`v-`开头，最常用的是`v-if`、`v-on`、`v-bind`，其中`v-bind`以及`v-on`都有缩写，分别是`:`和`@`
```
<a v-bind:href="url"></a>
<a v-on:click="doSomething">
<p v-if="greeting">Hello!</p>
```
## Computed Properties
### 1、Basic Example
```
<div id="app">
    a={{a}},b={{b}}
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        a:1
    },
    computed:{
        b:function(){
            return this.a+1;
        }
    }
})
</script>
```
当改变`vm.a`，则`vm.b`也将进行变更
### 2、Computed Property vs. $watch
`$watch`监控数据的变动，然后执行`callback`
```
<div id="app">
    {{fullName}}
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        firstName:'Foo',
        lastName:'Bar',
        fullName:'Foo Bar'
    },
})
vm.$watch('firstName',function(val){
    this.fullName=val+' '+this.lastName;
});
vm.$watch('lastName',function(val){
    this.fullName=this.firstName+' '+val;
});
</script>
```
### 3、Computed Setter
```
<div id="app">
    <p>{{firstName}}+{{lastName}}</p>
    {{fullName}}
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        firstName:'Foo',
        lastName:'Bar',
        fullName:'Foo Bar'
    },
    computed:{
        fullName:{
            // 最开始默认使用的是getter
            get:function(){
                return this.firstName+' '+this.lastName;
            },
            // 这个是setter
            set:function(newValue){
                var names=newValue.split(' ');
                this.firstName=names[0];
                this.lastName=names[names.length-1];
            }
        }
    }
})
</script>
```
## Class and Style Bindings
### 1、Binding HTML Class
虽然我们可以使用`class="{{className}}"`来设置`class`，但是官方文档中提醒我们不要`v-bind:class=""`混用
#### Object Syntax
有好几种用法
```
// #1
<div id="app" v-bind:class="{'class-a':isA,'class-b':isB}">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        isA:true,
        isB:false
    }
})
</script>

// #2
<div id="app" v-bind:class="classObject">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        classObject:{
            'class-a':true,
            'class-b':false
        }
    }
})
</script>
</body>
```
#### Array Syntax
绑定一系列的`classes`
```
<div id="app" v-bind:class="[classA,classB]">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        classA:'class-a',
        classB:'class-b'
    }
})
</script>

//
<div id="app" v-bind:class="[classA,isB?classB:'']">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        classA:'class-a',
        classB:'class-b',
        isB:true
    }
})
</script>

//
<div id="app" v-bind:class="[classA,{'class-b':isB,'class-c':isC}]">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        classA:'class-a',
        isB:true,
        isC:true
    }
})
</script>
```
### 2、Binding Inline Style
#### Object Syntax
`v-bind:style`看起来像`CSS`，但实际上是一个`JS Object`
```
<div id="app" v-bind:style="{color:activeColor,fontSize:fontSize+'px'}">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        activeColor:'red',
        fontSize:'14'
    }
})
</script>

//  看起来更加清爽的一种写法
<div id="app" v-bind:style="styleObject">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        styleObject:{
            color:'red',
            fontSize:'14px'
        }
    }
})
</script>
```
#### Array Syntax
```
<div id="app" v-bind:style="[styleObjectA,styleObjectB]">something</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        styleObjectA:{
            color:'red',
            fontSize:'14px'
        },
        styleObjectB:{
            margin:'20px',
            padding:'40px'
        },
    }
})
</script>
```
#### Auto-prefixing
嗯，这个我试了下=。=，不知为啥没有作用，下次去issues上面看看
## Conditional Rendering
### 1、v-if
```
<div id="app">
    <p v-if="ok">Yes</p>
    <p v-else>No</p>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        ok:true
    }
})
</script>
```
### 2、Template v-if
```
<div id="app">
    <template v-if="ok">
        <h1>YES</h1>
        <p>Show Yes</p>
    </template>
    <template v-else>
        <h1>NO</h1>
        <p>Show No</p>
    </template>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        ok:true
    }
})
</script>
```
### 3、v-show
```
<div id="app">
    <p v-show="ok">Yes</p>
    <p v-else>No</p>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        ok:true
    }
})
</script>
```
`v-show`是没有`<template>`语法的
### 4、v-else
必须跟在`v-if`或`v-show`的后面
### 5、Component caveat
`v-else`在使用了`v-show`的components后面使用是不会起作用的，可以使用另一种`v-show`的方式来代替，例如：
```
<div id="app">
    <custom-component v-show="ok"></custom-component>
    <p v-else>This could be a component too.</p>
</div>
//  intended with
<div id="app">
    <custom-component v-show="ok"></custom-component>
    <p v-show="!ok">This could be a component too.</p>
</div>

<script>
var vm=new Vue({
    el:'#app',  
    data:{
        ok:true
    }
})
</script>
```