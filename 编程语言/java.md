# Java基础

(参考Core Java和Java in a Nutshell)

---
---
### Java常识

Java 编程环境出现于 20 世纪 90 年代末,由 Java 语言和 runtime 组成。runtime 也叫 Java 虚
拟机(Java Virtual Machine,JVM)。

---

#### 什么是JVM？
JVM 是一个程序,提供了运行 Java 程序所需的运行时环境。如果某个硬件和操作系统平
台没有相应的 JVM,就不能运行 Java 程序。
Java 程序一般都在命令行中启动,例如:
```
java <arguments> <program name>
```
这个命令会在操作系统的一个进程中启动 JVM,提供 Java 运行时环境,然后在刚启动的
(空)虚拟机中运行指定的程序。

有一点很重要,你要知道:提供给 JVM 运行的程序不是 Java 语言源码,源码必须转换
(或编译)成一种称为 Java 字节码的格式。提供给 JVM 的 Java 字节码必须是类文件格式,
其扩展名为 .class。

JVM 是字节码格式程序的解释器,一次只执行字节码中的一个指令。而且,你还要知道,
JVM 和用户提供的程序都能派生额外的线程,所以用户提供的程序中可能同时运行着多个
不同的函数。

特性：
* 包含一个容器,让应用代码在其中运行;
* 较之 C/C++,提供了一个安全的执行环境;
* 代开发者管理内存;
* 提供一个跨平台的执行环境。

---

#### 什么是字节码？
字节码和运行于硬件处理器中的机器码不太一样。计算机科学家视字节码为一种“中间表现形式”,处在源码和机器码之间。
字节码的目的是,提供一种能让 JVM 解释器高效执行的格式。

---

#### javac是编译器吗？
编译器一般生成机器码,而 javac 生成的是和机器码不太一样的字节码。javac 非常像编译器的“前半部分”,它生成的中间表现形式
可以进一步处理,生成机器码。不过,因为类文件的生成是构建过程中单独的一步,类似于 C/C++ 中的编译,所以很多开
发者都把运行 javac 的操作称为编译。

---

#### Java是解释性语言吗？
JVM 基本上算是解释器(通过 JIT 编译大幅提升性能)。可是,大多数解释性语言(例如
PHP、Perl、Ruby 和 Python)都直接从源码解释程序(一般会从输入的源码文件中构建一
个抽象句法树)。而 JVM 解释器需要的是类文件,因此当然需要多一步操作,即使用 javac
编译源码。

---

##### 总结：Java是一门弱类型，静态的，解释型语言，经过javac编译的字节码可以跨平台解释运行在JVM上。

---
---

### 运行机制

#### Java是如何处理异常的？

Java 解释器执行 throw 语句时,会立即停止常规的程序执行,开始寻找能捕获或处理异常
的异常处理程序。异常处理程序使用 try/catch/finally 语句编写,下一节会介绍。Java
解释器先在当前代码块中查找异常处理程序,如果有,解释器会退出这个代码块,开始执
行异常处理代码。异常处理程序执行完毕后,解释器会继续执行处理程序后的语句。

如果当前代码块中没有适当的异常处理程序,解释器会在外层代码块中寻找,直到找到为
止。如果方法中没有能处理 throw 语句抛出的异常的异常处理程序,解释器会停止运行当
前方法,返回调用这个方法的地方,开始在调用方法的代码块中寻找异常处理程序。Java
通过这种方式,通过方法的词法结构不断向上冒泡,顺着解释器的调用堆栈一直向上寻
找。如果一直没有捕获异常,就会冒泡到程序的 main() 方法。如果在 main() 方法中也没
有处理异常,Java 解释器会打印一个错误消息,还会打印一个堆栈跟踪,指明这个异常在
哪里发生,然后退出。

---

#### Java中如何区分已检异常和未检异常？

想区分已检异常和未检异常,记住两点:异常是 Throwable 对象,而且异常主要分为
两类, 通过 Error 和 Exception 子类标识。只要异常对象是 Error 类,就是未检异常。
Exception 类还有一个子类 RuntimeException , RuntimeException 类的所有子类都属于未检
异常。除此之外,都是已检异常。

---

#### Java中的变长参数列表如何实现？

变长参数列表的声明方式为,在方法最后一个参数的类型后面加上省略号( ... ),指明最
后一个参数可以重复零次或多次。

参数列表中只能有一个省略号,而且只能出现在最后一个参数中。

例如：
```
public static int max(int first, int... rest) {
    int max = first;
    for(int i : rest) { // 合法,因为rest其实就是数组
        if (i > max) max = i;
    }
    return max;
}
```
上面的 max() 方法和下面这个没有区别:
```
public static int max(int first, int[] rest) {
    /* 暂时省略主体 */
}
```
---

