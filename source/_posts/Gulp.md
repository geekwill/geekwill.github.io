---
title: 不一样的可乐-gulp
date: 2015-12-02 13:56:01
tags:
  - gulp
categories: gulp
---

正式的从java跳到了前端的大坑里，哎…叹气啊，一坑又一坑，既然进来啦，那就整点“真”的吧，前端的什么模块化，自动化….等等，（臣妾学不完啊），所以喽，咱先从自动化入手吧。

到目前为止，前端比较流行的自动化工具grunt，gulp，fis，听说gulp比较容易懂，智商不行的我，心里想，靠，就你啦，然后我就用了gulp啦（主要是我怕学不会啊），每学习一门技术，我都会先去官网跟着指南走，这也是个学习的技巧吧，而且我觉得官方的指南是最好的入门教材（这个说的对），好吧，咱们开始吧。

首先进入 gulp中文网，这是国内小伙伴翻译的，赞一个，gulp官网，
咱们就来中文的吧，我英文老师请假，最好数学老师带教的，你懂的，进入首页，首先进入《入门指南》。

<!-- more -->

注：gulp是基于node的，请在操作前确认node环境是否安装成功，如果没有，左转node官网nodejs官网
1：第一步：安装gulp
**首先需要安装一个全局的gulp
```base
$ npm install --global gulp
```

2：进入项目目录，安装局部gulp
```base
$ npm install --save-dev gulp
```

3.在你项目的根目录下创建 gulpfile.js 文件
```js
var gulp = require('gulp');
gulp.task('default', function() {
  console.log('gulp');
});
```

4.运行 gulp
```base
$ gulp
```
如果不出意外的话，运行结果会在控制台输出gulp

好啦，基础的就学习完毕啦。
………………………靠？神马？没啦？

好吧，我错啦，我错啦，咱继续。
既然已经运行成功，那咱们现在来测试下功能，我为了测试，简单的新建了一个demo，目录

说明一下，js是放js的，css是放css的(地球人都知道)，gulpfile.js就不说了吧，node_modules这个目录是安装gulp插件时生成的，里边都是对应得插件代码，就不详细说啦，知道就好。

前端的优化手段，有一个很重要，就是要减少文件的http请求，所以demo里边那么多的css跟js，明显的可以优化，如果没有gulp的话，我们的解决方案就是手动使用压缩工具，压缩，合并，修改了在压缩在合并，明显的很浪费时间，所以才有了gulp，现在我们就来使用gulp自动压缩合并。

使用之前，我们需要先安装对用的插件，更多的插件请查看挂网的plugin页面搜索。我们今天用到的有js压缩gulp-uglify，css压缩gulp-minify-css，合并插件gulp-concat，以及gulp-rename修改文件名。
```js
$ npm install --save-dev gulp-uglify gulp-minify-css gulp-concat gulp-rename
```
安装成功后我们在来编辑gulpfile.js文件，
首先需要引入我们刚刚安装的插件
```js
var gulp = require('gulp'),
    uglify = require('gulp-uglify'),
    minifyCss = require('gulp-minify-css'),
    concat = require('gulp-concat'),
    rename = require('gulp-rename');
```
然后就需要创建task啦，（我的理解就是一个任务，比如，我们需要操作css，我们就可以刚做一个task，然后就是js…..）
继续操作：
```js
gulp.task('bulidCss',function(){
    //src('需要操作的源文件')
    //pipe(需要执行的任务)
    //concat('main.css')使用我们安装的concat合并所以源文件到自动新建main.css文件中
    //minifyCss()压缩css这里压缩的是main.css，因为gulp有种管道的概念，每次执行一个任务时，它会将执行后结果返回到下一个任务，所以这里是main.css
    //rename修改文件名称，basename: 'main'主文件名，suffix: '.min'添加的后缀，即：main.min.css
    //gulp.dest('src')输出最终(src)的文件到指定的目录，src(目录后不需要/哦，目录没有会自动新建)
    return gulp.src('css/*.css')
        .pipe(concat('main.css'))
        .pipe(minifyCss())
        .pipe(rename({basename: 'main', suffix: '.min'}))
        .pipe(gulp.dest('src'));
});
```
同理操作js
```js
gulp.task('bulidJs',function(){
    return gulp.src('js/*.js')
        .pipe(concat('main.js'))
        .pipe(uglify())
        .pipe(rename({basename: 'main', suffix: '.min'}))
        .pipe(gulp.dest('src'));
});
```
大功告成，现在我们来试试，
```base
$ gulp bulidCss
```
快看，src里多了个main.min.css，好吧，成功啦。

现在是不是页面就可以只引入main.min.css一个文件啦，而且是压缩的哟，体积很小的。同理js
………………………………………………..
或许有点人在想，你这样，我不还是一样需要没此都添加吗？好吧，咱有对应得方法，快看，gulp有个watch，顾名思义监控….试试试试，
```js
gulp.task('watch',function(){
    return gulp.watch(['js/*.js','css/*.css'], ['bulidCss','bulidJs']);
});
```
然后执行以下看看
```base
$ gulp watch
```
是不是就停在这里不动啦，我们来修改代码试试。
是不是一保存，他就自动执行啦，这样是不是就很愉快的修改代码啦！

最后补充一个default任务，就是在执行gulp后不带任何任务名称的时候默认执行这个，所以我们可以这样写，
```base
gulp.task('default', ['watch']);
```
这样的话，我们每次只要输入gulp，就会启动watch监视啦，好愉快啦，也祝你们编码愉快哟！
