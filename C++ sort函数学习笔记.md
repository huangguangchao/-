## C++ sort函数学习笔记

使用sort函数需要#include<algorithm>

也可以直接包含万能头文件#include<bits/stdc++.h>

同时要声明命名空间：using namespace std;

### sort函数的基本用法

~~~c
sort（a+m，a+n）；//[a+m，a+n）范围内的元素进行排序
sort（a+m，a+n，cmp）；//cmp是函数或仿函数
~~~

### 想从大到小排序怎么办？

~~~c
/*原理是为真的时候，第一个数字放前面*/
bool mycmp（int a，int b）
{
	if（a>b）
	return 1；
    return 0；
}
    
    int main（）
    {
	int i；
    sort（a+0，a+10，mycmp）；//数组名+数字的本质是指针操作for（i=0；i<10；i++）
	printf（"%d"，a[i]）；
        return 0；
    }
~~~

**c++内置了两个比较大小的仿函数less<>，greater<>分别进行大小的比较。**

less是从小到大排序，greater是从大到小排序

less和greater的参数都是要比较的数据的数据类型

### 对结构体进行排序

1、大小比较函数

2、重载运算符

3、仿函数

~~~c
struct node{
int a，b；
//重载“<”
bool operator<（node B）{
	if（a！=B.a）
    	return a<B.a；
   		return b<B.b；
}
~~~

**什么是友元函数？**

在结构体里重载则排序的时候自动使用重载符号，在外面重载则要手动使用，

使用友元函数也可以在sort的时候自动使用。

类的友元函数是定义在类外部，但有权访问类的所有私有（private）成员和保护（protected）成员。尽管友元函数的原型有在类的定义中出现过，但是友元函数并不是成员函数。

友元可以是一个函数，该函数被称为友元函数；友元也可以是一个类，该类被称为友元类，在这种情况下，整个类及其所有成员都是友元。

如果要声明函数为一个类的友元，需要在类定义中该函数原型前使用关键字 **friend**

---------来自菜鸟教程