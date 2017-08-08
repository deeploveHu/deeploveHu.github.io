---
layout: post
comments: true
categories: 系统应用
---

转自[Linux公社](http://www.linuxidc.com/Linux/2015-08/122300.htm)

Opera、Chrome、firefox 非常流行的三大浏览器，当然对于中国用户来说 Opera 还比较陌生一些，这是一款挪威Opera Software ASA公司制作的支持多页面标签式浏览器。跨平台，可运行在 Windows、Mac 和 Linux 系统上。

Opera浏览器创始于1995年4月，最新版本 Opera 33 Dev (Opera 32.0.1963.0) 基于 Chromium 46，带来 Windows 和 Mac OS X 增强，并针对 Linux 的BUG修复。

![2](http://www.linuxidc.com/upload/2015_08/15082620552695.png)

官方网站：[点击这里](http://www.opera.com/zh-cn)

安装，下载 DEB 安装包（支持 Ubuntu 14.04/14.10/15.04），通过以下命令安装。

32位系统：

    sudo apt-get install libnotify-bin gdebi
    wget http://get.geo.opera.com/pub/opera-developer/33.0.1963.0/linux/opera-developer_33.0.1963.0_i386.deb
    sudo gdebi opera-developer_33.0.1963.0_i386.deb

32位Ubuntu系统安装截图：

![2](http://www.linuxidc.com/upload/2015_08/15082620553953.png)

![2](http://www.linuxidc.com/upload/2015_08/15082620552472.png)

![2](http://www.linuxidc.com/upload/2015_08/15082620562939.png)

然后还要使用Tab键选择确定，回车

![2](http://www.linuxidc.com/upload/2015_08/15082620561606.png)

选择是，回车

![2](http://www.linuxidc.com/upload/2015_08/15082620569953.png)

64位系统：

    sudo apt-get install libnotify-bin gdebi
    wget http://get.geo.opera.com/pub/opera-developer/33.0.1963.0/linux/opera-developer_33.0.1963.0_amd64.deb
    sudo gdebi opera-developer_33.0.1963.0_amd64.deb

卸载命令：

    sudo apt-get remove opera-developer
