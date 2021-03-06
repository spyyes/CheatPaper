### 为什么构造函数不能是虚函数，而析构函数建议写成虚函数？

**构造函数不能是虚函数：**

1. 从存储空间角度分析

   虚函数是通过虚函数表实现的。虚函数表存储在对象的内存空间。如果构造函数为虚，那么就需要用虚函数表来调用。但是此时对象还没有实例化，无法找到虚函数表。

2. 从使用角度分析

   虚函数的作用在于通过父类的指针或者引用来调用它的时候能够变成调用子类的那个成员函数。而构造函数是在创建对象时自动调用的，不可能通过父类的指针或者引用去调用，因此也就规定构造函数不能是虚函数。同时创建一个对象总是要明确指定对象的类型。

**析构函数往往写成虚函数**

​		用基类类型指针绑定派生类实例，析构的时候，如果基类析构函数不是虚函数，则只会析构基类，不会析构派生类对象，从而造成内存泄漏。因此，在有动态分配堆上内存的时候，析构函数必须是虚函数，但是没有必要是纯虚函数。

### 为什么C++ 默认的构造函数不是虚函数？

C++默认的***析构函数不是虚函数是因为虚函数需要额外的虚函数表和虚表指针，占用额外的内存***。而对于不会被继承的类来说，其析构函数如果是虚函数，就会浪费内存。因此C++默认的析构函数不是虚函数，而是只有当需要当作父类时，设置为虚函数。

**只能有一个析构函数，不能重载。**

如果一个类中有指针，且在使用的过程中动态的申请了内存，那么最好显示构造析构函数在销毁类之前，释放掉申请的内存空间，避免内存泄漏。

类析构顺序：1）派生类本身的析构函数；2）对象成员析构函数；3）基类析构函数。

静态函数和虚函数的区别：
静态函数在编译的时候就已经确定运行时机，虚函数在运行的时候动态绑定。虚函数因为用了虚函数表机制，调用的时候会增加一次内存开销。

### 虚函数表的原理

1. C++编译器保证虚函数表的指针存在于对象实例的最前面。因此，通过对象实例的地址，可以寻址到虚函数表，然后可以遍历其中的函数指针，调用相应的函数。
2. 在继承时，虚函数按照声明顺序放在表中；父类的虚函数在子类的虚函数前面。如果子类有虚函数重载了父类的虚函数，那么覆盖的函数将放在虚函数表中原来的父类虚函数的位置。
3. 如果是多重继承，每个父类都有自己的虚函数表。子类的成员函数被放在第一个父类的表中。这是为了解决不同的父类类型的指针指向同一个子类实例，而能够调用到实际的函数。如果子类覆盖了父类函数，那么会在多个虚函数表中都覆盖。

### 简述几个定义

**虚函数：**

虚函数的作用是允许在派生类中重新定义与基类同名的函数，并且可以通过基类的指针或引用来访问基类或者派生类的同名函数。

**重载：**

名字相同，但参数个数或类型不同；命名变得简单。编译器决定应该调用哪个函数

```c++
int MAX(double f1, double f2){}
int MAX(int n1, int n2){}
int MAX(int n1, int n2, int n3){}
```

**缺省参数：**

缺省参数：最右边连续若干个参数有缺省值;优势在于提高程序可扩充性。

如果某个写好的函数要添加新的参数，原来那些调用函数的语句，未必需要用新的参数，那么可以避免对原来那些函数调用语句的修改。

**面向对象的程序设计的四个特点：**

抽象、封装、继承、多态

### 虚函数和纯虚函数的区别

- 定义一个函数为虚函数，不代表它没有被实现；定义它为虚函数是为了允许用基类的指针来调用子类的整个函数。
- 定义一个函数为纯虚函数，代表这个函数没有被实现。是为了实现一个接口，起到规范的作用，规范继承这个类的程序员必须实现这个函数。含有纯虚函数的类称为抽象类，不能生成对象。派生类中必须重新声明函数。

### 复制构造函数起作用的三种情况：

