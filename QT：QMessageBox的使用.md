##  QT：QMessageBox的使用

### 常用用法

QMessageBox::NoIcon 没有任何图标
QMessageBox::Information 消息图标
QMessageBox::Warning 警告消息
QMessageBox::Critical 严重

### 按钮变量

QMessageBox::NoButton 无图标
QMessageBox::Ok 确定
QMessageBox::Cancel 取消
QMessageBox::Yes 是
QMessageBox::No 否
QMessageBox::Abort 中断
QMessageBox::Retry 重试
QMessageBox::Ignore 取消

QMessageBox::Default(进行或运算),当Enter被按下时被击活

QMessageBox::Escape(进行或运算),当Esc被按下时被击活

### 构造函数

QMessageBox::QMessageBox(QWidget *parent=0,const char *name=0)
构造父对象为parent,名字标识为name
QMessageBox::QMessageBox(const QString &caption,const QString &text,Icon icon,

                         int button0,int button1,int button2,QWidget *parent=0,
    
                         const char *name=0,bool modal=TRUE,WFlags f=WStyle_DialogBorder)

构造标题为caption,文本为text,图标为icon和最多为三个按钮的消息框



QMessageBox::~QMessageBox()

### 销毁消息框



~~~c
void QMessageBox::about(QWidget *parent,const QString &caption,const QString &text)//静态显示一个标题为caption文本为text的简单关于框
~~~



~~~c
void QMessageBox::aboutQt(QWidget *parent,const QString &caption=QString::null)//静态显示关于该应用程序正在使用的QT版本号
~~~

~~~c
int QMessageBox::critical(QWidget *parent,const QString &caption,const QString &text,int button0,int button1,int button2=0)//静态打开一个标题为caption并且文本为text的严重消息框,该对话框最多有三个按钮,如果不想要三个按钮,可把最后一个或者最后二个设置为QMessageBox::NoButton
~~~

~~~c
int QMessageBox::critical(QWidget *parent,const QString &caption,const QString &text,const QString &button0Text=QString::null,const QString &button1Text=QString::null,const QString &button2Text=QString::null,int defaultButtonNumber=0,int escapeButtonNumber=-1)//虚显示一个标题为caption、文本为text并且按钮分别为1、2、3的严重消息对话框。返回被点击的按钮的数字（0、1或2）。


~~~

​	





Icon QMessageBox::icon() const
返回消息框的图标
const QPixmap *QMessageBox::iconPixmap() const
返回当前图标
int QMessageBox::information(QWidget *parent,const QString &caption,const QString &text,int button0,int button1=0,int button2=0)虚
打开一个标题为caption并且文本为text的消息框,点击返回按钮标识QMessageBox::Ok/QMessageBox::No等等

void QMessageBox::setButtonText(int button,const QString &text)
设置消息框按钮button的文本为text
void QMessageBox::setIcon(Icon icon)
设置消息框的图标
void QMessageBox::setIconPixmap(const QPixmap &)
设置当前图标
void QMessageBox::setText(const QString &)
设置被显示的消息框文本
void QMessageBox::setTextFormat(TextFormat)
设置消息框中显示的文本的格式
QString QMessageBox::text() const
返回被显示的消息框文本
TextFormat QMessageBox::textFormat() const
返回消息框中显示的文本的格式
int QMessageBox::warning(QWidget *parent,const QString &caption,const QString &text,int button0,int button1,int button2=0)静态
打开一个标题为caption文本为text的警告消息框
如果parent为0消息框变为应用程序全局的模式对话框
如果parent为一个窗口部件,消息框变为相对于parent的模式对话框

============================================================================================

以下是几种对话框：

先来看一下最熟悉的QMessageBox::information。我们在以前的代码中这样使用过：

QMessageBox::information(NULL, "Title", "Content", QMessageBox::Yes | QMessageBox::No, QMessageBox::Yes);

下面是一个简单的例子：



如果我们想自定义图片的话，也是很简单的。这时候就不能使用这几个static的函数了，而是要我们自己定义一个QMessagebox来使用：

~~~c
QMessageBox message(QMessageBox::NoIcon, "Title", "Content with icon."); 
 message.setIconPixmap(QPixmap("icon.png")); 
 message.exec();
这里我们使用的是exec()函数，而不是show()，因为这是一个模态对话框，需要有它自己的事件循环，否则的话，我们的对话框会一闪而过哦(感谢laetitia提醒).
需要注意的是，同其他的程序类似，我们在程序中定义的相对路径都是要相对于运行时的.exe文件的地址的。比如我们写"icon.png"，意思是是在.exe的当前目录下寻找一个"icon.png"的文件。这个程序的运行效果如下：
~~~



============================================================================================

还有一个常见的问题：就是我们如何才能将QMessageBox对话框中的按钮改写成中文：

1，QT中如何显示中文呢？

~~~QT
QTextCodec*pCodec=QTextCodec::codecForName("System");//获取系统字体编码

QTextCodec::setCodecForLocale(pCodec);

QTextCodec::setCodecForCStrings(pCodec);

QTextCodec::setCodecForTr(pCodec);
~~~





如果你的操作系统当前是中文环境的话，就可以显示中文。



或者：#include

QTextCodec::setCodecForTr(QTextCodec::codecForName(QTextCodec::codecForLocale()->name()));
QTextCodec::setCodecForLocale(QTextCodec::codecForName(QTextCodec::codecForLocale()->name()));
QTextCodec::setCodecForCStrings(QTextCodec::codecForName(QTextCodec::codecForLocale()->name()));

 

2，通过第1步的操作就可以在文本中显示中文了，但是还不能在内置的QMessageBox按钮上面显示中文。

该怎么办呢？

（1）从QT安装目录下面将文件“qt_zh_CN.qm”复制一份到你的项目目录下。

例如，我是从“D:\QtSDK\Desktop\Qt\4.7.3\msvc2008\translations”目录下复制的“qt_zh_CN.qm”文件。

你的项目目录指你项目的.pro文件所在的目录。

（2）在项目目录中新建一个文本文件，输入如下内容：

 

 

 

​    

        qt_zh_CN.qm


​    

 

保存后，将该文本文件的后缀名由txt改为qrc，表明这是资源文件。

（3）在Qt Creator中将上述文件加到你的项目中。

（4）修改代码如下，表示要加载相应的资源文件。

 

    QTranslator oTranslator;
    
    oTranslator.load(":/qt_zh_CN"); // 注意此处字符串以“:/”开头，后接的字符串是刚才复制的qm文件的名字
    
    QApplication oApp(argc, argv);
    
    oApp.installTranslator(&oTranslator);

（5）重新编译、运行程序即可。

或者：（1）同上

        （2）加载该文件：
    
             QTranslator oTranslator;
             oTranslator.load("qt_zh_CN.qm"); 
            QApplication oApp(argc, argv);


```QT
        oApp.installTranslator(&oTranslator);
```

 


最好是在加载该文件的时候，首先判断下改文件是否存在：QFile  file(qt_zh_CN.qm)； bool  b = file.exists();

（3）重新编译、运行程序即可。

效果如下




===========================================================================================
但是：用这个QMessageBox::information(this, sTitle, sMessage, "确定", "取消");
效果：


这样的效果岂不是更好。
【注】：当我们用简单的思维思考问题的时候，问题就很简单；当我们用复杂的思维思考问题的时候，问题就会很复杂。

————————————————
版权声明：本文为CSDN博主「Persevering_love」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/Persevering_love/article/details/72621879