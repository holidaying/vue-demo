# vue-demo
非单页面组件的vue+vue router+vue validator
主图
![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2604175-c5f7c3d28591c35c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##1、首先这是单页面(cdn引入)，非单独文件组件开发（npm构建的）。
```
<script src="https://cdn.bootcss.com/vue/1.0.24/vue.js" type="text/javascript" charset="utf-8"></script>
<script src="https://unpkg.com/vue-validator@2.1.7/dist/vue-validator.min.js"></script>
```
##2、页面布局
```
<div id="app">
        <validator name="validation1">
            <form novalidate class="validation1">
                <div class="username-field">
                    <label for="username">用户名:</label>
                    <input id="username" type="text" v-validate:username="['required']" v-model="username">
                </div>
                <div class="comment-field">
                    <label for="comment">评论:</label>
                    <input id="comment" type="text" v-validate:comment="{maxlength: 11}" v-model="comment">
                </div>
                <div class="errors">
                    <p v-if="$validation1.username.required">Required your name.</p>
                    <p v-if="$validation1.comment.maxlength">Your comment is too long.</p>
                </div>
                <input type="submit" value="提交测试" v-if="!$validation1.username.required" v-on:click="submit($validation1)">
            </form>
        </validator>
        <div class="header-bar">
            <div class="container">
                <nav-bar :items="navBar"></nav-bar>
                <hot-bar></hot-bar>
            </div>
        </div>
        <div class="main-content">
            <list-item v-for="item in listItems" :item="item"></list-item>
        </div>
        <footer-bar></footer-bar>

```
> 对应四个组件

![Paste_Image.png](http://upload-images.jianshu.io/upload_images/2604175-8a4fabe9d6e741b5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##3、启动vue
```
var example1 = new Vue({
    el: '#app',
    data: {
        listItems: [{
            message: 'Foo',
            name:"web前端",
            author:"aaaa",
            place:"前端",
            url:'zhuanlan.zhihu.com',
        }, {
            message: 'Foo',
            name:"web前端",
            author:"dddd",
            place:"前端",
            url:'zhuanlan.zhihu.com',
        }],
        navBar: [
            ["问答", "专栏", "头条"],
            ["前端", "后端", "iOS", "Android", "安全", "工具", "程序员"]
        ],
        username:"",
        comment:"",
    },
    methods:
    {
        submit:function (argument) {
            if(argument.valid)
            {
                alert("用户名:"+this.username+'（评论：'+this.comment+'）');
            }
            else
            {
                alert("无效");
            }
        }
    }
  ```
##4、利用vue-validator校验
> 前提条件 
 * 1.下载vue-validator文件
 * 2.定义validator标签，并定义name属性，以便校验时使用
 * 3.form中添加novalidate属性
> 本章校验的逻辑是当username有值时显示提交按钮，没有值时显示错误的提示信息；当评论数超过11个字符时，错误信息出现。
> 当点击提交按钮时，如果username有值，同时评论数小于11个字符时，调用submit函数显示成功，否则失败
> 主要用的是v-validate:username="['required']",v-validate:comment="{maxlength: 11}" ，argument.valid等校验语法
> 当输入框的语法不符合规范时，$validation1.username.required取他们的值就为true；当所有的语法符合规范时$validation1.valid才为true