1. 用一个对象初始化另一个对象

   ```c++
   Complex c2 = c1; //注意这是初始化，不是赋值。是c2的定义
   ```

2. 某函数的参数之一是A的对象，在函数调用时，类A的复制构造函数被调用

3. 如果返回值是类A的对象，那么函数返回时，A的复制构造函数被调用

### static关键字的作用

首先static的最主要功能是隐藏，其次因为static变量存放在静态存储区，所以它具备持久性和默认值0.

- **隐藏**（static函数，static变量均可）
  
- 所有未加static前缀的全局变量和函数都具有全局可见性，其它的源文件也能访问。如果加了static，就会对其它源文件隐藏。利用这一特性可以在不同的文件中定义同名函数和同名变量，而不必担心**命名冲突**。
  
- **保持变量内容的持久**（static变量中的记忆功能和全局生存期）
  - 存储在静态数据区的变量会在程序刚开始运行时就完成初始化，也是唯一的一次初始化。
  - 如果作为static局部变量在函数内定义，它的生存期为整个源程序，但是其作用域仍与自动变量相同，只能在定义该变量的函数内使用该变量。退出该函数后， 尽管该变量还继续存在，但不能使用它。

- **设置默认初始化值为0**

  - 方便初始化，比如稀疏矩阵和字符串。全局变量也具备这一属性，因为全局变量也存储在静态数据区。

- **C++中的类成员声明static**

  - 类的静态成员函数是属于整个类而非类的对象，所以它没有this指针，这就导致 了它仅能访问类的静态数据和静态成员函数。 

  - static并没有增加程序的时空开销，相反她还缩短了子类对父类静态成员的访问 时间，节省了子类的内存空间。      

  - 不能将静态成员函数定义为虚函数。   

  - 由于静态成员声明于类中，操作于其外，所以对其取地址操作，就多少有些特殊 ，变量地址是指向其数据类型的指针 ，函数地址类型是一个“nonmember函数指针”。

  - static数据成员的初始化：

    (1) 初始化在类体外进行，而前面不加static，以免与一般静态变量或对象相混淆。

    (2) 初始化时不加该成员的访问权限控制符private，public等。

    (3) 初始化时使用作用域运算符来标明它所属类，因此，静态数据成员是类的成员，而不

    是对象的成员。

    (4) 静态数据成员是静态存储的，它是静态生存期，必须对它进行初始化。

### C++和C的区别

- C语言是面向过程的，而C＋＋是偏向面向对象的。
- 【动态内存管理】C是使用malloc/free函数，而C++除此之外还有new/delete关键字；（关于malooc/free与new/delete的不同又可以说一大堆，最后的扩展_1部分列出十大区别）；
- 【类和结构体】C中没有class；C中的struct可以在C++中正常使用的，并且C++对struct进行了进一步的扩展，使struct在C++中可以和class一样当做类使用，而唯一和class不同的地方在于struct的成员默认访问修饰符是public,而class默认的是private。
- 【重载】C++支持函数重载，而C不支持函数重载，而C++支持重载的依仗就在于C++的名字修饰与C不同，例如在C++中函数int fun(int ,int)经过名字修饰之后变为 _fun_int_int ,而C是 _fun。
- 【引用】C++中有引用，而C没有。
- 【const定义数组】C 中用const修饰的变量不可以用在定义数组时的大小，但是C++用const修饰的变量可以（如果不进行&,解引用的操作的话，是存放在符号表的，不开辟内存）；
- 【函数库】C语言有标准的函数库，它们松散的，只是把功能相同的函数放在一个头文件中；而C++对于大多数的函数都是有集成的很紧密，特别是C语言中没有的。C++中的API是对Window系统的大多数API有机的组合，是一个集体。但你也可能单独调用API。

### malloc/free 与 new/delete的区别

