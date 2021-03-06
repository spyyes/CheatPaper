# Java基础部分

## 标识符

标识符：标识符是用来给类、对象、方法、变量、接口和自定义数据类型命名的。

**48个**关键字，**2个**保留字(const, goto)，**3个**特殊的直接量(true false null)

![img](https://img-blog.csdn.net/20160131123826738?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQv/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

​    Java标识符由数字，字母和下划线（_），美元符号（$）组成。在Java中是区分大小写的，而且还要求首位不能是数字。最重要的是，Java关键字（于该文章后半部分）不能当作Java标识符。

Java 9规定：不允许单独使用下划线作为分隔符。

## **synchronized**

synchronized 是 Java 中的关键字，是利用锁的机制来实现同步的。

**锁机制的两个特性：**

- 互斥性：即在同一时间只允许一个线程持有某个对象锁，通过这种特性来实现多线程中的协调机制。
- 可见性：必须确保在锁被释放之前，对共享变量所做的修改，对于随后获得该锁的另一个线程是可见的（即在获得锁时应获得最新共享变量的值），否则另一个线程可能是在本地缓存的某个副本上继续操作从而引起不一致。

**对象锁和类锁：**

- 对象锁：在 Java 中，每个对象都会有一个 monitor 对象，这个对象其实就是 Java 对象的锁，通常会被称为“内置锁”或“对象锁”。类的对象可以有多个，所以每个对象有其独立的对象锁，互不干扰。
- 在 Java 中，针对每个类也有一个锁，可以称为“类锁”，类锁实际上是通过对象锁实现的，即类的 Class 对象锁。每个类只有一个 Class 对象，所以每个类只有一个类锁。

**synchronized 的用法分类**

- 根据修饰对象分类-- synchronized 可以修饰方法和代码块
  - 修饰代码块
    - synchronized(this|object) {}
    - synchronized(类.class) {}
  - 修饰方法
    - 修饰非静态方法
    - 修饰静态方法

- 根据获取的锁分类

  - 获取对象锁
    - synchronized(this|object) {}
    - 修饰非静态方法

  - 获取类锁
    - synchronized(类.class) {}
    - 修饰静态方法

**注意：**

- synchronized关键字不能继承。对于父类中的 synchronized 修饰方法，子类在覆盖该方法时，默认情况下不是同步的，必须显示的使用 synchronized 关键字修饰才行。
- 在定义接口方法时不能使用synchronized关键字。
- 构造方法不能使用synchronized关键字，但可以使用synchronized代码块来进行同步。
- 所以对象锁和类锁是独立的，互不干扰。


  **ref:**<https://juejin.im/post/594a24defe88c2006aa01f1c#heading-12>



## java 注解

@override 

1、可以当注释用,方便阅读；
2、编译器可以给你验证@Override下面的方法名是否是你父类中所有的，如果没有则报错。例如，你如果没写@Override，而你下面的方法名又写错了，这时你的编译器是可以编译通过的，因为编译器以为这个方法是你的子类中自己增加的方法。



## extends vs. implements 

　　在类的声明中，通过关键字extends来创建一个类的子类。一个类通过关键字implements声明自己使用一个或者多个接口。extends 是继承某个类, 继承之后可以使用父类的方法, 也可以重写父类的方法;implements 是实现多个接口, 接口的方法一般为空的, 必须重写才能使用.

　extends是继承父类，只要那个类不是声明为final或者那个类定义为abstract的就能继承，JAVA中不支持多重继承，但是可以用接口 来实现，这样就要用到implements，继承只能继承一个类，但implements可以实现多个接口，用逗号分开就行了;比如 `class A extends B implements C,D,E`;

接口一般是只有方法声明没有定义的，那么java特别指出实现接口是有道理的，因为继承就有感觉是父类已经实现了方法，而接口恰恰是没有实现自己的方法，仅仅有声明，也就是一个方法头没有方法体。因此你可以理解成接口是子类实现其方法声明而不是继承其方法。

继承可以使用 extends 和 implements 这两个关键字来实现继承，而且所有的类都是继承于 java.lang.Object，当一个类没有继承的两个关键字，则默认继承object（这个类在 **java.lang** 包中，所以不需要 **import**）祖先类。

extends 继承：

- 子类拥有父类非 private 的属性、方法。
- Java 的继承是单继承，但是可以多重继承，单继承就是一个子类只能继承一个父类，多重继承就是，例如 A 类继承 B 类，B 类继承 C 类，所以按照关系就是 C 类是 B 类的父类，B 类是 A 类的父类，这是 Java 继承区别于 C++ 继承的一个特性。C++可以多继承。
- 

### implements关键字

使用 implements 关键字可以变相的使java具有多继承的特性，使用范围为类继承接口的情况，可以同时继承多个接口（接口跟接口之间采用逗号分隔）。

### super 与 this 关键字

super关键字：我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。

this关键字：指向自己的引用。

### final关键字

final 关键字声明类可以把类定义为不能继承的，即最终类；或者用于修饰方法，该方法不能被子类重写：

### 构造函数

子类是不继承父类的构造器（构造方法或者构造函数）的，它只是调用（隐式或显式）。如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 **super** 关键字调用父类的构造器并配以适当的参数列表。

如果父类构造器没有参数，则在子类的构造器中不需要使用 **super** 关键字调用父类构造器，系统会自动调用父类的无参构造器。

## strictfp

　　strictfp, 即 **strict float point** (精确浮点)。

　　strictfp 关键字可应用于类、接口或方法。使用 strictfp 关键字声明一个方法时，该方法中所有的float和double表达式都严格遵守FP-strict的限制,符合IEEE-754规范。当对一个类或接口使用 strictfp 关键字时，该类中的所有代码，包括嵌套类型中的初始设定值和代码，都将严格地进行计算。严格约束意味着所有表达式的结果都必须是 IEEE 754 算法对操作数预期的结果，以单精度和双精度格式表示。

| native    | 用来声明一个方法是由与计算机相关的语言（如C/C++/FORTRAN语言）实现的 |
| --------- | ------------------------------------------------------------ |
| transient | 声明不用序列化的成员域                                       |

| volatile | 表明两个或者多个变量必须同步地发生变化 |
| -------- | -------------------------------------- |
|          |                                        |

java.lang.Object

## Serilizable接口

Java为我们提供了Serializable接口，这是一个空接口；如果一个类实现了Serializable接口，那么就代表这个类是自动支持序列化和反序列化的，毋须我们编程实现。如果一个类没有实现Serializable接口，那么默认是不能被序列化的，除非使用其他办法。这些办法，会在后续的文章的说明。

## throw vs. throws

**throw与throws的比较**
1、throws出现在方法函数头；而throw出现在函数体。

2、throws表示出现异常的一种可能性，并不一定会发生这些异常；throw则是抛出了异常，执行throw则一定抛出了某种异常对象。

3、两者都是消极处理异常的方式（这里的消极并不是说这种方式不好），只是抛出或者可能抛出异常，但是不会由函数去处理异常，真正的处理异常由函数的上层调用处理。

**好的编程习惯：**

1.在写程序时，对可能会出现异常的部分通常要用try{...}catch{...}去捕捉它并对它进行处理；

2.用try{...}catch{...}捕捉了异常之后一定要对在catch{...}中对其进行处理，那怕是最简单的一句输出语句，或栈输入e.printStackTrace();

3.如果是捕捉IO输入输出流中的异常，一定要在try{...}catch{...}后加finally{...}把输入输出流关闭；

4.如果在函数体内用throw抛出了某种异常，最好要在函数名中加throws抛异常声明，然后交给调用它的上层函数进行处理。

**自定义异常**

用户可以自定义异常，新建一个异常类，让其继承Exception类或Exception的某个子类。然后用throw抛出自己定义的异常类对象。