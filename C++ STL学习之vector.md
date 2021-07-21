

## C++ STL学习之vector

### vector:变长数组（不固定大小的数组），类比于栈

ps:

​		使用前要包含头文件  <vector>以及

​		#include<algorithm>：algorithm意为"算法",是C++的标准模版库（STL）中最重要的头文件之一，提供了大量基于迭代器的非成员模版函数。

​		**函数的调用： vector.对应函数名**

​		size（）//返回元素个数 

​		empty（）//返回是否为空；

​		clear（）//清空元素，不清空内存
​		front（）//返回第一个元素的值

​		back（）//返回最后一个元素的值
​		push_back（x）//压入一个数x

​		pop_back（）//弹出最后一个数
​		begin（）

​		end（）

​		 at()//取出下标

 		c_str()//执行

 		erase()//根据迭代器的位置，删除元素；erase(myvector.begin() + 3)

​		 fill//填充

​		 find

​		 find_if

​		 find_if_not

​		 insert()插入元素；insert(myvector.begin() + 3, 998);//在第4个位置插入

​		sort（）由小到大排序，用法见下文

​		swap()//交换两个vector

**Iterators（迭代器）**



​		迭代器可被用来访问一个容器类的所包函的全部元素，其行为像一个指针。举一个例子，你可用一个迭代器来实现对vector容器中所含元素的遍历。

​		//正向迭代器iterator，begin()返回一个迭代器，它指向容器c的第一个元素，end()返回一个迭代器，它指向容器c的最后一个元素的下一个位置

​		//反向迭代器reverse_iterator，rbegin()返回一个逆序迭代器，它指向容器c的最后一个元素，rend()返回一个逆序迭代器，它指向容器c的第一个元素前面的位置


~~~c
vector<int>v;//相当于定义一个int类型的变长数组v
//vector:变量类型，相当于string int double
//<int>传入参数的类型,可以是float，double，string，int,Node...
//v是变量名
v.push_back(4);//压入一个值4
v.push_back(2);
v.push_back(8);

~~~

~~~c
vector<int>：：iterator it；//定义一个类型为vector<int>的迭代器
    for（it =v1.begin（）；it l=vl.end（）；it++）
     cout<< *it <<endl；// * 代表取这个it对应的元素
~~~

[]的使用和数组一样

~~~c
v[0] = 1000;
cout <<v[0]<<endl;
是可以这样使用的，但是必须是先开辟了空间才能这样赋值，只定义了vector v就这样赋值时错误的！
~~~

~~~c
vector的初始化
vector<int>abc（10）；//初始化了10个默认值为0的元素
~~~

~~~c
sort(v.begin(),v.end());//由小到大排序
~~~

清空vector的内存

~~~c
/*C++中vector的clear（）只是清空vector，并不会清空开的内存。用一种方法可以清空vector的内存。先定义一个空的vectorx，然后用需要清空的vector和x交换，因为x是局部变量，所以会被系统回收内存（注意大括号一定不能去掉）。*/
vector<int> v;
{
    vector<int>x;
    V. swap(x);
}
~~~

