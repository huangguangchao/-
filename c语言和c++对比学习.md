## 头文件

### c语言

stdio.h

stdlib.h

string.h

...

### c++

iostream

cstdlib

cstring

...

## 命名空间

~~~c
//声明命名空间有三种方式//
1、std::cout
2、using namespace std;
3、using std::cout
//::域运算符

~~~

## 输入输出

~~~c++
c语言//
//scanf("%d,&num");//格式控制符%d,%s%c%u%x%f%lf...
    printf("hellow %d\n"),num;

//c++
cin >> num;//不用取地址，cout自动识别
//cout << "hello"<<num<<endl;
//endl意思是结束这一行
~~~

## 基本数据类型

c语言：int char float double

c++新增了bool

bool isok = ture;//两种值，true and false

~~~c++
cout << boolalpha<<isok<<endl;//boolalpha指定输出为true or false，不自动转化为0或1
~~~

## 结构体

~~~c
struct Testcpp
{
    int id;
    char name[10];
    
};
struct Testcpp stu;//c语言正确，c++正确
Testcpp stu1;//c语言错误，c++正确
c语言结构体：里面不允许定义函数 不允许省略struct关键字
c++结构体：允许定义函数 允许省略struct关键字
    
~~~

## 强制类型转换

~~~c
c与语言：要在转换的变量之前加上（要转换的类型）就可以了
    c++：
    static_cast<float>(a);//强制类型转换
	const_cast<int&>(cc)//去掉常量属性
        //其实是另外开辟一块空间把常量复制一下，是假的去掉常量属性
        
~~~

## 条件运算符

~~~c
//（a>b?a:b） = 66;//在c语言中？：返回的是一个值，这样写错误
（a>b?a:b） = 66;//在c++中？：返回的是一个引用，即变量本身，正确
~~~

## 动态内存分配

~~~c
int* cl = (int)malloc(sizeof(int)*10);//c语言动态内存分配
for（inti=e；i<1e；i++）
{
c1[i]=i；
}
for（inti=e；i<1e；i++）
{
printf（“%d"，c1[i]）；
free（c1）；//不使用后要进行释放，避免出现内粗泄露
}    
sout<<end1；
//int*seg=new int[10]；//不初始化
int*spp=new int[10]{e，2，3，4，5，6}；//C++动态内存分配，可以在申请的同时进行初始化
for（int i=e；i<1e；i++）
printf（“%d”，sep[i]）；
~~~

## 变量的引用

~~~c
int a = 4;
int& ab = a;//去别名，实际上还是表示同一块内存空间
ab = 740；
cout<< a << "  " <<ab<<endl;
输出a和ab的值是一样的；
    
~~~