- **【操作对象有所不同】**

  - malloc/free是C++/C 语言的**标准库函数**，需要头文件支持；new/delete 是C++的**运算符**，需要编译器支持。
  - 对于用户自定义的对象而言，光用malloc/free 无法满足动态对象的要求。对象在创建的同时要自动执行构造函数， 对象消亡之前要自动执行析构函数。由于malloc/free 是库函数而不是运算符，不在编译器控制权限之内，不能够把执行构造函数和析构函数的任务强加malloc/free。
  -  new会先调用operator new函数，申请足够的内存（通常底层使用malloc实现）。然后调用类型的构造函数，初始化成员变量，最后返回自定义类型指针。delete先调用析构函数，然后调用operator delete函数释放内存（通常底层使用free实现）。

- **【用法不同】**

  - ```c++
    void * malloc(size_t size); void free( void * p );
    ```

  - 【返回值】返回值的类型是void *，所以在调用malloc 时要显式地进行类型转换；

    - 为什么free 函数不象malloc 函数那样复杂呢？这是因为指针p 的类型以及它所指的内存的容量事先都是知道的，语句free(p)能正确地释放内存。如果p 是NULL 指针，那么free对p 无论操作多少次都不会出问题。如果p 不是NULL 指针，那么free 对p连续操作两次就会导致程序运行错误。

  - 【参数】malloc 函数本身并不识别要申请的内存是什么类型，它只关心内存的总字节数。因此malloc对开辟的空间大小需要严格指定，而new只需要对象名。

    - 这是因为new 内置了sizeof、类型转换和类型安全检查功能。对于非内部数据类型的对象而言，new 在创建动态对象的同时完成了初始化工作。如果对象有多个构造函数，那么new 的语句也可以有多种形式。**注意如果用new 创建对象数组，那么只能使用对象的无参数构造函数。**

  - 【关于数组】malloc开辟的空间即可以给单个对象用也可以给数组用，释放的方式都是free()；而new开辟对象数组用的是new[size] ,释放的的时候是 delete[]。

  - 【分配失败】new内存分配失败时，会抛出bac_alloc异常。malloc分配内存失败时返回NULL。

- **【内存区域】**

  - new操作符从**自由存储区（free store**）上为对象动态分配内存空间，而malloc函数从**堆**上动态分配内存。
  - 自由存储区是C++基于new操作符的一个抽象概念，凡是通过new操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C语言使用malloc从堆上分配内存，使用free释放已分配的对应内存。自由存储区不等于堆，还可以是静态存储区。

- **既然new/delete的功能完全覆盖了malloc/free，为什么C++还保留malloc/free呢？**

  - 因为C++程序经常要调用C函数，而C程序只能用malloc/free管理动态内存。如果用free释放“new创建的动态对象”，那么该对象因无法执行析构函数而可能导致程序出错。如果用delete释放“malloc申请的动态内存”，理论上讲程序不会出错，但是该程序的可读性很差。所以new/delete、malloc/free必须配对使用。

### 引用和指针的区别

- 指针有自己的一块空间，而引用只是一个别名； 
- 使用sizeof看一个指针的大小是4，而引用则是被引用对象的大小； 
- 指针可以被初始化为NULL，而引用必须被初始化且必须是一个已有对象的引用； 
- 作为参数传递时，指针需要被解引用才可以对对象进行操作，而直接对引用的修改都会改变引用所指向的对象； 
- 可以有const指针，但是没有const引用。？
- 指针在使用中可以指向其它对象，但是引用只能是一个对象的引用，不能 
  被改变； 
- 指针可以有多级指针（**p），而引用只有一级； 
- 指针和引用使用++运算符的意义不一样；

### 重载的原理

### 三种内存类型

- 堆：由程序员自己分配释放（用malloc和free,或new和delete），如果我们不手动释放，那就要到程序结束才释放。如果对分配的空间在不用的时候不释放而一味的分配，那么可能会引起内存泄漏，其**容量取决于虚拟内存**，较大。**由低地址向高地址扩展。空间不连续**
- 栈：由**编译器**自动分配释放，其中存放在主调函数中被调函数的下一句代码、函数参数和局部变量，容量有限，较小。**由高地址向低地址扩展。空间连续**

- 静态存储区：**由编译器分配，由系统释放**，其中存放的是全局变量、static变量和常量。

**区别点：**

