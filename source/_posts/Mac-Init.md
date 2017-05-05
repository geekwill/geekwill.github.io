---
title: Mac前端初始化
date: 2016-12-05 10:36:21
tags:
  - mac
categories: Mac
---

都说，现在搞前端的都必须用Mac，为啥？因为可以狠狠的装B啊，拿着Mac去星巴克一坐，在来杯coffee，这B装的太完美啦😄，虽然哥还没来得及干过，但是想想都很爽哟。
好吧，我们今天是用来敲代码的，想去装装的，别怪哥没提醒你哈。其实Mac除了装逼，的确也很适合敲代码的，这也是为什么国外的程序员们都觉得拿着PC去敲代码很不专业的原因啦，那有人就会问，为什么，PC在中国也有很多大神用的很吊啊，不可否认，有，但是Mac比PC更效率，因为它有完美的terminal命令行，友好的UI，稳定的系统，专业的软件等等，好吧，我不喷啦，我是个从java转向前端的猿，所以今天我就把我初始化Mac到一名前端开发者环境的搭建，以及推荐给大家的软件，本文纯属个人习惯，勿喷!

<!-- more -->

### 系统设置
首先，我们拿到一个新的或者都是重装过系统的Mac电脑，对其操作的第一项就是对其设置为自己的习惯，这里我介绍我自己的一些设置做参考：
1：Dock
​ 因为Mac是宽屏的，如果把dock放在屏幕的下方的话，这样的话屏幕就会被占用很大的位置，所以我会把其放在左边（或者右边），然后为了让我开启很多软件的时候会把图标挤的越来越小，我会把所有的图标从dock上移除，这样dock就都是我打开的软件啦。
2：Finder
​ 作为开发者，我想对文件的路径和类型很想一眼就明知，所以我们需要将其默认的显示：Finder > 偏好设置

* 通用 > 开启新finder窗口时打开账户根目录（比如我的是：Will）。
* 标记 > 默认。
* 边栏 > 取消勾选我的所有文件，iCloud Drive，AirDrop，最近使用的标记。勾选用户目录（Will）。
* 高级 > 勾选显示所有文件扩展名。

