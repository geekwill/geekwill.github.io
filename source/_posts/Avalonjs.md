---
title: 初识avalonjs
date: 2015-12-16 14:56:11
tags:
  - avalon
categories: avalon
---
#### 什么是数据双向绑定？

通俗的讲就是，当我们修改了后台的数据模型之后，前端对应的数据展示就会自动刷新，并修改数据的显示，听起来是不是很爽？可现实是真的很爽，因为我们不需要再去反复的处理Dom的操作啦，只需要修改后台的数据就好啦，不去操作Dom就意味着我们可以提高页面的效率啦，那是不是也免去了F5啦，哈哈，纯属个人理解，勿喷，如果你觉得有误导你，可以@我，立刻修改。

既然说了是视图跟数据模型的双向绑定，那肯定是少不了关联的，那要这么关联呢，avalon里使用ms-duplex来标识绑定的数据模型的，我们只需要将需要绑定的html元素上添加ms-duplex="text"，这样的话就可以啦。

<!-- more -->

首先我们先来创建一个avalon的模型
例如：
```html
//html code
<form action="/" ms-controller="form">
    <input type="text" name="username" ms-duplex-boolean="data.username" />
    <input type="text" name="text" ms-duplex-boolean="text" />
    <label>{{data.username}}</label>
    <label>{{text}}</label>
</form>

//js code
<script type="text/javascript">
var vm_form = avalon.define({
  $id: 'form',
  text: '',
  data:{
    username: ''
  }
});
</script>
```
然后下边这句代码你该熟悉了吧ms-duplex="text"，这里是绑定了data对象里的username字段，还可以绑定对象属性，是不是更吊
```html
<input type="text" name="username" ms-duplex="data.username" />
```
然后我们现在在文本框输入的话，对应的label标签是不是把数据同步显示啦，是不是很赞，
再次说下：这句代码是用来显示对应viewmodel的数据的，在avalon里使用双大括号标识的，如果跟我们的项目有冲突的话，是可以改的哟，到后边我们用到我的也会讲下的。

然后我们在添加点事件验证下我们后台的数据是不是真的变啦，（其实是想简单的提下avalon的事件处理），so，让我们在viewmodel对象里添加一个按钮的事件处理的函数，然后在函数里console.log我们的数据对象，像这样：
```js
var vm_form = avalon.define({
  $id: 'form',
  fnClickForm: function(){
    console.log(vm_form.$model.data);
    console.log(vm_form.$model.text);
  },
  text: '',
  data:{
    username: ''
  }
});
```
然后再form表单里添加一个按钮：
```html
<input type="button" name="submit" value="提交" ms-click="fnClickSubmit"/>
```
ms-click="fnClickSubmit"我们你们都知道了吧，ms-Click就是用来绑定单击事件的，so，你们懂，现在大家暂时知道就好，我后边会讲的，😄，大家试想下应用场景吧…….
事例全部代码：
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <title>avalon双向数据绑定</title>
  </head>
  <body>

    <form action="/" ms-controller="form">
      <input type="text" name="username" ms-duplex="data.username" />
      <label>{{data.username}}</label>
      <input type="password" name="pass" ms-duplex="data.pass"/>
      <label>{{data.pass}}</label>
      <input type="button" name="submit" value="提交" ms-click="fnClickSubmit"/>
    </form>

    <script src="//cdn.bootcss.com/jquery/1.11.3/jquery.min.js"></script>
    <script src="//cdn.bootcss.com/avalon.js/1.5.5/avalon.min.js"></script>
    <script type="text/javascript">
    $(function(){
      var _form = avalon.define({
        $id: 'form',
        fnClickSubmit: function(){
          console.log(_form.$model.data);
        },
        data:{
          username: '',
          pass: ''
        }
      });
    });
    </script>
  </body>
</html>
```
