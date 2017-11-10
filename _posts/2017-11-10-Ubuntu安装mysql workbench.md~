---
layout: post
title:	"Ubuntu安装mysql workbench"
categories: ubuntu
tags:  经验
---

* content
{:toc}



转自[香港硅谷](https://www.hksilicon.com/articles/4394?lang=cn)




> MySQL Workbench 可以拿来管理 MySQL, 也可以来画 ERD, 此篇主要是写如何安装.

> 使用 deb 安装, 并解决套件相依性的快速安装法: 采用 dpkg 安装 deb 档, 再利用 aptitude 来自动补齐所需要的套件.

## MySQL Workbench 相关文件

> MySQL Workbench 使用教学可见: [Visual Database Creation with MySQL Workbench](https://code.tutsplus.com/articles/visual-database-creation-with-mysql-workbench--net-10975)


> MySQL Workbench 下载: [MySQL Workbench](https://dev.mysql.com/downloads/workbench/) # 下述进行前, 请先于此处下载 MySQLWorkbench() 的 deb 安装档

### 安装

使用 dpkg 安装 `mysql-workbench-oss_5.2.18-1ubu910_amd64.deb`, 如下述命令

		$ dpkg -i mysql-workbench-oss_5.2.18-1ubu910_amd64.deb # 会出现下述错误讯息
		dpkg：因相依问题，不能设定 mysql-workbench-oss：
 		mysql-workbench-oss 相依于 libmysqlclient15off (>= 5.0.27-1)﹔然而：
  		未曾安装套件 `libmysqlclient15off'。
 		mysql-workbench-oss 相依于 libzip1﹔然而：
  		未曾安装套件 `libzip1'。
 		mysql-workbench-oss 相依于 python-paramiko﹔然而：
  		未曾安装套件 `python-paramiko'。
 		mysql-workbench-oss 相依于 python-pexpect﹔然而：
  		未曾安装套件 `python-pexpect'。
 		mysql-workbench-oss 相依于 mysql-client﹔然而：
  		未曾安装套件 `mysql-client'。
		dpkg：在处理 mysql-workbench-oss (--install) 时发生错误：
 		相依问题 - 保留为未设定
		正在进行 desktop-file-utils 的触发程式 ...
		在处理时有错误发生：
 		mysql-workbench-oss

再来补相关 Package, 靠 aptitude 来补, 如下述指令:

`$ sudo apt-get install aptitude` 

`$ aptitude full-upgrade # aptitude` 会自动帮你找解决.

出现下述讯息, 第一个选 N, 第二个、第三个选 Y 即可.

	注:
		第一个选 N 是因为 aptitude 认为最简单的解法就是将刚刚安装的 deb 移除, 只要移除一个 Package 就可以解决相依性的问题, 所以选 N, 不要让它移掉.
		第二个选 Y 是 aptitude 找出所相依性的 Package, 问是否要用此解决方案, 选 Y
		第三个选 Y 就是安装, 解决套件相依性问题.

MySQL Workbench 详细安装过程

		以下的套件状态为毁断
  		mysql-workbench-oss 
		0 个套件升级, 0 个新安装, 0 个将移除且 0 个不会升级.
		需要下载 0B 的归档档案. 解装后将用去 0B.

		以下套件含有相依性:
  			mysql-workbench-oss: 相依关系: libmysqlclient15off (>= 5.0.27-1) 但这无法安装
                       			相依关系: libzip1 但这无法安装
                       			相依关系: python-paramiko 但这无法安装
                       			相依关系: python-pexpect 但这无法安装
                       			相依关系: mysql-client 但这无法安装
		以下动作会解决这些相依问题:

		移除 下列套件：
		mysql-workbench-oss

		分数是 121

		是否接受该解决方案？[Y/n/q/?] N <-- 不要移掉

		以下动作会解决这些相依问题:

		安装 下列套件：
		libdbd-mysql-perl [4.011-1ubuntu1 (karmic)]
		libdbi-perl [1.609-1 (karmic)]
		libmysqlclient15off [5.1.30really5.0.83-0ubuntu3 (karmic)]
		libnet-daemon-perl [0.43-1 (karmic)]
		libplrpc-perl [0.2020-2 (karmic)]
		libzip1 [0.8-1 (karmic)]
		mysql-client-5.1 [5.1.37-1ubuntu5.1 (karmic-updates, karmic-security)]
		python-paramiko [1.7.4-0.1 (karmic)]
		python-pexpect [2.3-1 (karmic)]

		分数是 -35
		是否接受该解决方案？[Y/n/q/?] Y

		以下新套件将会安装:
  		libdbd-mysql-perl{a} libdbi-perl{a} libmysqlclient15off{a} libnet-daemon-perl{a} libplrpc-perl{a} libzip1{a} mysql-client-5.1{a} 
  		python-paramiko{a} python-pexpect{a} 
		以下部份套件将会被设定：
  		mysql-workbench-oss
		您想继续吗? [Y/n/?] Y





