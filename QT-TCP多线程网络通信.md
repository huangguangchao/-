

##  QT多线程网络通信

使用QT提供的类进行基于TCP的套接字通信需要用到两个类：

1、**QTcpServer**   服务器类，用于监听客户端连接以及和客户端建立连接

2、**QTcpSocket**    通信的套接字类，客户端、服务器端都需要使用

​			这两个套接字通信类都属于网络模块  network；QFile和QTcpSocket的祖先类都是QIODevice



### QTcpServer

#### 公共成员函数

~~~c++
QTcpServer::QTcpserver(QObject *parent = Q_NULLPTR);
//构造成员函数，构造一个用于监听的服务器端对象
//传入的参数是一个父对象
~~~

~~~c
bool QTcpServer::listen(const QHostAddress &address = QHostAddress::Any, quint16 port = 0)
 //传入的参数一个是要绑定的本地地址，any代表可以是任意的ip地址，建议使用默认的any；用于定位主机
    //一个是端口号，把数据发送到某一个进程，使用时不赋值就会随机绑定一个端口
~~~

~~~c
bool QTcpServer::isListening() const
    //判断当前对象是否在监听，是返回true，否返回false
~~~

~~~c
QHostAddress Qrcpserver::serverAddress()const;
//如果当前对象正在监听返回监听的服务器地址信息，否则返回QHostAddress:：Nul1
~~~

~~~c

quint16 QTcpServer:：serverport（）const
 //如果服务器正在侦听连接，则返回服务器的端口；否则返回0
~~~

~~~c
QTcpSocket* QTcpSerVer::nextPendingConnection();
//得到和客户端建立连接之后用于通信的QTcpsocket 套接字对象
~~~

~~~c
[signal] void QTcpServer::acceptError(QAbstractSocket::SocketError socketError)
    //当接受新连接导致错误时，将发射如下信号。socketError参数描述了发生的错误相关信息
~~~

~~~c
[signal] void QTcpServer::newConnection()
   // 每次有新连接可用时都会发出newConnection0信号。
   // 可以和QTcpSocket* QTcpSerVer::nextPendingConnection();搭配使用
~~~

~~~c
[virtual] void QAbstractSocket::connectToHost(const QString &hostName, quint16 port, QIODevice::OpenMode openMode = ReadWrite, QAbstractSocket::NetworkLayerProtocol protocol = AnyIPProtocol)
    //连接服务器，需要指定服务器端绑定的IP和端口信息。
~~~

###  **QTcpSocket**  

QTcpSocket 是一个套接字通信类，不管是客户端还是服务器端都需要使用。在Qt中发送和接收数据也属于IO操作（网络I0），先来看一下这个类的继承关系：

![image-20210722133115742](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210722133117.png)

接收数据：

~~~c
//指定可接收的最大字节数maxsize的数据到指针 data指向的内存中
qint64 QIODevice::read（char*data，qint64 maxsize）;
//指定可接收的最大字节数maxsize，返回接收的字符串
    QByteArray QIODevice::read（qint64 maxsize）;
//将当前可用操作数据全部读出，通过返回值返回读出的字符串
        QByteArray QIODevice::readAll（）;
~~~

发送数据：

~~~c
	//发送指针 data指向的内存中的maxSize个字节的数据
qint64 QIoDevice::write（const char*data，qint64 maxsize）；

    //发送指针 data指向的内存中的数据，字串以\0作为结束标记
qint64 QIODevice::write（const char*data）；

    //发送参数指定的字符串
qint64 QIODevice::write（const QByteArray&byteArray）；
~~~



信号：

~~~c
//在使用Qrcpsocket进行套接字通信的过程中，如果该类对象发射出readyRead（）信号，说明对端发送的数达了发之后就可以调用read函数接收数据了。
[signal]void QIODevice:：readyRead（）；
    
   
    //调用connectTolost（）函鼓并成功建立连接之后发出connected（）信号。
[signal]void QAbstractsocket:：connected（）；
   
   
   // 在套接字浙开连接时发出disconnected（）信号。
[signal]void QAbstractsocket:：disconnected（）；
~~~



### 通信流程

1.创建套接字服务器 Qrcpserver 对象
2.通过Qrcpserver 对象设置监听，即：QTcpserver:：listen（）
3.基于QTcpServer:：newconnection（）信号检测是否有新的客户端连接

4.如果有新的客户端连接调用QTcpsocket *QTcpserver:：nextpendingconnection（）得到通信的套接字对象
5.使用通信的套接字对象 QTcpSocket 和客户端进行通信









