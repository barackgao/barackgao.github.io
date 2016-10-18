---
title: 在AWS上配置VPN
date: 2016-10-17 16:41:58
categories: 技术
tags: [AWS,VPN]

---
随着我们上网需求的提升，拆掉墙放眼展望世界已是必然的趋势，此时亚马逊的AWS横空出世，为我们拆墙看世界提供了极大的方便，同时这个还可以满足电脑、手机多种设备所需。下面就让我们动起手来吧！
<!-- more -->
## 配置和启动实例
1、创建实例，选择图示的版本，既符合免费的套餐，又是我们熟悉的ubuntu；

{% asset_img 1.png 图1 %}

2、一路下一步到配置安全组，注意这一步很重要，否则可能导致VPN无法连接。点击添加规则，自定义TCP规则，端口号选择1723，来源选择自定义`0.0.0.0/0`；

{% asset_img 2.png 图2 %}

3、然后启动实例，这样就完成了实例的创建和启动。万事开头难，有了实例我们就可以开始做点文章了。
## 创建和关联弹性IP
1、在左边栏中选择弹性IP，然后点击上方的蓝色按钮分配新地址，这样我们又拥有一个属于我们自己的公网IP了；

{% asset_img 3.png 图3 %}

{% asset_img 4.png 图4 %}

2、选择图中我们分配好的IP，在上方的点击操作按钮，选择关联地址，在弹出的窗口中选择我们刚刚创建好的实例，不用自己输入，直接双击鼠标选择实例就好。这样我们就完成了第二步，创建和关联好了公网IP，这样VPN客户端使用的时候就有了固定的公网IP，妈妈再也不用担心我的VPN连不上服务器了。

{% asset_img 5.png 图5 %}

## 登陆实例
AWS给出了具体的登录方法，windows用户可以使用Xshell等软件登录，Mac用户使用terminal直接登录就好，或者嫌麻烦的话使用网页上的terminal也可以，具体的登录方法参见AWS官方文档，在这里就不赘言了。
## 配置pptpd（这一部分比较麻烦，请仔细看好每一步）
### 使用apt源服务来安装PPTPD服务

    sudo apt-get update
    sudo apt-get install pptpd

### 安装完成之后编辑` pptpd.conf `配置文件

    sudo vi /etc/pptpd.conf

确保如下选项的配置

	option /etc/ppp/pptpd-option #指定PPP选项文件的位置
	debug #启用调试模式
	localip 192.168.0.1 #VPN服务器的虚拟ip
	remoteip 192.168.0.200-238,192.168.0.245 #分配给VPN客户端的虚拟ip

### 编辑PPP选项配置文件

	sudo vi /etc/ppp/pptpd-options

确保如下选项的配置

	name pptpd #pptpd服务的名称
	refuse-pap #拒绝pap身份认证模式
	refuse-chap #拒绝chap身份认证模式
	refuse-mschap #拒绝mschap身份认证模式
	require-mschap-v2 #允许mschap-v2身份认证模式
	require-mppe-128 #允许mppe 128位加密身份认证模式
	ms-dns 8.8.8.8 #使用Google DNS
	ms-dns 8.8.4.4 #使用Google DNS
	proxyarp #arp代理
	debug #调试模式
	dump #服务启动时打印出所有配置信息
	lock #锁定TTY设备
	nobsdcomp #禁用BSD压缩模式
	logfile /var/log/pptpd.log #输出日志文件位置，这一步可以省略

### 编辑用户配置文件来添加用户

	sudo vi /etc/ppp/chap-secrets

确保如下选项的配置格式：

	用户名   服务类型   密码   分配的ip地址
	test    *    1234    *

第一个\*代表服务可以是PPTPD也可以是L2TPD，第二个\*代表随机分配ip，童鞋们可以选择自己喜欢的用户名和密码，客户端登录的时候需要匹配。

### 重启PPTPD服务

	sudo service pptpd restart  

### 配置网络和路由规则 
设置ipv4转发

	sudo sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf
	sudo sysctl -p

启用iptables的NAT configuration

	sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

### 设置例行任务
为了保证每次EC2实例重启后NAT configuration能启动, 还要修/etc/rc.local文件, 在exit 0这行上面加上

	iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE

### 最后重启pptpd服务

	sudo /etc/init.d/pptpd restart

终于配置工作大功告成了，下面就是连接客户端的问题了，大家不要着急，等我一一给各位童鞋道来。
## 客户端配置
因为自己使用的是iPhone，就以iPhone为例来配置客户端，啥也不说直接上图，类型选择PPTP，按理说L2TP也是可以的，其中的Server就是我们刚刚创建的弹性IP，Account是我们设置好的用户名，如果严格按照教程来的话就是test，Password是用户名对应的密码，同样如果是严格按照教程来的话就是1234

{% asset_img 6.png 图6 %}

万事具备，连接即可，这样童鞋们无论是抓小精灵、海淘败一败抑或是逛一逛霓虹国的小网站都可以开心愉快的浪起来了。开山第一篇文档，还希望大家多支持。

<font color=#ff0000 size=5>最后提醒一下各位童鞋， AWS免费账户每月只有 15G 免费流量, 用超了要 从信用卡扣钱的！这个问题很关键！！！</font>

转载请注明出处。