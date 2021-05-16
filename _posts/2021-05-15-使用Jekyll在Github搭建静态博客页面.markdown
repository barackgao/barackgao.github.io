---
layout: post
title:  "使用Jekyll在Github搭建静态博客页面"
date:   2021-05-16 11:15:52 +0800
categories: 技术分享
---
Github Pages作为世界最大的同性交友平台为Github用户提供了一个展示自我的平台。本文简单介绍了如何使用Jekyll和Markdown搭建自己Github Pages主页，记录和分享生活。

1. Jekyll的本地搭建

   本文只介绍在MacOS Catalina 10.15.7上搭建Jekyll环境的教程，目前比较全面的教程还比较少，希望本文能够帮助大家。

   Jekyll的依赖包括了Ruby、Gem、Bundler等等。有经验的Mac用户会发现MacOS预装了Ruby，但是由于本人习惯使用HomeBrew在Mac管理软件包，所以这里踩了很多坑。HomeBrew的最新版本已经不支持sudo运行，缺少一些系统目录的访问权限，因此我们需要使用HomeBrew重新安装一个Ruby。两个Ruby的安装目录不同，一定需要注意设置系统的环境变量，否在在后期执行命令时会有各种各样的问题。废话不多说，直接上命令，为了方便访问Github，在执行这些命令之前，还请把DNS地址修改为`8.8.8.8`。

   * 安装HomeBrew

     `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` 

   * 使用Home安装Ruby

     `brew install ruby`

   使用`where ruby`命令可以发现系统预装的Ruby和我们刚刚使用HomeBrew安装的Ruby的路径是不同的

   系统预装的Ruby路径为`/usr/bin/ruby`，而HomeBrew安装的Ruby路径为`/usr/local/opt/ruby/bin/ruby`，在后续的步骤之前一定要把目录`/usr/local/opt/ruby/bin/`添加到用户的环境变量，否在你会碰到各种各样的问题，别问我是怎么知道的。使用`which ruby`确认一下当前使用的Ruby是哪一个，如果看到的是`/usr/local/opt/ruby/bin/ruby`，那么就可以执行后续的操作了。官方文档上为了方便多版本的Ruby切换使用的`rbenv`对Ruby的版本进行管理，因为懒所以我直接忽略了这个直接开始安装bundler和jekyll。安装的方式分为用户安装和全局安装两种，我选择了用户安装，个人认为就是安装的位置不一样，不过这个非常影响环境变量的配置，请各位看官一定一定要注意正确配置！

   * 安装Bundler和Jekyll

     `gem install --user-install bundler jekyll`

   安装完后，我们可以使用`which bundle`和`which jekyll`命令查看安装位置，由于我们没有使用rbenv对Ruby版本进行管理，因此我们会发现Bundler和Jekyll被安装到`～/.local/share/gem/ruby/3.0.0/bin/`，需要在环境变量中添加该路径执行后边的操作。至此，个人主页的环境搭建已完成。

2. 在本地创建个人主页

   选择创建主页的目录，执行命令`jekyll new HomePage`，我们就可以快速创建我们自己的主页。在运行之前，我们还需要执行一步关键的操作，这个在很多教程上都没有写到，我也是尝试了很久才避免了这个问题，还希望大家一定注意这一步操作。使用命令`cat Gemfile | grep webrick`中发现依赖文件中没有添加webrick这包。因此，在本地运行之前我们一定一定要添加这个包！

   * 添加webrick

     `bundle add webrick`

   完成之后执行命令`cd HomePage`进入主页的目录，执行`bundle exec jekyll serve --trace`我们就可以在浏览器中输入` http://127.0.0.1:4000/`访问到我们创建的个人主页。其中各种内容的修改，我们之后专门出一篇文章说明。

3. 把个人主页内容推送到Github

   我们只需要在自己Github创建Github Pages的仓库，然后在个人主页文件夹中添加对应的`.gitignore`模版将主页文件上传，就可以在Github Pages对应的域名中访问自己的主页啦～

本期的教程到这里就结束了，欢迎大家交流、打赏！

![名片和打赏](/assets/img/bottom_qr_code.png)