#### Java中数组的拷贝机制是怎样的？
参考https://blog.csdn.net/qq_36883748/article/details/79950993

1. 循环拷贝

其实没什么好说的啦，就是用一个for循环进行元素的逐个拷贝，是浅拷贝，拷贝速度比较慢； 

2. System.arraycopy（浅拷贝）

这个是系统提供的拷贝方式，它是浅拷贝，也就是说对于非基本类型而言，它拷贝的是对象的引用，而不是去新建一个新的对象。通过它的代码我们可以看到，这个方法不是用java语言写的，而是底层用c或者c++实现的，因而速度会比较快。

arraycopy() 方法的作用简单明了,但使用起来有些难度,因为要记住五个参数。第一个
参数是想从中复制元素的源数组;第二个参数是源数组中起始元素的索引;第三个参数是
目标数组;第四个参数是目标索引;第五个参数是要复制的元素数量。就算重叠复制同一个数组, arraycopy() 方法也能正确运行（将数组自身整体做移动）。

3. Arrays.copyOf（浅拷贝）

实际上它调用的就是System.arraycopy，所以肯定也是浅拷贝。

4. Object.clone

clone()比较特殊，对于对象而言，它是深拷贝，但是对于数组而言，它是浅拷贝。 
对于数组而言，它不是简单的将引用赋值为另外一个数组引用，而是创建一个新的数组。但是我们知道，对于数组本身而言，它的元素是对象的时候，本来数组每个元素中保存的就是对象的引用，所以，拷贝过来的数组自然而言也是对象的引用，所以对于数组对象元素而言，它又是浅拷贝。

---

#### Java中堆和栈的区别？

参考https://blog.csdn.net/mulinsen77/article/details/86557241

首先要明确Java中基本类型和引用类型的区别。

8种基本数据类型：
* boolean
* char
* byte
* short
* int
* long
* float
* double

5种引用类型：
* 类
* 数组
* 接口
* 枚举
* 注解

1. 栈：

函数中定义的基本类型变量，对象的引用变量都在函数的栈内存中分配。

栈内存特点，数数据一执行完毕，变量会立即释放，节约内存空间。

栈内存中的数据，没有默认初始化值，需要手动设置。

 2. 堆：

堆内存用来存放new创建的对象和数组（即引用类型）。

堆内存中所有的实体都有内存地址值。

堆内存中的实体是用来封装数据的，这些数据都有默认初始化值。

堆内存中的实体不再被指向时，JVM启动垃圾回收机制，自动清除，这也是JAVA优于C++的表现之一（C++中需要程序员手动清除）。

---

#### Java中的几个常用术语分别代表什么含义？
分别是覆写override，隐藏hiding，重载overload，遮蔽shadowing，遮盖obscuring

参考https://blog.csdn.net/devilmaycc/article/details/22792023

* Overriding(覆写,又称为重写）
一个实例方法可以override它的父类中可以访问的具有相同签名的所有实例方法。
```
class Base {
public void func() { }
}
class Derived extends Base {
public void func() { } // overrrides Base.f()
}
```


* Hiding（隐藏）
静态方法，成员类型都会隐藏他的父类中可以访问的具有相同名字的域。
```
class Base {
public static void f() { }
}
class Derived extends Base {
public static void f() { } // hides Base.f()
}
```
注意：静态方法即类方法的调用不同于实例方法，不是和实例绑定的，通过类名来调用。


* Overloading (重载)
类中的方法可以重载类中的其他的方法，只要他们具有相同的名字和不同的签名(参数个数不同，参数，类型不同，如果仅仅是返回值不同的话，编译报错)
```
class CircuitBreaker {
      public void f(int i){ } // int overloading
      public void f(String s) { } // String overloading
}
```

* Shadowing(遮蔽)
当前作用域一个变量，方法或者类型可以遮蔽其他其他作用域的具有相同名字的变量，方法和类型
```
class WhoKnows {
      static String sentence = "I don't know.";
}
public static void main(String[] args) {
       String sentence = "I know!";// shadows static field
       System.out.println(sentence);// prints local variable
}
```

* Obscuring（遮盖）
在同一个作用范围中，如果出现了具有相同名字的变量，类型(方法，类，接口等)，包名，变量会遮盖类型和包，类型回遮盖包，其实只用遵守java的命名规范就可以消除产生遮盖的可能性
```
public class Obscure {
static String System; // Obscures type java.lang.System
}
public static void main(String[] args) {
// Next line won't compile: System refers to static field
System.out.println("hello, obscure world!");
}
```
父子类之间也会出现字段遮盖，可用super调用父字段。

---










---
---





### 