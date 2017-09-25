---
layout: post
title:	"关于usr/bin/ld: cannot find -lxxx问题总结"
categories: 好玩
tags:  经验
---

* content
{:toc}


[转载]




# linux下编译应用程序常常会出现如下错误：

**/usr/bin/ld: cannot find -lxxx**

意思是编译过程找不到对应库文件。其中，-lxxx表示链接库文件 libxxx.so。

注：有时候，由于库文件是编译过程临时生成的，如果前面出错也会导致出现这种情况，下面针对的是由于本机系统环境缺失而引起的。。

### 一般出现这种错误有以下几种原因：

1. 系统缺乏对应的库文件；
2. 版本不对应；
3. 库文件的链接错误；
4. 库文件路径设置问题。

  对应第一第二种情况，可以通过下载安装lib来解决，ubuntu大多数可以直接通过apt-get来安装：

`apt-get install libxxx-dev`

  一般遇到这种问题笔者第一时间也是会去检查系统是否已安装该lib或者是否已选择正确版本（只是习惯问题），如果还是不能解决问题，那么，引起错误的原因不是链接错误就是库文件路径问题了。

  通过find或者locate指令定位到链接文件，查看链接文件是否正确的指向了我们希望的lib，如果不是，用 ln -sf */libxxx.so.x */libxxx.so 指令修改它。

不过，注意了，万一你不小心改错了什么就。。。

编译程序遇到：/usr/lib64/gcc/x86_64-suse-linux/4.3/../../../../x86_64-suse-linux/bin/ld: cannot find -lxml2

解决/usr/bin/ld: cannot find -lxxx 问题

**问题：**

在linux环境编译应用程式或lib的source code时常常会出现如下的错误讯息：

`/usr/bin/ld: cannot find -lxxx`

这些讯息会随着编译不同类型的source code 而有不同的结果出来如：

	/usr/bin/ld: cannot find -lc
	/usr/bin/ld: cannot find -lltdl
	/usr/bin/ld: cannot find -lXtst

其中xxx即表示函式库文件名称，如上例的：libc.so、libltdl.so、libXtst.so。

其命名规则是：lib+库名(即xxx)+.so。

会发生这样的原因有以下三种情形：

* 系统没有安装相对应的lib
* 相对应的lib版本不对
* lib(.so档)的symbolic link 不正确，没有连结到正确的函式库文件(.so)

**解决方法：**

	(1)先判断在/usr/lib 下的相对应的函式库文件(.so) 的symbolic link 是否正确，若不正确改成正确的连结目标即可解决问题。
	(2)若不是symbolic link 的问题引起，而是系统缺少相对应的lib安装lib即可解决。
	(3)如何安装缺少的lib：

以上面三个错误讯息为例：

	错误1缺少libc的LIB
	错误2缺少libltdl的LIB
	错误3缺少libXtst的LIB

以Ubuntu为例：

先搜寻相对应的LIB再进行安装的作业如：
	apt-cache search libc-dev
	apt-cache search libltdl-dev
	apt-cache search libXtst-dev

实例：

在进行输入法gcin的Source Code的编译时出现以下的错误讯息：

	/usr/bin/ld: cannot find -lXtst

经检查后发现是：

	lib(.so档)的symbolic link 不正确

解决方法如下：

	cd /usr/lib
	ln -s libXtst.so.6 libXtst.so

如果在/usr/lib的目录下找不到libXtst.so 档，那么就表示系统没有安装libXtst的函式库。

解法如下：

	apt-get install libxtst-dev

       如果是库文件路径引发的问题，可以到/etc/ld.so.conf.d目录下，修改其中任意一份conf文件，（可以自建conf，以方便识别）将lib所在目录写进去，然后在终端输入 ldconfig 更新缓存。


