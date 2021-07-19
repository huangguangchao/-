##  QT下的串口编程

widget.h文件中要#include<QSerialPort>

widget.cpp文件中要#include <QSerialPortInfo>

 接收框：

![image-20210718111359823](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210718111402.png)

属性选择框：

![image-20210718112108487](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210718112110.png)

发送框：

![image-20210718112156561](https://gitee.com/hgchshs/markdown-table/raw/master/img/20210718112157.png)



### 查找串口号

~~~c
ui->setupUi(this);
QStringList serialNamePort;//定义一个QStringliist类型的变量
foreach (const QSerialPortInfo&info,QSerialPortInfo::availablePorts())
 {
        serialNamePort<<info.portName();
 }
ui->seialCb->addItems(serialNamePort);
//使用QSerialPortInfo:::availablePorts()获取当前串口，该函数返回容器类Qlist，用Qt定义的关键字foreach遍历容器Qlist里的串口信息，并将串口信息放到QStringList的类对象serialNamePort，显示到ui的串口组件。
~~~

### foreach关键字

foreach（variables ，Container）关键字是Qt对c++的一个扩展，主要用于按顺序历经容器（container）中的对象，

foreach用法类似于for循环，但是又有所不同，foreach可以使用一个变量名来遍历容器中的所有元素。

foreach宏的参数依次为：元素类型，元素名，容器类型，容器名。

 

从qt5.7开始，不鼓励使用这个宏。它将在Qt的未来版本中被删除

如果担心命名空间污染，可以通过将以下行添加到.pro文件来禁用此宏：

CONFIG += no_keywords

 

在标准C++中，并没有foreach关键字。

但是在QT中，可以使用这一个关键字，其主要原因是QT自己增加了这一个关键字，就像slots和signals、emit等一样。增加的foreach关键字在编译时会进行预处理。

其用法为：

foreach (varItem , Items)  // foreach（variable ，container）
其中，varItem(variable)是容器Items(container)中的一个项，相当于：variable=container.item 。遍历会从头遍历到尾。

如以下代码：

~~~c
QStringList slt = {"abc", "def", "123"};
foreach(QString s , slt )
{
    cout<<s<<endl;
}
// 输出结果为：
abc
def 
123
~~~

原文链接：https://blog.csdn.net/u013352076/article/details/108593430

### QSerialPortInfo(#include`<QSerialPortInfo>`)

该类是一个串口的辅助类类，提供主要是提供系统已经存在串口的信息。

该类中的静态函数（QList<QSerialPortInfo> availablePorts()后面详细介绍）生成了一个QSerialPortInfo对象的QList。

在该QList中的每个QSerialPortInfo对象分别对应于各个可用端口的信息（主要包括端口号（com），系统的位置，以及串口类型，厂商等信息）

可以调用静态该函数来获取系统的每一个可用端口信息QSerialPortInfo成员信息可以被调用于使用在QSerialPort设置串口。

下面来详细介绍QSerialPortInfo的成员以及功能

其成员public函数分为几类

####  1：构造函数

QSerialPortInfo()
QSerialPortInfo(const QSerialPort & port)
QSerialPortInfo(const QString & name)
QSerialPortInfo(const QSerialPortInfo & other)

以上四个构造函数是在定义该类的对象的时候接受不同的参数创建对象。

####  2：析构函数

~QSerialPortInfo()  

#### 3：端口信息函数

该类函数主要是返回该对象所对应的端口信息。

QString description() const   该函数返回的是一个QString数据类型，表示对象所对应的端口类型，例如是标准的通信端口，还是USB转串口等

bool hasProductIdentifier() const 判断该端口是否有有效的的16位产品编码，有的话返true否则返回false

bool hasVendorIdentifier() const 判断该端口是否有有效的16位产品供应商的编码，有的话返true否则返回false

boolisBusy() const  判断该端口是否被被占用，有的话返true否则返回false

boolisNull() const  判断该对象是否有一个确定的对应关联端口，如果是有的话返true否则返回false

QStringmanufacturer() const  返回生产厂商的信息

QStringportName() const  返回对象对应的端口号类型，端口号类型用QString数据类型表示，若是没有有效厂家信息，返回的是空QString

quint16productIdentifier() const  返回端口的16位序列号，若是没有，返回的是0

QStringserialNumber() const  返回用QSrting表示的的序列号  是在5.3以后的版本才有的

void swap(QSerialPortInfo & other) 该对象与 other引用所指向的对象互换相关信息，该函数的运行非常快，而且不会失败。

QString systemLocation() const  返回串口系统的位置

quint16 vendorIdentifier() const返回该端口是否有有效的16位产品供应商的编码，若是没有则返回0

#### 4：static函数

static函数为类的全部服务而不是为某一个类的具体对象服务。static成员函数与静态数据成员一样，都是类的内部实现，属于类定义的一部分。

QList<QSerialPortInfo>availablePorts()   该静态函数返回的是QSerialPortInfo对象的QList，该QList中的QSerialPortInfo对象对应于该系统的可用的端口。

调用该函数可以返回可用的端口

例如，该例子将每一个可用的端口的端口名打印出来，在comboBox控件上显示：



 foreach (const QSerialPortInfo &qspinfo, QSerialPortInfo::availablePorts())
  {
    ui->comboBox->addItem(qspinfo.portName());
  }  

QList<qint32>  standardBaudRates()  该函数返回的是当前串口标准的可用的波特率



其他函数：

QSerialPortInfo &operator=(const QSerialPortInfo & other)

如运算符重载函数等