---
layout: post
title:	"Qt之QAbstractSocket"
categories: Qt
tags:  Qt
---

* content
{:toc}



> 这QAbstractSocket 类提供了整个socket的类型，是QTcpSocket和QUdpSocket的基类
创建一个本体套接字，可以调用QAbstractSocket 和 setSocketDescriptor()去包裹一个本地套接字
这个类竟可能的联合了TCP和UDP，尽管UDP是不可靠的连接，但是connectToHost()为UDP建立了一个假的连接，使其步骤尽量与TCP协议相似。在本质上QAbstractSocket通过调用connectToHost()记住了地址和端口,而QAbstratSocket中的成员函数 read和write将会使用保存的这个值。

## 通信连接流程

无论在什么时候QAbstractSocket都有一个状态，而我们可以通过调用成员函数state返回这个状态，刚开始的状态是 UnconnectedState,当程序调用了connectToHost之后，QAbstractSocket的状态会变成HostLookupState,如果主机被找到，QAbstractSocket进入connectingState状态并且发射HostFound()信号,当连接被建立的时候QAbstractSocket进入了connectedState状态并且发射connected()信号，如果再这些阶段出现了错误，QAbstractSocket将会发射error()信号，无论在什么时候，如果状态改变了，都会发射stateChanged(),如果套接字准备好了读写数据，isValid()将会返回true。

    State:   UnconnectState(Emit connectToHost())-->HostLookupState-->(找到host)-->connectingState(Emit HostFound())-->建立了连接-->connectedState(Emit connected())

无论什么时候 QAbstractSocket的状态改变都会发射stateChanged()信号，无论什么时候只要发生了错误，就会发送error()信号。

## 读写数据

向套接字里读写数据通过调用 read或者是write函数 ，当数据被写进套接字后会发射bytesWritten()信号。需要注意的是，qt没有限制写缓冲区的大小，因此我们可以监听这个信号，以此来获得写缓冲区的大小。

当一块数据到达时候，readRead()信号将会被发送，通过调用bytesAliable()函数可以得到可读的数量，因此我们可以连接readRead()信号连接到槽，以此来读取数据，如果不以此读取完毕，剩下的数据还是有效的，如果要限制读缓冲区可以调用 setReadBufferSize()。

## 关闭连接

关闭一个连接可以调用disconnectFromHost(),这个时候QAbstractSocket会进入closingstate状态，当将数据全部发送完之后，QAbstaractSocket会进入closedState状态，并且发送disconnected()信号。

而对于每个连接我们可以通过调用 peerPort() peerAddress() peerName 返回每个连接的名字，地址，端口，而我们可以通过调用localPort()和localAddress返回本地端口和地址。

## 相关函数

下列函数可以被用来实现一个阻塞套接字，并且挂起调用线程
* waitForConnected 阻塞直到连接被建立
* waitForReadyRead() 阻塞直到新的数据到达
* waitForBytesWritten() 阻塞直到数据写入完毕
* waitForDisconnected() 阻塞直到连接被关闭。


