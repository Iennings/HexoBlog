---
title: Java编程思想读书笔记（二）
date: 2016-09-04 00:26:31
tags: Java
---
# 复用类
## final 关键字
1.final 对基本类型运用时，能让基本类型数值保持不变，但对于对象引用时，能够保持对象的引用不变，但是对于对象自身的是可以被修改的，这个在不可变对象使用中要注意到这个问题。
# 多态
## 转机
1.导出类无法覆盖基类的私有方法，在向上转型中，调用的私有方法时，得到的是基类的私有方法运行的结果。
只有非peivate方法不会被覆盖。
2.当导出类转型为基类时，调用的任何域访问操作都将由编译器解析，因此不是多态的，所以当该对象使用某个域的值时，得到的结果是基类的域的结果，不是子类相同域的结果。还有静态方法也不具有多态性。
## 构造器和多态
1.创建对象调用构造器时，初始化的实际过程：
１）在其他对象任何事物发生之前，将分配给对象的存储空间初始化为二进制的零
２）调用基类构造器。不断递归下去，从最顶层的基类到最底层的导出类。（还有补充，请看下面的代码）
３）按声明顺序调用成员的初始化方法。
４）调用导出类构造器的主体
```
class Glyph {
	void draw() {
		System.out.println("Glyph.draw()");
	}
	Glyph() {
		System.out.println("Glyph() before draw()");
		draw();
		System.out.println("Glyph() after draw()");
	}
}

class RoundGlyph extends Glyph {
	private int radius = 1;
	RoundGlyph(int r) {
		radius = r;
		System.out.println("RoundGlyph.RoundGlyph(), radius = " + radius);
	}
	void draw() {
		System.out.println("RoundGlyph.draw(), radius = " + radius);
	}
}

public class PolyConstructors {
	public static void main(String[] args) {
		new RoundGlyph(5);
	}
} /* Output:
Glyph() before draw()
RoundGlyph.draw(), radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(), radius = 5
*///:~
```
<!--more-->
在这这段代码中有子类继承的方法在基类构造器中被调用时，radius的值还没被子类给初始化，所以目前是为0,而且只初始化对应别调用的构造器，域不会被初始化，只有导出类才按声明顺序调用成员的初始化方法。像==private int radius = 1;== 这样的成员会被初始化。
# 接口
## 接口
1.接口中方法声明无论是显示还是隐式都是public的，而且域隐式是static和final的
2.在嵌套接口中，如果一个类中的接口中是私有的，继承该接口的内部类在实现该接口方法后，由于实现一个private接口只是一种方式，它可以强制接口中的方法定义不要添加任何类型信息（也就是说，不允许向上转型），代码如下
```
class A {
	private interface D {
		void f();
	}
	public class Dimp implements D {
		public void f() {
		}
	}
	public D getD() {
		return new Dimp();
	}
	private D dRef;
	public void receiveD(D d) {
		dRef = d;
		dRef.f();
	}
}
public class B {
	public static void main(String[] args) {
		A a = new A();
		/×
			*如果使用了A.D ad = a.getD();是会出错的，因为private接口不允许向上转型，报错如下:
			*D has private access in A
			*而且就算使用A.Dimp dimp = a.getD();也会出问题,报错如下:
			*inCompatible types: D cannot be converted to A.Dimp
		*/
		A a2 = new A();
		a2.receiveD(a.getD());
	}
}
```
# 内部类
## 内部类与控制框架
1) 控制框架的完整实现是由单个的类创建，从而使得实现的细节被封装了起来。内部类用来表示解决问题所必需的各种不同的action()。
2) 内部类能够很容易的访问外围类的任意成员，所以可以避免这种实现变得笨拙。如果没有这种能力，代码将变得令人讨厌，以至于你肯定会选择别的方法。
# 持有对象
## 基本概念
1.Java容器类类库的用途是"保存对象"，并将其划分为两个概念：
１）Collection.一个独立元素的序列，它的子类由List(必须按照插入的顺序保存元素)，Set(不能有重复的元素)，Queue(按照排队的规则来确定对象产生的顺序)
２）Map.一组成对的"键值对"，和Python中的字典类似，它将一个对象与另一个对象关联起来，组成一张映射表，成为键的对象可以查找出与其对应的对象(值)。
## 容器的打印
1.HashSet存储速度是最快的，TreeSet按照比较结果的升序保存对象。LinkedHashSet,它按照被添加的顺序保存对象，Map类的子类也与Set类的一样
## List
1.ArrayList，和LinkedList的区别在于，前者的随机访问速度比较低，但是其在List中间进行的插入和删除代价成本比较高昂，因为ArrayList对中间的访问是真正的随机访问
## 迭代器
1.迭代器可以将容器给统一起来，它的工作就是遍历和选择序列对象，其中Iterator的子类ListIterator它只能用于各种List的访问，它具有的优点是，可以双向移动，还可以产生前后索引，以及可以使用set()方法替换它访问的最后一个元素，以及可任意通过使用List的方法listIterator(n),确定一开始的就指向列表索引为n的元素处的ListIterator。
## 队列和栈
1.栈(Stack)是后进先出的，而队列(Queue)是先进先出，对于栈的功能可以用LinkedList实现，如下代码
```
Public class Stack<T> {
	private LinkedList<T> storage = new LinkedList<T>();
	public void push(T v) {
		storage.addFirst(v);
	}
	public T peek() {
		return storage.getFirst();
	}
	public T pop() {
		return storage.removeFirst();
	}
	public boolean empty() {
		return storage.isEmpty();
	}
	public String toString() {
		return storage.toString();
	}
}
```
2.队列(Queue),它是一个接口，所以LinkedList是实现了它的接口，所以可以通过向上转型为Queue，offer()方法是将一个元素插入到队尾，或者返回false。peek()和element()都将在不移除的情况下返回队头，但是peek()方法在队列为空时返回null，而element()会抛出NoSuchElementException异常。poll()和remove()方法将移除并返回队头，但是poll()在队列为空是返回null，而remove()会抛出NoSuchElementException异常。
对于PriorityQueue(优先队列)，当调用offer()方法插入一个对象时，这个对象将被排序，默认的排序是自然顺序，如果需要自己定义排序，可以通过提供自己的Comparator来修改这个顺序，当调用peek()，poll()和remove()方法时，获取的是元素将是队列中优先级最高的元素。

