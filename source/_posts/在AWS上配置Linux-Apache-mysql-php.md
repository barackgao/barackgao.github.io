---
title: 在AWS上配置Linux+Apache+mysql+php
date: 2016-10-20 00:11:14
categories: 技术
tags: [AWS,LAMP]

---
AWS推出了免费使用12月的优惠，童鞋们怎么能错过这样一个熟悉服务器的机会呢？废话不多说，教程走起来。
<!--more-->
## 使用EC2创建ubuntu Linux实例
这里不赘述太多，如果不会可以参考上篇《在AWS上配置VPN》，只是这里需要特别注意一下需要在安全组中添加规则，打开80端口的访问。
## 安装和配置apache
1、感觉并不需要太多的配置，只需要直接键入命令就OK了；

    sudo apt-get install apache2
    
2、需要说明的是，默认的根目录是`/var/www/html`，如果你有强迫症，可以选择修改`/etc/apache2/site-enable/000-default.conf`中相应的项目来修改默认的根目录；
3、测试一下，在浏览其中输入实例绑定的弹性IP，我们应该看到如下的页面就搞定了。
{% asset_img 1.png 图1 %}
## 安装和配置mysql
1、话不多说先安装；
    
    sudo apt-get install mysql-server
    
2、安装过程中会提示你输入数据库根用户的密码，自己设定好就好；
{% asset_img 2.png 图2 %}
3、安装完成后，输入`mysql -u root -p`，然后输入密码进入数据库中进行测试，这命令行真的不好用，先输入几条命令测一测再说；

	show databases 显示数据库
	use database_name 选中数据库database_name
	show tables 显示数据库中的所有表
	
4、如果我们需要添加新的用户，那么只需要在mysql数据库下的user表中插入相关的表项即可。
## 安装和配置php
1、直接键入如下命令安装；
	sudo apt-get install php5
2、不需要太多的配置，创建一个php页面进行测试就好。在命令行中输入如下命令创建一个php测试页面
	
	echo "<?php phpinfo(); ?>" | sudo tee /var/www/testing.php

值得注意的是，创建的页面一定要在apache配置的根目录中，不然是访问不了的，重要的一步，重启apache服务
	
	service apache2 restart
	
然后只需要在浏览器中输入实例绑定的弹性IP/testing.php，看到如下的界面就配置成功了；
{% asset_img 3.png 图3 %}
## 安装配置phpmyadmin
1、虽然说完成以上的步骤LAMP算是配置成功了，但是什么都要使用CLI这个确实让人感到淡淡的忧伤，所以我们还要引入phpmyadmin的安装和配置，实现图形化的管理，上安装命令
	
	apt-get install libapache2-mod-auth-mysql phpmyadmin
	
phpmyadmin会提示选择配置的web服务器，选择我们刚刚配置的apache2即可。
{% asset_img 4.png 图4 %}
之后，询问是否为phpMyAdmin配置一个名为`dbconfig-common`的数据库，选择`yes`然后输入之前设置的数据库密码，并确认密码；
{% asset_img 5.png 图5 %}
2、进行简单的配置，将phpMyAdmin的配置文件，复制到Apache2下
	
	sudo cp /etc/phpmyadmin/apache.conf /etc/apache2/conf-enabled/phpmyadmin.conf

然后重启Apache服务器
	
	sudo service apache2 restart
3、测试
在浏览器中输入弹性IP/phpmyadmin，我们可以看到如下的界面
{% asset_img 6.png 图6 %}
请忽略登录超时，输入用户名和密码就可以在浏览器中管理数据库了；

需要注意的是php中有个叫mcrypt的组建默认是dismod的，我们需要打开它，不然会显示一些奇奇怪怪的提示让人感到很不舒服，具体的命令如下
	
	sudo php5enmod mcrypt

然后重启apache服务
	
	sudo service apache2 restart

就可以看到舒舒服服的这个管理页面啦
{% asset_img 7.png 图7 %}
转载请注明出处。