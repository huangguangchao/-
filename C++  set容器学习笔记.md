##  C++  set容器学习笔记

特点：

1.自动去重

2.升序排序

### 1.set容器的定义
~~~c
set<typename>name；//定义一个名字为name数据类型为typename的set容器
typename：任意+STL（>>  加上空格）//数据类型可以是任意类型或者STL的其他容器
~~~

### 2.set容器的访问方式
迭代器（迭代器+整数vector string）

### 3.set容器的富用函数
insert（）             时间复杂度logN，参数是typename类型，插入一个元素
find（） 			   时间复杂度log N，参数是typename类型，返回指向某个值的元素的迭代器

earse（）	         时间复杂度 1 //可以删除单个元素，也可以删除一定范围内的元素，删除单个元素用法和find一样，删除一定范围内的元素：one. erase (one. find(3), one. end()）;括号里面输入一个范围的迭代器

size()                     求集合中有多少个元素，不用输入参数

clear（）        	  时间复杂度 logN，清空所有元素，不用输入参数

get_allocator()     返回集合的分配器

key_comp()	      返回一个用于元素间值比较的函数

value_comp()	   返回一个用于比较元素间的值的函数

max_size()		    返回集合能容纳的元素的最大限值

 count(…)              返回某个值元素的个数  ——等价于find(…)//集合中一个元素最多出现一次,所以它的返回值                                                   只有0和1

empty()                 如果集合为空，返回true 

### 4.set容器的用途

自动去重并且升序排序