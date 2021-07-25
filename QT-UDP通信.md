## QT-UDP通信

Header: 	#include <QUdpSocket>

#include<QWidget>
#include <QUdpSocket>
#include<QString>
#include<QHostAddress>

qmake:	 QT+=network

### 初始化socket

1、new一个socket；

在public里先声明一个QUdpSocket *udpSocket;//QUdpSocket *类型的变量udpSocket

~~~c
udpSocket=new QUdpSocket(this); //this的意思是如果父对象被删除那么子对象也被删除
~~~



2、把端口号绑定到socket上

一般在打开按钮的槽函数中实现，槽函数统一在private中定义。

3、调用连接函数

~~~c
udpSocket->bind(ui->localPort->text().toUInt());//把端口号窗口的内容转为uint类型作为要绑定的端口号
connect(udpSocket,SIGNAL(readyRead()),this,SLOT(readyRead_slot()));//三个参数分别为QUdpSocket *类型的变量，信号readyRead()，this，SLOT(接收框的槽函数))
~~~





### 接收数据槽函数

1、接收框的槽函数

~~~c
void Widget:: readyRead_slot()
{
while(udpSocket->hasPendingDatagrams()){//数据没有读完的时候进入
	QByteArray array; //定义一个数组用来储存数据
    array. resize(udpSocket->pendingDatagramSize());//数组的大小为upd中未读完数据的大小
    udpSocket->readDatagram(array. data(), array. size()); //调用读数据函数，参数1为数据内容，参数2为数据大小
    QString buf; 
    buf = array.data();//用一个QString类型的变量讲数据输出
    ui->recvEdit->appendPlainText(buf);//接收框输出数据
}
~~~



2、关于提示信息

~~~c
//打开是否成功可以用一个message Box来直观提示
	if（udpSocket->bind（ui->localPort->text（）.toUInt（））==true）
    {
        QMessageBox::information（this，“提示”，“成功“）；
	}
	else
    {
		QMessageBox::information（this，“提示”，“失败”）；
    }
~~~



### 发送数据槽函数

~~~c
//定义一些变量用来存放数据
quint16 port;//端口号
QString sendbuff;//要发送的数据
QHostAddress address;//主机地址

address. setAddress(ui->aimIp->text()); //把目标ip转化为hostaddress类型
sendbuff=ui->sendEdit->text(); //赋值
port=ui->aimPort->text(). tolInt();//赋值
udpSocket->writeDatagram
    (sendbuff.tolocal8Bit().data(),sendbuff.Length(),address,port);
//sendbuff里的数据转成char*          要发送的数据的长度   目标IP地址 目标端口号
~~~



关闭udp的槽函数

~~~c
udpSocket->close();
//同样也可以加个massage Box用来直观提示
~~~

