## c++类的继承

类的继承是 is kind of 的关系；比如猫是一种动物，视频文件是一种文件等。



​			语法：class B : public  A {}
​			表示类**B继承于类A**，把**A称为父类**（基类），把**B称为子类**（派生类）。

​			当B继承于A时，则自动地将父类中的所有public成员继承。

​		例如，

~~~c 

class VideoTutorial:public Tutorial
{
    
};

class Tutorial
{
    public：
     char name[32]；
   	 char author[16]；
    public：
     void ShowInfo（）；
}；
~~~

​			则在**VideoTutorial**类中也具有了这些成员，而不必显式写出。子类只需要把自己的独有的那部分特性写出来。



​			当一个类成员被修饰为protected的时候，有以下规则成立：
​			（1）该成员不能被外部访问，同private
​			（2）该成员可以被子类继承，同public所以，public和protected的成员都能够被子类继承

