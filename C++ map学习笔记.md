##  C++ map学习笔记

使用map得包含map类所在的头文件

\#include <map>



map时一个关联容器，提供一对一的映射关系。

~~~c
map<int,char>oneMap;
~~~

### <key，value>

第一个称为关键字（key），别名时first，每个关键字只能在map中出现一次

第二个称为**该关键字的值**（value），别名是second。

### 用途

当数据结构具有对应关系的时候可以采用map

如”姓名-电话号码“

Alice 13824611111
Bob13902382222
Tom13002500333
Jack 18011114444

就可以用map

~~~c
map< string,string>friends;
friends. insert("Alice","13824611111"); 
friends. insert("Bob","13902382222");
friends. insert("Tom","13002500333");
friends. insert("Jack","18011114444");
~~~

### map对象创建的方式及函数

定义map对象：

~~~c
//map<key,value>mapName
//key和value的类型可以是int、char、string、long、node等
~~~

插入对象（四种方式）

 ~~~c
 oneMap. insert(pair<int, char>(1,'A')); 
 oneMap. insert(make_pair(2,'B'));//个人喜欢这个*
 oneMap. insert(map<int, char>:: value type(3,'C'));                              
 oneMap[4] = "D";
 //ps:当map中有这个关键字时，insert操作是不能在插入数据的，但是用数组方式就不同了，它可以覆盖以前该关键字对应的值
 ~~~

迭代器函数

~~~c
//正向迭代器函数
begin()//返回第一个元素的迭代器
end()//返回最后一个元素的迭代器

    
//反向迭代器函数    
rbegin()//返回倒数第一个元素的迭代器
rend()//返回倒数最后一个元素的迭代器
~~~

查找函数

~~~c++
find(X)返回元素x的迭代器，为空则返回map::end()位置
oneMap.find(1);或者oneMap.find('A');
~~~



clear()和empty()

~~~c
oneMap.clear()；
    if(oneMap.empty())
    {
		cout<<"容器为空”<<end1;
    }
    else
    {
        cout<<"容器不为空”<<end1;
    }
~~~

equal_range(x)

~~~
返回元素X的迭代器；X指的是关键字
cout<<（*oneMap.equal_range（2）.first）.first//输出关键字为2的迭代器的关键字
<<"->"<<oneMap.equal_range（2）.first->second//输出关键字为2的迭代器的数值
<<end1；
//输出结果：2->'B'
~~~

**equal_range.first**和**lower_bound**返回值是一样的，都是指向第一个相等元素（可以这样认为）的迭代器，无论最后找到或者找不到匹配的关键字，它们都相等。

同理，**equal_range.second**和**upper_bound**的返回值一样，都是指向最后一个相等元素的后一个元素的迭代器，如果找不到关键字，那么将会得到一个安全的关键字插入位置。





容量

~~~c++
//size（）   max_size（）
cout<<"当前大小为："<<oneMap.size（）<<end1；cout<<"最大容量为："<<oneMap.max_size（）<<end1；
//size()返回当前容器的大小,max_size返回当前容器最大容量
~~~



交换两个map

~~~c++
swap()；参数是map类型
~~~

