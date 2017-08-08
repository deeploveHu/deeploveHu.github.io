---
layout: post
comments: true
categories: QT编程
---

转自[iteye](http://qimo601.iteye.com/blog/1283372)

1、QTcpSocket 继承于QAbstractSocket继承于QIODevice

2、QTcpSocket 提供的几种接收和发送数据方法

* write ( const char *, qint64 ) : qint64  
* write ( const char * ) : qint64  
* write ( const QByteArray & ) : qint64  
* writeData ( const char *, qint64 ) : qint64  
* read ( char * data, qint64 maxSize ): qint64  
* read ( qint64 maxSize ):QByteArray  
* readAll ():QByteArray  
* readLine ( char * data, qint64 maxSize ):qint64  
* readLine ( qint64 maxSize = 0 ):QByteArray  

3、例子1  write ( const QByteArray & ) : qint64

    //用于暂存要发送的数据  
    QByteArray block;  
    //使用数据流写入数据  
    QDataStream out(&block,QIODevice::ReadWrite);  
    //设置数据流的版本，客户端和服务器端使用的版本要相同  
    out.setVersion(QDataStream::Qt_4_6);  

    //设置发送长度初始值为0  
    out << (quint16) 0;  
    //设置发送内容  
    out << hash;  

    //回到字节流起始位置  
    out.device()->seek(0);  
    //重置字节流长度  
    out << (quint16) (block.size()-sizeof(quint16));  

    //往套接字缓存中写入数据，并发送  
    tcpSocket->write(block);  

3、例子2  write ( const char *, qint64 ) : qint64

    QString *a = new QString;  
    tcpSocket->write(a,a->length());  

4、例子3  数据流直接使用QIODevice

    QDataStream in(tcpSocket);  
    in<< quint16(0xFFFF); //此时QIODevice加载了此数据，而且直接发送出去  

    quint16 length = 0;  
    QDataStream out(tcpSocket);//如果此时tcpSocket直接有数据发送过来  
    out >> length;//length获得第一个整型值，并在tcpSocket中清空该数据  
