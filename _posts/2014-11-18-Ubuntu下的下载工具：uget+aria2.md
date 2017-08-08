---
layout: post
comments: true
categories: 系统应用
---

转自[威客百科](http://baike.renwuyi.com/2014-11/2153.html)

*Windows下的下载工具--迅雷，之所以下载速度快，乃是它能搜索资源、为己所用，而不是仅仅从原始地址这单一资源处下载。*

*Ubuntu下也有类似的工具，那就是aira2。*

*aira2是一个命令行下载工具，可以配合其他图形界面的下载软件使用。我用的是uget+aria2。uget本身是一个小巧实用的多线程下载工具，加上aria2作为插件，下载速度有明显提高。*

### 一、安装

uget和aria2都可以在“软件中心”中安装，但是版本太老啦，无法发挥作用，所以最好还是在终端中添加ppa进行安装：

#### 1.uget的安装：

    sudo add-apt-repository ppa:plushuang-tw/uget-stable

    sudo apt-get update

    sudo apt-get install uget

#### 2.aria2的安装：

    sudo add-apt-repository ppa:t-tujikawa/ppa

    sudo apt-get update

    sudo apt-get install aria2

安装完aria2后，可以在终端中运行aria2c -v，查看版本和支持的特性。需要1.10以上的版本才能支持资源搜索。

### 二、使用与设置技巧

#### 1.主界面及版本

![2](http://baike.renwuyi.com/2014-11/data/uploads/654530819546abaaf2667c.jpg)


#### 2.启用aria2插件

![2](http://baike.renwuyi.com/2014-11/data/uploads/901724908546ababaebcd9.jpg)


#### 3.设置下载任务的属性(同时下载几个任务、多少个服务器、保存位置等)

![2](http://baike.renwuyi.com/2014-11/data/uploads/405625490546abac5899ba.jpg)

![2](http://baike.renwuyi.com/2014-11/data/uploads/1749213998546abad335700.jpg)


经本人测试(其他网友也是如此设置的)，服务器数设置为16比较合适。

OK啦，现在可以享受你的下载速度啦。我这破网速，用uget一直维持在80k/S左右，用了aria2插件后，经常冲上160K/S，哈哈。
