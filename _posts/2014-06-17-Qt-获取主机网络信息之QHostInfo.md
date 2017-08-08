---
layout: post
comments: true
categories: QT编程
---

转自[博客园](http://www.cnblogs.com/xiaomanon/p/3792789.html?utm_source=tuicool&utm_medium=referral)

Qt中提供了几个用于获取主机网络信息的类，包括QHostInfo、QHostAddress、QNetworkInterface以及QNetworkAddress.在本节中，我将在这里总结QHostInfo类的用法，其他的类的介绍将会在后续文章中给出。

*注：更详细的内容，请参看官方文档。*



QHostInfo类提供了一系列用于主机名查询的静态函数。

QHostInfo类利用操作系统所提供的查询机制来查询与特定主机名相关联的主机的IP地址，或者与一个IP地址相关联的主机名。这个类提供了两个静态的便利函数：一个工作在异步方式下，并且一旦找到主机就发射一个信号；另一个以阻塞方式工作，并且最终返回一个QHostInfo对象。

要使用异步方式查询主机的IP地址，则调用lookupHost()，它需要传递3个参数，依次是主机名或IP地址、接收对象和接收槽函数，并且返回一个查询ID。你可以通过调用以查询ID为参数的abortHostLookup()方法的来中止查询。

当得到查询结果后就会调用此槽函数。查询结果被存储到一个QHostInfo对象中。可以通过调用addresses()方法来获得主机的IP地址列表，同时可以通过调用hostName()方法来获得查询的主机名。

如果查询失败，error()返回发生错误的类型。errorString()给出一个人们能够读懂的查询错误描述。

    #include "mainwindow.h"
    #include <QDebug>

    MainWindow::MainWindow(QWidget *parent)
         : QMainWindow(parent)
    {
       QHostInfo::lookupHost("www.baidu.com",
                            this, SLOT(printResult(QHostInfo)));
    }
    void MainWindow::printResult(QHostInfo result)
    {
        qDebug() << result.hostName();
        QList<QHostAddress> addrList = result.addresses();
        if (!addrList.isEmpty())
        {
           for  (int i = 0; i < addrList.size(); i++)
           {
             qDebug() << addrList.at(i);
           }
       }
    }

如果你想要使用阻塞方式查询，则使用QHostInfo::fromName()函数。查询给定主机名对应的IP地址。此函数在查询期间将阻塞，这意味着程序执行期间将挂起直到返回查询结果。返回的查询结果存储在一个QHostInfo对象中。

如果你传递一个字面IP地址给name来替代主机名，QHostInfo将搜索这个IP地址对应的域名 (ie. QHostInfo将执行一个反向查询)。如果成功，则返回的QHostInfo对象中将包含对应主机名的域名和IP地址。

    #include "mainwindow.h"
    #include <QDebug>

    MainWindow::MainWindow(QWidget *parent)
        : QMainWindow(parent)
    {
        QHostInfo info = QHostInfo::fromName("www.baidu.com");
        qDebug() << info.addresses();
    }



参考资料：  
《获取网络接口信息》-MyNote

《Qt网络之获取本机网络信息》-51CTO
