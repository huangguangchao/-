## C++模板学习笔记

~~~c
//template <typename T>//T：称为类型模板参数，代表的是一个类型。
template<class T>					//class 可以取代typename，但是这里的class并没有“类”
									//T这个名字可以任意起
T Sub（T tvl，T tv2）
{
    return tvl-tv2；
}

//一：基本范例
//a）模板的定义是以template关键字开头。
//b）类型模板参数T前面用typename来修饰，所以，遇到typename就该知道其后面跟的是一个类型。
//typename可以被class取代，但此处的class并没有“类”
//c）类型模板参数T（代表是一个类型）以前前面的修饰符typename/class都用<〉括起来
//d）T这个名字可以换成任意其他标识符，对程序没有影响。用T只是一种编程习惯。

~~~

