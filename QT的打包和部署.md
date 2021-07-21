##  QT的打包和部署

### 1、把工程切换到release模式，然后编译。

release模式：基本没有调试信息。

debug模式：有很多调试信息。

### 2、找到release模式构建的文件夹。

### 3、改一下图标

先把图标加到工程所在文件夹，然后再pro文件里面添加RC_ICONS = “图标文件的名称（带后缀）”

<u>注意</u><u>：图标的格式必须为.ico这个格式的，其他不可以。</u>

这里推荐一个线上其他格式转ico格式的网站

[制作ico图标 | 在线ico图标转换工具 方便制作favicon.ico - 比特虫 - Bitbug.net](https://www.bitbug.net/)

### 4、封包需要用到QT的控制台

在开始菜单里输入QT，打开这个程序：Qt 5.11.164-bit for Desktop(MSVC 2017)/Qt 5.11.1 for Desktop(MinGW 5.3.0 32 bit)

在一个方便的地方创建一个新的文件夹；**注意不能有中文路径**；然后把release文件夹里生成的exe文件拷贝到新的文件夹里

### 5、小黑框里的指令操作

1、”cd/d + 新的文件夹的绝对路径“；<!--进入文件夹-->

2、“dir”指令查看目录

3、使用“windeployqt + exe文件的名字”指令把库加到新建的文件夹里

### 6、关于小黑框的坑

小黑框有两个；一个是ming32位的，一个是msvc64位的，打包的时候看release文件的名称是哪个就i选哪个。不然打包出来的文件无法执行。



**以下是本人的博客**

https://blog.csdn.net/hgchshs/article/details/118944641