​ Finder > 显示 > 显示标签页栏，显示路径栏，显示状态栏，显示边栏。
以上是我本人根据个人习惯设置，效果如下：
![](/images/2016-12-05/Mac-1.png)
### Mac神器之   HomeBrew
首先，给大家介绍一个Mac上必不可少的神器，Home Brew，为什么先要介绍它，因为我们接下来的APP都需要用它来安装。
在开始安装Home Brew 之前，请先安装苹果的Xcode，因为有很多依赖都需要它来安装，如果不想安装的话，也可以自行google单独安装，但是这里我推荐大家装，因为不光省事，而且，既然是前端开发者（本人），以后做app项目的话，你少不了模拟器的，是吧，还是装了吧。
安装homebrew [官网](http://brew.sh/)其实已经说的很清楚啦，但是还是说下吧，因为Mac自带了Ruby的环境，所以我们可以直接安装homebrew啦，大家终端执行下面的代码：
``` base
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
等待安装成功后检查是否运行正常及更新软件，执行：
``` base
$ brew doctor && brew update
```
接下来我们就使用brew安装软件啦
``` base
$ brew tap caskroom/cask && brew install brew-cask
```
`brew cask`是个神马？跟homebrew有关系吗？
`brew`是从下载源码解压然后 ./configure && make install ，同时会包含相关依存库。并自动配置好各种环境变量，而且易于卸载。
`brew cask `是 已经编译好了的应用包 （.dmg/.pkg），仅仅是下载解压，放在统一的目录中（/opt/homebrew-cask/Caskroom），省掉了自己去下载、解压、拖拽（安装）等蛋疼步骤，同样，卸载相当容易与干净。这个对一般用户来说会比较方便，包含很多在 AppStore 里没有的常用软件，[更多](https://www.zhihu.com/question/22624898/answer/22782144)

### Mac神器之   Oh-my-zsh * iTerm2
介绍iterm2之前，我们先来说说，经常使用命令行的都知道，在道上有个东西叫zsh，但是mac的终端默认是启用了bash，没有启用zsh，so，我们来修改：
``` base
$ cat /etc/shells
# List of acceptable shells for chpass(1).
# Ftpd will not allow users to connect who are not using
# one of these shells.

/bin/bash
/bin/csh
/bin/ksh
/bin/sh
/bin/tcsh
/bin/zsh
```
替换默认的bash
``` base
chsh -s /bin/zsh
```
既然有了zsh，那肯定少不了oh-my-zsh：
``` base
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/
tools/install.sh -O - | sh
```
安装成功后修改主题：
``` base
$ vim ~/.zshrc
//修改ZSH_THEME="ys" 保存退出
//重新加载
$ source ~/.zshrc
```
现在我们重启在终端是不是比以前吊很多啊，现在我们的terminal已经很不错的，但是总有一款比不错的更不错，那就属它啦，iterm 提供了高亮，自动补全.......各种屌炸天的功能等我们慢慢的琢磨[官网](http://iterm2.com/)。安装：
``` base
$ brew cask install iterm2
```
安装成功后，在iTerm>Preferences>Profiles>Default里修改自己的颜色，以及背景可以透明哟，还希望大家参考官网慢慢的探索去吧....
![](/images/2016-12-05/Mac-3.png)

### Mac神器之  Alfred2
还是那句话Mac的Spotlight现在做了相当好啦，但是还有比相当好更好的，那就是Alfred2，结合Powerpack，我们可以体验神一样的享受[官网](https://www.alfredapp.com/)，安装
``` base
$ brew cask alfred
```
既然我们有了alfred我感觉就没必要用Spotlight，那我们先来关闭Spotlight
系统设置 > 键盘 > 快捷键 > Spotlight 全部取消勾选，然后我们来修改alfred的快捷键，alfred > Preferences > General > Alfred Hotkey修改为 command + 空格。
那现在让我们体验体验：
command + 空格，输入 哈哈 回车，是不是自动打开默认的浏览器进行搜索啦，然后重新再来，输入 mail就会自动打开邮件，输入open 就会打开文件，真心的方便，瞬间就不需要鼠标了有木有。
都说啦，结合Powerpack才叫爽，但是Powerpack是收费的，我操，尼玛，唉，想想这么好的东西要钱也是很正常的，推荐购买，但是我这有那什么....你们懂的，私下的弄弄就行。
推荐个网站[有很多很好玩的哟](http://www.alfredworkflow.com/)
使用方法，下载，导入，然后 呼出Alfred输入关键字，回车，奇迹出现，举个例子：
下载这个[https://github.com/liyaodong/AlfredWorkflows](https://github.com/liyaodong/AlfredWorkflows)，导入，然后呼出Alfred，输入： say my name is will，回车，我操，他竟然最近会读哟，我操，好屌.....
![](/images/2016-12-05/Mac-4.png)

### APP安装
既然有了这么屌的电脑，so，那必须少不了牛叉的软件啦，下面我针对我日常所用的APP给大家推荐。
##### 必装类
* QQ
* Wiz(为知笔记)
* NeteaseMusic(网易云音乐)
* Eudic(欧陆词典)
* Appcleaner(app卸载)
* BetterZip(压缩软件)
* Logitech-gaming-software(罗技游戏软件，其实我是用来做驱动的，我的鼠标是G500S)
* Moom(窗口管理神器)
* Baiduinput(百度输入法 for mac)

##### 编程类
* Sublime Text(前端必装)
* Adobe PS AI...(主要是ps切图)
* Microsoft…...(文档，不解释)
* Myeclipse(java代码必备)
* VirtualBox(主要是安装winxp虚拟机测试兼容性，我突然想干死微软的亲妈，ie7)
* CodeRunner(js代码必备)
* Dash(文档api必备)
* GoogleChrome(不解释)
* SequelPro(mysql数据库连接必备，主要是免费)
* Navicat Premiun(可选收费)
* Typora(强力推荐markdown书写神器，博客的文章都用它写的)

好吧，现在我们都搞定啦，那是不是可以安装APP啦，那么这里我给大家推荐几款好用的APP，以上为本人必装的软件，给极力推荐给大家，现在咱们来安装啦，打开终端，执行：
``` base
$ brew cask update
$ brew cask install qq iterm baiduinput wiznote neteasemusic eudic appcleaner betterzip moom sublimetext virtualbox coderunner dash google-chrome sequelpro typora
```
然后等待安装成功…我操，好爽，好吧，这下知道我为什么开始让大家安装homebrew跟cask了吧，Mac上安装软件就这么简单，brew cask的常用命令，更多的请参考[homecask官网](http://caskroom.io/)
``` base
$ brew cask search qq //查找软件
$ brew cask uninstall qq //卸载软件(很干净的卸载哟)
$ brew cask update //更新软件包
$ brew cask cleanup //清空缓存
```
以上部分软件过大或者因为收费，不能使用brew cask安装的，请大家自行下载安装，需要的请联系我，我云盘分享给你（有破解哟）。
![](/images/2016-12-05/Mac-2.png)

### 开发模块安装
Node

``` base
$ brew install node //安装最新的node
//如果你想安装指定版本的node，推荐使用nvm，但是本人不知道为什么，安装之后，重新开启终端就不行了，一直没成功，求指教啊，但是我还是用brew安装了node4.0的版本啦。
$ brew search node
homebrew/versions/node010        homebrew/versions/node06         leafnode                         nodebrew
homebrew/versions/node012        homebrew/versions/node08         node                             nodeenv
homebrew/versions/node04         homebrew/versions/node4-lts ✔    node-build                       nodenv
Caskroom/cask/mindnode-pro       Caskroom/cask/node               Caskroom/cask/nodeclipse         Caskroom/cask/soundnode
Caskroom/cask/node-profiler      Caskroom/cask/nodebox            Caskroom/cask/printnode
//本人安装了homebrew/versions/node4-lts4.0稳定版
```

Git

由于xcode自带了git，所以暂且还是用的自带的，后期可能会自己安装最新版的git，到时候在更新吧。
