# geek

hexo theme [Demo](http://www.geekcode.me)

主题默认是用ejs模板，采用了sass，所以需要另外安装hexo-renderer-sass

```
 npm install --save hexo-renderer-sass
```

安装说明
  首先cd到hexo项目的theme目录下，克隆项目到本地

```
 git@github.com:geekwill/geek.git
```

  修改项目_config.yml文件的theme
```
theme: geek
```

设置多说评论组件，增加下边代码要_config.yml文件

```
duoshuo:
  enable: true
  short_name: willblogsir
```

设置menu菜单，还是增加代码到_config.yml文件
```
menu:
  About: /about
```

增加或修改其他设置
```
title: Will | Will的个人博客站点
description: code，学历，交流，想法，随笔，思考，感叹，瞬间，笔记...
author: Will
favicon: /favicon.ico
appicon: /app-icon.png
url: http://www.geekcode.me
