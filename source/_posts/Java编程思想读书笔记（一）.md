---
title: Java编程思想读书笔记（一）
date: 2016-08-16 00:09:06
tags: Java
---
《Thinking in Java》这本书几个月前就在看了，但是自己的Java基础比较差，到后面一些东西看得有些懵逼，上个月勉强看完了。自己现在做一些读书笔记吧，也把自己看这本书的获得知识以及思考分享给大家，我会画出这本书的思维导图给大家看下，我的文笔很烂，表达不清楚，或者说的太多废话，望大家不要介意，还有一些东西可能由错误，有什么意见也可以指点出，谢谢了！
# 对象导论
本章是让没有了解面向对象程序设计的人，进行了解的，是背景性，补充性材料。Alan Kay总结的面向对象语言的五基本特性：
1.万物皆为对象
2.程序是对象的集合，它们通过发送消息老告知彼此所要做的事
3.每个对象都有自己的由其它对象所构成的存储
4.每个对象都拥有其类型
5.某一特定类型的所有对象都可以接收同样的消息
理解：所有东西都可以分类，比如书这一类，人这一类，那么人在仔细分，就是具体到我们每个人了，我们是独一无二的，我们单独自己就是一个对象，是人这一类中的一个例子，而程序就好比我们一个整体，比如一个班级，包含是一个班人，就是我们的集合，我们一个人是一个对象，而班级像程序一样也要运行，班主任告知学生和任课老师要做的事，任课老师也告知彼此要做的事。而第三点，万物都是对象，所以具体的书本也是对象，以及器官也是对象，那么我们每个人也是由其它的对象组成的。而每个对象都有自己的分类，比如人类，书类等。比如上课了，学生都要在自己笔记本上做笔记，学生是一个特定的类型，学生们都接收了这个消息。
每个对象都由自己的状态，行为和标识，状态就好比类里变量赋予了值，行为就是类里定义的方法，但由可以根据对象的不同使方法产生不同的结果，好比 东施效颦 产生了不同的结果，而对象都是特定的独一无二的，在内存里每个对象都由唯一一个地址，我们人也一样，就算是双胞胎，也有不一样的因素。
接口就好比人会思考这个技能一样，原本所有动物都没有的，但是人类添加了这个接口，实现了这个接口定义的思考这个方法。
所谓的继承就是一层层关系下来的，比如书，书有很多种类，文学，科普什么的，然后文学这类算继承了书这一类，而文学里还有各种分类，最后到具体的某一本书名了，所以这是一步一步地继承下去的。
封装就像任课老师们教的课一样，而班主任只需要告诉任课老师们把学生教好，至于任课老师所教的课的东西，怎么教好学生都不用班主任管理，每个任课老师（一个接口），就每个任课老师讲课这些方式就像接口内部实现一样，以及接口外部调用者（班主任），这是就是封装了！
而多态就是好比一道母题，根据它的特性生成很多子题，子题继承了母题，不过更改了母题的条件，这样让母题有多种实现的方式，不同子题对母题的不同实现方式！
<!--more-->
# 一切都是对象
1.基本类型中在Java SE5的自动装箱功能中，如：
```
char c = 'X';
Character ch = 'X';
```
这两者的值是相等的，但是它们有一定的区别，char是基本类型，只有值，像书上说的引用“自动”变量，这个是直接存储“值”; 而Character是装箱基本类型(就是书中说的包装器类型)，如果采用`Character ch1 = new Character('X')`这样会出现的ch1是一个对象引用，两个这样的Character对象进行比较时，值会相同，但是存储两个对象的地址会不同，所以使用"=="进行比较时，是会不相等的，如果是采用`Character ch = 'X'`这种形式，变量存储的直接就是一个值;而且装箱类型有null这个非功能值，基本类型没有这个非功能。初始化时如果没有对Character 对象赋值默认值是null，而基本类型的就会随机产生一个值;
2.Class
这里直接使用一张图来简单说明Class(类)
![类的内容](/image/classContent.png)
# 操作符
1.这些比较简单，有一个对于新手来说要注意的，就是==和!=操作符，这个比较的是两个对象的引用，如果比较两个对象内容是否相等，使用equals()方法A.qeuals(B);
2.无符号右移操作符>>>，无论正负，都向高位插入0;
3.在Java中有一个容易犯错的地方
```
while(x = y) {
	//...
}
```
这在C/C++中运行可以这个值可以转化为布尔值，但是在Java中这个结果不是布尔值，那么编译器没有得到它所期望的布尔值，就会抛出一个编译时错误
# 初始化清理
## 1.重载和重写（覆盖）
这两个是不同意思，不过它们与动态连接构成了多态性。不过这个我们到后面出现的地方再讲，先把重写和重载讲解一下。
### 重载：
主要用于构造器，创建一个新的对象时，要对对象进行初始化，那么就需要构造器了，但是如果初始化时另外一个新对象传入的参数与原来不一样，那么就需要新的构造器了。重载其实就是一个类中，拥有多个构造器，它们的参数类型不同。这样就有一个好处，不用因为要传入不同类型的参数就要新建出新的类。这里列出一段代码，来显示重载
```
public static OverrideLoad {
	public OverrideLoad() {

	}
	public OverrideLoad(int i) {
		... //对象初始化具体处理
	}
	public OverrideLoad(Number i)
	...
}
```
重载有时候在参数数目相同，类型类似的参数下容易搞混淆了，因为方法名字都是相同的，不容易区分。重载会涉及到，传递的某个基本类型参数，如果这个参数太大比形式参数大，那么它会通过类型转换来执行窄化转换，如果是形式参数太大，就会把形式参数的类型给提升。
### 重写：
当子类继承父类后，如果子类需要用到和父类相同的方法，但是方法内的内容有些不同，那么这时候通过重写父类的方法，变成合适子类的使用场景的方法。这个帮助Java只能单继承方式有效解决了这个不足，多态性就具有这个好处，同时解决Java单继承问题的还有抽象类和接口这两种重要手段！这列也给出一段代码
```
public class Father {
	public void dosomething() {
		System.out.println("Watching TV");
	}
}
public class Son extends Father {
	/**
	  * 子类重写父类的方法，注意重写父类方法的时，
	  * 子类方法上的关键字和参数要和父类一样，方法内容可以不同。
	*/
	@Override
	public void dosomething() {
		System.out.println("Doing homework");
	}
}
```
## 构造器初始化
1.对与一个对象初始化过程中，先初始化类中的静态对象（只会初始一次，以后新建对象，不会再初始化），然后是非静态对象，最后是构造器。
2.对于static 关键字用法注意，static 方法内部不能调用非静态方法，反过来来可以。因为如果通过类本身调用static方法那么，因为调用的方法还没有来得及初始化，就会出现问题，不过有个困惑，我把static方法内调用的方法是属于在static方法所在类的方法。

