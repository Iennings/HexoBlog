---
title: Java编程思想读书笔记（三）
date: 2016-10-11 03:01:13
tags: Java
---
# 通过异常处理错误
## Java标准异常
RuntimeException，由它继承而来的异常被称为“不受检查的异常”。这种异常属于错误，会自动被捕获，不用我们自己去动手。所以我们可以不用在异常声明中声明方法将被抛出RuntimeException类型的异常。要注意到的是只能在代码中忽略RuntimeException及子类的异常，其他异常都是由编译器强制实施的，而RuntimeException算编程错误。
Exception是所有“被检查的异常”的基类。
## 用finally进行清理
当要把出内存之外的资源恢复到它们的初始状态，就要finally子句，对于没有垃圾回收和构析函数自动调用机制的语言来说，finally可以是程序员保证：无论try快里发生了什么，内存总能的到释放。因为finally字句总是会执行的，所以在一个方法中，可以从多喝点返回，并且可以保证重要的清理工作仍旧会执行：
```
public class MultipleReturns {
	public static void f(int i) {
		System.out.println("Initialization that requires cleanup");
		try {
			System.out.println("Point 1");
			if(i == 1) return;
			System.out.println("Point 2");
			if(i == 2) return;
			System.out.println("Point 3");
			if(i == 3) return;
			System.out.println("End");
			return;
		} finall {
			System.out.println("Performing cleanup");
		}
	}
	public static void main(Stringp[] args) {
		for(int i = 1; i <= 4; i++) {
			f(i);
		}
	}
} /* Output:

Initialization that requires cleanup
Point 1
Performing cleanup
Initialization that requires cleanup
Point 1
Point 2
Performing cleanup
Initialization that requires cleanup
Point 1
Point 2
Point 3
Performing cleanup
Initialization that requires cleanup
Point 1
Point 2
Point 3
End
Performing cleanup
*///:~
```
### 异常使用指南
在下列情况下使用异常：
１）在恰当的级别处理问题。（在知道该如何处理的情况下才捕获异常。）
２）解决问题并且重新调用产生异常的方法。
３）进行少许修补，然后绕过异常发生的地方继续执行。
４）用别的数据进行计算，以替代方法预计会返回的值。
５）把当前运行环境下能做的事情尽量昨晚，然后把相同的异常重抛到更高层。
６）把当前运行环境下能做的事情尽量做完，然后把不同的异常抛到更高层。
７）终止程序。
８）进行简化。（如果你的异常模式是问题变得太复杂，那用起来会非常痛苦也很烦人。）
９）让类库和程序更安全。（这既是在为调试做短期投资，也是在为程序的健壮性做长期投资。）
## 字符串
### 重载 "+" 与StringBuilder
如果是使用String字符串创建的对象，在使用"+"连接符时，编译器会创建一个StringBuilder对象，用以构造最终的String，为每个字符串调用一个StringBuilder的append()方法。最后调用toString()生成结果
如果一个类编写toString()方法时，如果字符串操作比较简单，那就可以依赖编译器，它会合理地为我们构造最终的字符串结果。
### 正则表达式
字符类
.						任意字符
[abc]					包含a,b和c的任何字符（和a|b|c作用相同）
[^abc]					除了a,b和c之外的任何字符（否定）
[a-zA-Z]				从a到z或A到Z的任何字符（范围）
[abc[hij]]				任意a,b,c,h,i和j字符（与a|b|c|h|i|j作用相同）（合并）
[a-z&&[hij]]			任意h,i或j（交）
\s						空白符（空格，tab，换行，换页和回车）
\S						非空白符（[^\s]）
\d						数字[0-9]
\D						非数字[^0-9]
\w						词字符[a-zA-Z0-9]
\W						非词字符[^\w]

边界匹配符
^						一行的起始
$						一行的结束
\b						词的边界
\B						非词的边界
\G						前一个匹配的结束
## 类型信息
### 为什么需要RTTI
在Java中所有类型转换是在运行时进行正确性检查的。这也是RTTI名字的含义：在运行时，识别一个对象的类型。
### Class对象
Class对象就是用来创建类的所有的“常规”对象的
Class的newInstance()方法是实现“虚拟构造器”的一种途径，虚拟构造器允许我们声明：“我不知道你的确切类型，但是无论如何要正确创建你自己”，这句话让我对反射的逐渐有了理解，利用Class对象的这个方法，在反射时，创建这个需要的类型。看下面的一段代码，体验使用的例子中Class的方法
```
//:typeinfo/toys/ToyTest.java
package typeinfo.toys;
import static net.midview.util.Print.*;

interface HasBatteries {

}
interface Waterproof {

}
interface Shoots {

}
class Toy {
	Toy() {
	}
	Toy(int i) {
	}
}

class FancyToy extends Toy implements HasBatteries, Waterproof, Shoots {
	FancyToy() {
		super(1);
	}
}
public class ToyTest {
	static void printInfo(Class cc) {
		print("Class name: " + cc.getName() + " is interface? [" + cc.isInterface() + "]");
		print("Simple name: " + cc.getSimpleName());
		print("Canonical name: " + cc.getCanonicalName);
	}
	public static void main(String[] args) {
		Class c = null;
		try {
			c = Class.forname("typeomfp.toys.FancyToy");
		} catch(ClassNoFoundException e) {
			print("Can't find FancyToy");
			System.exit(1);
		}
		printInfo(c);
		for(Class face : c.getInterfaces())
			printInfo(face);
		Class up = c.getSuperclass();
		Object obj = null;
		try {
			obj = up.newInstance();
		} catch(InstantiationException e) {
			print("Cannot instantiate");
			System.exit(1);
		} catch(IllegalAccessException e) {
			System.exit(1);
		}
		printInfo(obj.getClass());
	 }
} /*Output:
Class name: typeinfo.toys.FancyToy is interface? [false]
Simple name: FancyToy
Canonical name: typeinfo.toys.FancyToy
Class name: typeinfo.toys.HasBatteries is interface? [true]
Simple name: HasBatteries
Canonical name: typeinfo.toys.HasBatteries
Class name: typeinfo.toys.Waterproof is interface? [true]
Simple name: Waterproof
Canonical name: typeinfo.toys.Waterproof
Class name: typeinfo.toys.Shoots is interface? [true]
Simple name: Shoots
Canonical name: typeinfo.toys.Shoots
Class name: typeinfo.toys.Toy is interface? [False]
Simple name: Toy
Canonical name: typeinfo.toys.Toy
*///:~
```
up仅仅只是一个Class引用，在编译器不具备任何更近一步的类型信息。当我们创建新实例时，会得到Object引用，但是这个引用指向的是Toy对象。
### 反射
java反射是在程序运行时，获取某个对象的确切类型，这个确切的类型要求在编译时必须已知，这样可以在运行时识别它，然后获取相关信息，反射可以获得这个java类型的相关Field,Method,以及Constructor。在Class类与java.lang.reflect类库一起对反射提供了支持，在java.lang.reflect类库中提供了Fiedl,Method,Constructor三个类。这些类型的对象都是在运行时期创建的，用以表示未知类里对应的成员。可以用Constructor创建新的对象，用set(),get()方法修改和读取与Field对象相关的对象。对于反射来说，重要的一点是，反射可以读取private修饰符修饰的类，字段，方法，并且可以调用它们。包括私有内部类和匿名类都可以调用。
### 动态代理
动态代理是23种设计模式中最基本的几种设计模式，它的UML图如下所示
动态代理的目的是在与充当中间人的角色实现两个类之间的低耦合性，而且代理可以提供额外的或不同的操作，示例代码也如下所示，