- 堆是由低地址向高地址扩展，栈是由高地址向低地址扩展。
- 堆是不连续的空间，栈是连续的空间。

- 在申请空间后，栈的分配要比堆的快。对于堆，先遍历存放空闲存储地址的链表、修改链表、再进行分配；对于栈，只要剩下的可用空间足够，就可分配到，如果不够，那么就会报告栈溢出。
- 栈的生命期最短，到函数调用结束时；静态存储区的生命期最长，到程序结束时；堆中的生命期是到被我们手动释放时（如果整个过程中都不手动释放，那就到程序结束时）。

### C++中四种cast类型强制转换

```c++
cast-name<type-id>(expression);
```

- static_cast 
  - 和C语言的强制转换实现的功能几乎一样。
  - **没有运行时类型检查**来保证转换的安全性。
  - 一般用于：父类和派生类的指针或应用的转换；基本数据类型的转换；空指针转换成目标类型的空指针；把任何类型的表达式换成void型。
  - 不能转换掉const, volatile, __unaligned属性。
- const_cast
  - 用来修改const或volatile属性。转换掉const属性和volatile属性。
- dynamic_cast
  - 支持运行时识别指针或引用所指的对象。
  - dynamic_cast主要用于类层次间的上行转换和下行转换，还可以用于类之间的交叉转换。
  - 在类层次间进行上行转换时，dynamic_cast和static_cast的效果是一样的；在进行下行转换时，dynamic_cast具有类型检查的功能，比static_cast更安全。
  - 在进行下行转换的时候，在运行时如果是安全的，会返回对应的指针；否则返回空指针。
- reinterpret_cast
  - 很少使用。
  - 该运算符把expression重新解释成type-id类型的对象。对象在这里的范围包括变量以及实现类的对象。
  - 乍看像C语言的强制转换。其实不然，C语言的会将一些数值之类的截断等处理，比如浮点型转整形。浮点型跟整形的保存数据处理方式是不同的，但经过处理之后就变成了截断的数值。而此时如果用reinterpret_cast来转换，得到的数值肯定是让你诧异的值，因为其实直接将那二进制的值重新当做另外一种数据类型来解释的。

### C++中的smart pointer四个智能指针及实现原理

可以方便管理对象的生命周期，防止忘记delete；另外也可以保证异常安全。

一个对象什么时候和什么条件需要被析构和删除是由智能指针本身决定的，用户并不需要管理。

- auto_ptr
  - 访问auto_ptr的成员函数用的是'.'，访问指向对象的成员函数时用的是'->'
  - 使用智能指针进行赋值时，如ptest2 = ptest，ptest2会接管ptest原来的内存管理权，ptest会变成空指针。如果ptest2不为空，就会释放原来的资源。因此应该避免把auto_ptr放在容器中，因为算法对容器操作时，很难避免STL内部对容器实现了赋值传递操作，这样会使容器很多元素被置成NULL。
  - 判断一个智能指针是否为空要调用ptest.get() == NULL
  - 如果调用release成员函数，只是把智能指针的赋值设置为空，但是原来指向的内存并没有释放，析构函数并没有调用。如果想中途释放资源，而不是等到智能指针被析构时释放，可以用ptest.reset()

- unique_ptr
  - 所指的对象在作用域之外会自动析构。
  - 不能尝试复制它的内容到另一个scoped_ptr中，防止错误的多次析构同一个指针所指的对象。
- shared_ptr
  - 引用计数。支持复制，复制的本质是引用次数+1。在引用数为0时，自动析构。
- weak_ptr
  - 为了解决shared_ptr出现引用环的问题，
- intrusive_ptr
  - 要求所指向的对象本身实现一个引用计数机制。

### 函数指针

### 静态函数和虚函数的区别

静态函数在编译的时候就已经确定运行时机，虚函数在运行的时候动态绑定。虚函数因为用了虚函数表机制，调用的时候会增加一次内存开销

### **重载和覆盖**

### **strcpy和strlen**

### **++i和i++的实现**

我记得是有程设中有原理介绍，是运算符重载的事情。

### **C++函数栈空间的最大值**

