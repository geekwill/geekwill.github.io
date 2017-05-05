---
title: Hexo重写博客
date: 2016-11-18 10:36:21
tags:
  - hexo
categories: hexo
---

刚刚接触博客的时候，百度一下全是github和jekyll的配对，而且当时学习前端时，看的视屏中老师页推荐jekyll的，so，就开始了jekyll的播客之旅，其实吧，体验还是不错的，毕竟有github大神在呢😄😄😄😄😄，但是在主题这方面，好像有点欠缺，然后在学习期间看的大神们的博客，各种炫酷叼炸天啊，所以，俺心动啦，经过我各种google，（这里赞扬下伟大的谷歌，恶心下伟大的天朝，当然我是砸墙而出的，你懂们的...）好到了很多不错的教程文章，然后我就参考了中文的hexo教程，（是不是有种不是好厨师的设计不是好程序员的赶脚）再然后我果断的借鉴（抄袭，美名其曰造轮子）了某大神(感谢)的主题，真心的好看，100个赞。

<!-- more -->

好吧，不扯啦，下边就简单的介绍下步骤：
详细的请参考： [hexo中文文档](https://hexo.io/zh-cn/docs/)
1：确保你已经安装完成node跟git，没有的话自行[google](https://www.google.com.hk/)

### 安装hexo
```base
npm install -g hexo-cli
```
查看下帮助命令
```base
$ hexo -v
hexo-cli: 0.1.9
os: Darwin 15.2.0 darwin x64
http_parser: 2.5.0
node: 4.2.3
v8: 4.5.103.35
uv: 1.7.5
zlib: 1.2.8
ares: 1.10.1-DEV
icu: 56.1
modules: 46
openssl: 1.0.2e
```
证明安装成功，然后查看下帮助
```base
$ hexo help
Usage: hexo <command>

Commands:
  help     Get help on Hexo.
  init     Create a new Hexo folder.
  version  Display version information.

Global Options:
  --config  Specify config file instead of using _config.yml
  --cwd     Specify the CWD
  --debug   Display all verbose messages in the terminal
  --safe    Disable all plugins and scripts
  --silent  Hide output on console

For more help, you can use 'hexo help [command]' for the detailed information
or you can check the docs: http://hexo.io/docs/
```

### 初始化hexo项目
如上显示，
* help：帮助信息，
* init：创建新的hexo目录，
* version：不解释了吧，

那我们就按照他说的创建一个新的hexo目录，执行
```base
$ hexo init blog
INFO  Copying data to ~/Documents/Workspaces/blog
INFO  You are almost done! Don't forget to run 'npm install' before you start blogging with Hexo!
```
显示如上创建成功，提示你在启动hexo之前需要执行npm install，这是因为我们要需要提前安装hexo的模块依赖，npm是啥，自己google哟....
执行：
```base
$ cd blog/ && npm install
```
等待中........安装成功后，我们再次执行帮助
```base
$ hexo h
Usage: hexo <command>

Commands:
  clean     Removed generated files and cache.
  config    Get or set configurations.
  deploy    Deploy your website.
  generate  Generate static files.
  help      Get help on a command.
  init      Create a new Hexo folder.
  list      List the information of the site
  migrate   Migrate your site from other system to Hexo.
  new       Create a new post.
  publish   Moves a draft post from _drafts to _posts folder.
  render    Render files with renderer plugins.
  server    Start the server.
  version   Display version information.

Global Options:
  --config  Specify config file instead of using _config.yml
  --cwd     Specify the CWD
  --debug   Display all verbose messages in the terminal
  --draft   Display draft posts
  --safe    Disable all plugins and scripts
  --silent  Hide output on console

For more help, you can use 'hexo help [command]' for the detailed information
or you can check the docs: http://hexo.io/docs/
```
是不是之前的多啦，我这里就说常用的几个，其他的看文档哦，
* clean 删除产生的文件和缓存
* deploy 部署你的站点
* init 创建新的hexo文件
* new 创建一个新的post（文章）
* server 启动服务

### 启动服务
那我们还按着他说的来，咱先启动起来看看叼不叼
```base
hexo server
INFO  Hexo is running at http://0.0.0.0:4000/. Press Ctrl+C to stop.
```
告诉我们hexo运行在 [http://0.0.0.0:4000/](http://0.0.0.0:4000/)，Ctrl+C可以停止服务，
那我们就进入[http://0.0.0.0:4000/](http://0.0.0.0:4000/)看看，我操真的可以了哟......

光一篇文章搞毛，我以后自己写怎么办？
好吧，Ctrl+C停止服务，新建一篇文章，
```base
$ hexo new first-post
INFO  Created: ~/Documents/Workspaces/blog/source/_posts/first-post.md
```
然后在source/_posts目录下是不是多了个同名的文件，而且是.md的哟，你可以修改一下内容，在重启server，我操，又有啦。叼。

### 部署到Github
那么问题来啦，光在我本地能看搞毛啊，自爽么，不行，大家爽才是真的爽嘛......
在执行之前，请确保你在github上已注册，并创建了对应的项目
打开根目录下的_config.yml，文件，最下边
```js
deploy:
  type:
```
修改为：
```js
deploy:
  type: git
  repository: https://github.com/***/blog.git #在仓库找到该仓库的ssh
  branch: gh-pages #分支，这里为gh-pages
```
hexo的部署是借助插件自动部署github，所以我们需要提前安装安装
```base
$ npm install hexo-deployer-git --save
```
安装成功后执行：参考文档：[部署](https://hexo.io/zh-cn/docs/deployment.html)
```base
$ hexo deploy
```
然后就可大家一起爽啦，然后发现，着主题太挫啦，然后我们看这个链接[主题大全](https://github.com/hexojs/hexo/wiki/Themes)，
然后我笑啦，然后就没有然后啦

### 安装主题
选择我们喜欢的主题，克隆主题，然后更新一下
```base
$ git clone https://github.com/steven5538/hexo-theme-athena.git themes/athena
$ cd theme/athena
$ git pull
```
在然后修改们的_config.yml文件的
```js
theme: athena
```
启动我们的server，就可以看到真的主题的，真TM好看......

### 备注
请仔细阅读根目录和主题目录下_config.yml，并尝试修改为自己的连接或者文字，比如：
```js
title: WillYuSir
author: will
```
这些就可以体现在我们的博客上啦，仔细阅读，仔细阅读，仔细阅读，重要的说三遍。
到此为止吧，更多的参考文档哟....
