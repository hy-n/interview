### Interview

- 修改简历先 ✅

- 知乎上的问题看一下答案，自己整理下实际遇到的例子，加以阐述。 

- 发散，解构，然后再重构。 

- 可以刷一点牛客上的笔试题，选择题什么的。 

- leetcode 前100 ，有思路写代码，没思路看思路。图得看啊 van 一问呢。

- kata 热身，可以适当更换一些

  - quicksort ... mergesort, heapsort 

  - list ... 翻转链表，合并链表，链表排序.. 

  - 树的操作


success is not final, failure is not fatal, it is the courage to continue that counts!  

## C 

### const的作用有哪些，谈一谈你对const的理解？

1. 基本数据类型，可以放在类型前后均可，结果一样，使用变量时不能对进行值的修改，只读变量
2. 修饰指针 const char* 修饰的时指针指向的变量 char，指针指向为常量。
3. 修饰指针char* const 修饰的是指针本身，说明指针的值为常量
4. const char* const  指针指向的变量和指针的值均不可修改
5. 多用于修饰函数形式参数，还有声明字符串常量。如果不用const char* 会有undefined behavior 
6. 在函数中修饰参数，保护原对象的属性
7. 在类中的用法
   1.  const 成员变量，在某个对象的生命周期内是常量，对于类来说是可以改变的，在初始化列表中进行初始化
   2. const 成员函数，修饰this 指针 防止成员函数修改对象的内容，不能与static 一起使用，因为static 修饰静态成员函数，没有this 指针。
8. 修饰类对象，定义常量对象
   1. 常量对象只能调用const 成员函数，保护其const 属性
   2. 如果实在想修改某成员，可以用mutable进行修饰
   3. 如果整个类中都恒定的常量，用类中enum 常量 or static const 变量
9. this 指针是个 T *const， const 成员函数的this 指针是 `const T* const`



**描述char\*、const char\*、char\* const、const char\* const的区别？**

如上描述

指针常量和常量指针有什么区别？

### **static的作用是什么，什么情况下用到static？**

1. 修饰变量

   1. 局部变量 生命周期和存储位置发生了变化，作用域没变。被初始化一次 data/bss 
   2. 全局变量 生命周期和存储位置发生了变化，只有本文件可见，无法被其他文件中符号链接到， data/bss 

2. 修饰函数

   1. c的一种封装，因为没有namespace，内部使用的接口，外部文件无法访问和调用，无法链接

3. 修饰类

   1. static 函数 该函数属于一个类而不是属于类中的特定对象

   2. static 变量 所有对象共享，只有一份，可以通过类和对象去访问

   3. static 成员函数为整个类所有，不接受this指针，只能访问类的static 变量

   4. static 成员必须要在类外进行初始化，因为对象创建晚于static 成员初始化。

   5. 静态成员函数没有this 指针，所以加上virtual 也没有实际意义: this->vptr->vtable->virtual function 

      

### 全局变量与局部变量的区别？

作用域和存储位置：

1. 全局变量：全局可见，extern, static 初始化的全局变量在data, 没有初始化的在bss, 默认值为0 
2. 局部变量： 作用域内可见， static 的在全局/数据区 初始化的，没有初始化的。。。 

### **宏定义的作用是什么？**

在预编译阶段进行文本替换即展开，没有类型，也没有类型检查了。在展开的过程中很容易出现边界效应，达不到预期，并且从汇编角度讲会有很多立即数的数据拷贝

### 内存对齐的概念？为什么会有内存对齐？

加快访问速度

可移植性

某些硬件只支持内存对齐





### **inline** **[内联函数](https://www.zhihu.com/search?q=内联函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})的特点有哪些？它的优缺点是什么？**

**inline 是给编译器的建议，编译器可能根本都不鸟**

一个非static 函数如果在多个编译单元中重复定义，链接阶段会报错

**加上inline 会认为它是一个弱符号，编译器来决定使用哪一个**

inline namespace 选择性的暴露给用户特定的版本接口。

inline的使用时有所限制的，inline只适合函数体内部代码简单的函数使用，不能包含复杂的结构控制语句例如while、switch，并且不能内联函数本身不能是直接递归函数（即，自己内部还调用自己的函数）。

因为内联函数要在调用点展开，**所以编译器必须随处可见内联函数的定义，要不然就成了非内联函数的调用了。所以，这要求：声明和定义要一致。**

类内定义的函数默认inline .

以代码膨胀为代价，省去了函数调用的开销，从而提高代码执行效率。



### 如何避免野指针？

不要使用没有初始化的指针。

当指针指向的对象已经消亡，超出了变量的声明周期，那么就不要再使用它。而不是只是赋值一个NULL试图cover 

两种意见，一种是置为NULL ，一种是说这是逻辑有问题，不应该访问，要在ut阶段发现这个问题。 

C++ 中 reference

smart pointer 都是很好的解决方案



### **如何计算结构体长度？**

默认对齐4字节 编译器的默认行为这是。所以要指定

结构体中内置数据类型最大者两者取最小公倍数

padding 

1. 首地址能够被最宽的基本类型成员大小整除
2. 结构体每个成员相对首地址的偏移量都是成员大小的整数倍。不够就padding
3. 结构体总大小为最宽基本类型大小的整数倍，不满足就padding

### sizeof和[strlen](https://www.zhihu.com/search?q=strlen&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})有什么区别？

sizeof是运算符，strlen 是 <string.h> 中提供的函数

sizeof在编译器输出结果并进行替换，结果为类型/变量的大小

strlen 在运行时输出结果，大小为字符串的长度，并不安全容易越界



### **知道条件变量吗？条件变量为什么要和锁配合使用？** Todo

互斥锁只能保证线程之间的互斥，但是不能保证线程之间的执行顺序，条件变量是要控制线程之间的执行顺序，当生产者生产完后消费者去消费。不是消费者一直去循环或者sleep

是在做cond_wait的时候，需要先解锁释放资源，让其他线程有机会操作条件变量，然后用wait阻塞当前线程，待到被唤醒时再重新加锁。但是为了防止在解锁和wait之间条件变量被修改，解锁和wait应该是一个原子操作[3]（[1]说还有加锁，应该是不太对，因为如果加锁和阻塞也是原子的，那么他们三个必须紧密地前后执行，就无法让出cpu，其他线程就无法执行）。为了让解锁和wait是原子的，这里有一个单独的写好的函数pthread_cond_wait(&cond, &mutex); 他会自动完成原子的释放锁和阻塞，以及被唤醒后的自动加锁。

那么为什么不能先wait阻塞再释放锁呢？因为wait之后本线程已经阻塞并等待唤醒了，而它还没有释放锁，cpu还被占着，其他线程还没法执行，就永远也没法唤醒这个线程，就死锁了。

### 如何用C 实现 C++ 的面向对象特性（封装、继承、多态）todo 

### **[memcpy](https://www.zhihu.com/search?q=memcpy&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})怎么实现让它效率更高？**

青春版：

```cpp
//模拟memcpy的实现
void * mymemcpy(void *dest, const void *src, size_t n)
{
    if (dest == NULL || src == NULL)
          return NULL;
    char *pDest = static_cast <char*>(dest);
    const char *pSrc  = static_cast <const char*>(src);
    if (pDest > pSrc && pDest < pSrc+n) // 如果存在覆盖的情况，从后往前
    {
        for (int i=n-1; i >=0; --i)
        {
                pDest[i] = pSrc[i];
        }
    }
    else	// 不存在，从前往后
    {
        for (size_t i= 0; i < n; i++)
        {
                pDest[i] = pSrc[i];
        }
    }

    return dest;
}
```



旗舰版：glibc 

策略： 64字节内小内存方案，64K以内用中尺寸方案，大于64K用大内存拷贝方案

查表： 拷贝不同的小尺寸，直接跳转到相应地址解除循环

目标对齐： 64 字节以上拷贝先用跳转方法让目标地址对齐，

矢量拷贝：cpu特性，并行一次读取N个矢量到sse2 寄存器，再并行输出

缓存：prefetchnta，提前预取数据，等真的用的时候数据已经就位

内存直接写：使用movntdq来直接写内存 【汇编】



### typedef和[define](https://www.zhihu.com/search?q=define&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})有什么区别？

1. 编译阶段： 编译阶段，预处理阶段
2. 类型检查 yes /no
3. 作用域 yes / no
4. typedef 别名， 类型 #define 常量， 变量，编译开关

## C++ 

### 1.构造与析构 



### 2. 重写和覆盖(override)，重载(overload)

### 3.封装，继承，多态

### 4.虚函数，虚函数表。

### 5.Big 5 构造，析构，拷贝构造，拷贝赋值，移动构造，移动赋值。

### 6. C语言和C++有什么区别？

### 7. **struct和class有什么区别？**

访问权限struct 默认是public, class 默认是private 

### 8. extern "C"的作用？

在c++中引用了c的头文件，需要按照c的方式来调用函数，否则会出现undefined reference 链接错误。

### 9.**了解RAII吗？介绍一下？RAII可是C++很重要的一个特性**



### 10. 函数重载和覆盖有什么区别？

重载是指相同作用域下的两个函数名相同，但是函数参数的类型，个数，顺序不同，不考虑返回值，是静态多态。

覆盖是指在基类中定义了一个虚函数，在派生类中也定义了一个具有相同函数原型(函数名，参数列表)的函数，这样派生类中的同名函数会覆盖基类中的同名函数，在运行时根据对象的类型调用相应的虚函数。属于运行时多态

符号隐藏，如果基类中有一组重载函数，子类在继承父类的时候如果覆盖了任意一个，那么其余没有被重写的同名函数在子类中是不可见的。

### 11**谈一谈你对多态的理解，运行时多态的实现原理是什么？**

多态分为静态多态和运行时多态：

静态多态指的是模版和函数重载，即在编译器就能确定的多态。

运行时多态指的是虚函数机制，讲一下对象的布局，覆盖的过程，运行时通过指针来访问，vptr去vtable中找到对应的虚函数，进行调用。

对虚函数机制的理解，[单继承](https://www.zhihu.com/search?q=单继承&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})、多继承、虚继承条件下虚函数表的结构

### 12.**如果虚函数是有效的，那为什么不把所有函数设为虚函数？**

 [C++哲学，如果用不到就不用，运行时的开销，编译时间变长]

### 13.构造函数可以是虚函数吗？[析构函数](https://www.zhihu.com/search?q=析构函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})可以是虚函数吗？

构造函数不能时虚函数，因为要在构造的过程中才将虚函数的地址放到虚函数表中。

在继承关系下析构函数必须是虚函数，否则使用指针时会调用基类的析构函数，会造成资源的泄漏。

### 14.**基类的析构函数可以调用虚函数吗？基类的构造函数可以调用虚函数吗？**

> 当一个构造函数被调用时，它做的首要的事情之一就是初始化它的VPTR。然而，它只能知道它属于“当前”类——即构造函数所在的类。于是它完全不知道这个对象是否是基于其它类。当编译器为这个构造函数产生代码时，它是为这个类的构造函数产生代码——既不是为基类，也不是为它的派生类（因为类不知道谁继承它）。所以它使用的VPTR必须是对于这个类的VTABLE。而且，只要它是最后的构造函数调用，那么在这个对象的生命期内，VPTR将保持被初始化为指向这个VTABLE。但如果接着还有一个更晚派生类的构造函数被调用，那么这个构造函数又将设置VPTR指向它的VTABLE，以此类推，直到最后的构造函数结束。VRTP的状态是由被最后调用的构造函数确定的。这就是为什么构造函数调用是按照从基类到最晚派生类的顺序的另一个理由。
>
> 但是，当这一系列构造函数调用正发生时，每个构造函数都已经设置VPTR指向它自己的VTABLE。如果函数调用使用虚机制，它将只产生通过它自己的VTABLE的调用，而不是最后派生的VTABLE（所有构造函数被调用后才会有最后派生的VTABLE）。另外，许多编译器认识到，如果在构造函数中进行虚函数调用，应该使用早绑定，因为它们知道晚绑定将只对本地函数产生调用。无论哪种情况，在构造函数中调用虚函数都不能得到预期的结果。



[C++构造函数和析构函数调用虚函数时都不会使用动态联编]

构造函数： 基类对象先进行构造，此时派生类的数据成员还没有初始化，此时调用派生类的虚函数是不安全的，c++ 不会进行动态绑定

析构函数： 析构函数完成的是对象的清理动作，在清理时先调用派生类的析构函数，然后再调用基类的析构函数，在调用基类的析构函数时，派生类对象的数据成员已经完成了清理动作，此时再调用派生类的虚函数已经没有意义了。

**１.　从语法上讲，调用完全没有问题。**
**２.　但是从效果上看，往往不能达到需要的目的。**
**Effective 的解释是：**
**派生类对象构造期间进入基类的构造函数时，对象类型变成了基类类型，而不是派生类类型。**
**同样，进入基类析构函数时，对象也是基类类型。**
**所以，虚函数始终仅仅调用基类的虚函数（如果是基类调用虚函数），不能达到多态的效果，所以放在构造函数中是没有意义的，而且往往不能达到本来想要的效果。**

### 15. 什么场景需要用到[纯虚函数](https://www.zhihu.com/search?q=纯虚函数&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})？纯虚函数的作用是什么？

### 16. **指针和引用有什么区别？什么情况下用指针，什么情况下用引用？**

### 17. new和malloc有什么区别？

- **malloc的内存可以用delete释放吗？**

- malloc出来20字节内存，为什么free不需要传入20呢，不会产生内存泄漏吗？

- **new[]和delete[]一定要配对使用吗？为什么？**

### 18. 类的大小怎么计算？

### 19. **volatile关键字的作用**

​	三个特性：

1. 易变性：汇编层面上限制上下文之间不会使用寄存器中的值，而是重新从内存中读取
2. 不可优化：告诉编译器不要对带有这个变量的语句进行优化，甚至将变量直接消除，保证指令一定被执行
3. 顺序性：保证volatile变量之间的顺序性，编译器不会进行乱序优化。

### 20. 如何实现一个线程池？说一下基本思路即可！





### 21.**了解各种强制类型转换的原理及使用吗？说说？**

static_cast 

const_cast 

dynamic_cast 

reinterpret_cast 





### 1. new和 malloc 的区别？ 

new 的实现原理： 如果是简单类型，则直接调用 operator new()，在 operator new() 函数中会调用 malloc() 函数，如果调用 malloc() 失败会调用 _callnewh()，如果 _callnewh() 返回 0 则抛出 bac_alloc 异常，返回非零则继续分配内存。 

如果是复杂类型，先调用 operator new()函数，然后在分配的内存上调用构造函数。 

malloc 的实现原理：有多种启发式策略：空闲链表，bitmap 等... 如果大于128K 直接向操作系统申请堆内存。

new 和 malloc 的区别 - new 是操作符，而 malloc 是 c rtl函数； - 使用 new 操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算，而 malloc 则需要显式地指出所需内存的尺寸。

 new 分配失败的时候会直接抛出异常，malloc 分配失败会返回 NULL； 对于非简单类型，new 在分配内存后，会调用构造函数，而 malloc 不会； 

new 分配成功后会返回对应类型的指针，而 malloc 分配成功后会返回 void * 类型指针  

 malloc 可以分配任意字节，new 只能分配实例所占内存的整数倍数大小； 

 new 可以被重载，而 malloc 不能被重载； 

 new 操作符从自由存储区上分配内存空间，而 malloc 从堆上动态分配内存； 

 使用 malloc 分配的内存后，如果在使用过程中发现内存不足，可以使用 realloc 函数进行内存重新分配实现内存的扩充，new 没有这样直观的配套设施来扩充内存。

(delete 和 free 的区别也在这里)

free 不需要传入关于size 的信息，这是c运行动态库的设计者决定的，这就以为着堆管理器需要自己维护一个关于大小信息和void* 指针的关系，一般来说会在返回给用户的指针前若干个字节中记录关于这块已分配的内存信息，并且还有一些校验信息，以防其中内容被破坏了。

### 2. new delete new[] delete[] 配对使用

new 是首先调用operator new() ，如果是复合类型，则会在分配的T* 调用T 的构造函数。

```c
auto ptr = operator new() T; 
ptr->T(); 
return ptr; 
```

Delete 时如果是复合类型，首先调用 T的析构函数，然后再回收空间

```cpp
ptr -> ~T(); 
operator delete() ptr; 
```



new [] 无非是先申请一个对象的数组空间，然后对每个对象指针都调用一次构造函数，完成对象数组的构造。 

delete [] 首先对每个对象调用一次析构函数，然后再进行数组对象的空间回收。



### 3. 继承和虚函数的一些问题

- 普通类对象是什么布局？
- 带虚函数的类对象是什么布局？
- 单继承下不含有覆盖函数的类对象是什么布局？
- 单继承下含有覆盖函数的类对象是什么布局？
- 多继承下不含有覆盖函数的类对象是什么布局？
- 多继承下含有覆盖函数的类对象的是什么布局？
- 多继承中不同的继承顺序产生的类对象布局相同吗？
- 虚继承的类对象是什么布局？
- 菱形继承下类对象是什么布局？
- 为什么要引入虚继承？
- 为什么虚函数表中有两个析构函数？
- 为什么构造函数不能是虚函数？
- 为什么基类析构函数需要是虚函数？

虚继承和非虚继承对于虚函数表的影响，如果使用普通继承的方式，派生类会与基类共用一个虚函数表。也就是说派生类中新定义的虚函数地址会被放到基类的虚函数表中。而虚继承的话两个表是分开的。 



#### 1. 虚函数的实现原理

虚函数的作用 C++ 中的虚函数的作用主要是实现了动态多态的机制。

动态多态，简单的说就是用父类型的指针指向其子类的实例，然后通过父类的指针调用实际子类的成员函数。这种技术可以让父类的指针有“多种形态”，这是一种泛型技术。 

 虚函数实现原理 编译器处理虚函数时，给每个对象添加一个隐藏的成员。隐藏的成员是一个指针类型的数据，指向的是函数地址数组，这个数组被称为虚函数表（virtual function table，vtbl）。虚函数表中存储的是类中的虚函数的地址。如果派生类重写了基类中的虚函数，则派生类对象的虚函数表中保存的是派生类的虚函数地址，如果派生类没有重写基类中的虚函数，则派生类对象的虚函数表中保存的是父类的虚函数地址。 

加分回答 使用虚函数时，对于内存和执行速度方面会有一定的成本： 1. 每个对象都会变大，变大的量为存储虚函数表指针； 2. 对于每个类，编译器都会创建一个虚函数表； 3. 对于每次调用虚函数，都需要额外执行一个操作，就是到表中查找虚函数地址。



虚函数表的构成：

1. 偏移量
2. RTTI 信息 运行时类型识别信息
3. 虚函数的地址，虚函数定义在代码段。

#### 2. 静态绑定与动态绑定

```cpp
    // 只有虚函数的重写属于动态绑定，其行为取决于动态的类型
    // 其他如重写虚函数的默认参数，虚函数的重载，非虚函数的重写和覆盖都是静态绑定，只会调用静态类型的行为
    pb2->myfunc();
    pb1->myfunc();

    // pb1, pb2 的静态类型是 Base* 而name属于是虚函数的重载，所以是静态绑定，只会调用B* 的方法，不管动态类型是什么
    pb1->name(2); 
    pb2->name(3.0); 


    //  sex 属于虚函数的重写，所以是动态绑定，pb1 调用类型B* 的方法，pb2 调用类型D* 的方法
    pb1->sex(); 
    pb2->sex(); 
```



#### 3. 查看对象的内存布局

```cpp
struct Base {
    Base() = default;
    ~Base() = default;

    void Func() {}
    virtual void vfunc() {}
    int a;
    int b;
};

struct Derived :public Base  
{

    virtual void vfunc() { ; }
    int c; 

};
```

查看对象的内存布局

```shell
clang -Xclang -fdump-record-layouts -stdlib=libc++ -c layout.cpp
```

查看对象虚函数表的内存布局

```shell
clang -Xclang -fdump-vtable-layouts -stdlib=libc++ -c layout.cpp
```

在另一个markdown中。

继承的3种方式: public, private, protected 

static 在类中的使用

动态绑定的原理: 派生类继承了多个基类后对象的布局。 



## C++ 11 

#### 强化for循环表达式

```cpp
for(auto & a: b)
{
    // 如果b不是常量对象 a 是 T&类型 ，修改了a b中的元素也被修改了
   	// 如果b 是常对象 那么a 是 const T& 类型，不可对a进行修改。 
}

for(auto a : b)
{
    // a 是拷贝出来的，修改a 不会影响b，对于复杂的对象不推荐使用。因为会多一次拷贝操作，不如用const auto& 
}

for(const auto & a: b)
{
    // a 是常量引用，不可以对a进行修改，自然不会影响b c++建议能加const 就用const 
}
```

### Rvalue & lvalue  

### 常用的一些模版特性：

1. std::move 
2. std::forward 
3. std::function 
4. std::bind 

### 智能指针三幻神

1. unique_ptr; 
2. shared_ptr; 
3. weak_ptr; 
4. auto_ptr(已废弃)



### 1. 智能指针及线程安全

智能指针实现原理 建立所有权（ownership）概念，对于特定的对象，只能有一个智能指针可拥有它，这样只有拥有对象的智能指针的析构函数会删除该对象。然后，让赋值操作转让所有权。这就是用于 auto_ptr 和 unique_ptr 的策略，但 unique_ptr 的策略更严格，unique_ptr 能够在编译期识别错误。

 跟踪引用特定对象的智能指针计数，这称为引用计数（reference counting）。例如，赋值时，计数将加 1，而指针过期时，计数将减 

1. 仅当最后一个指针过期时，才调用 delete。这是 shared_ptr 采用的策略。
2.  使用场景 如果程序要使用多个指向同一个对象的指针，应该选择 shared_ptr； 如果程序不需要多个指向同一个对象的指针，则可以使用 unique_ptr; 如果使用 new [] 分配内存，应该选择 unique_ptr; 如果函数使用 new 分配内存，并返回指向该内存的指针，将其返回类型声明为 unique_ptr 是不错的选择。 因为unique_ptr 可以很方便的转化为shared_ptr 
3.  线程安全 shared_ptr 智能指针的引用计数在手段上使用了 atomic 原子操作，只要 shared_ptr 在拷贝或赋值时增加引用，析构时减少引用就可以了。首先原子是线程安全的，所有 shared_ptr 智能指针在多线程下引用计数也是安全的，也就是说 shared_ptr 智能指针在多线程下传递使用时引用计数是不会有线程安全问题的。 但是指向对象的指针不是线程安全的，使用 shared_ptr 智能指针访问资源不是线程安全的，需要手动加锁解锁。 4. 智能指针的拷贝也不是线程安全的。
4. .reset() 是线程安全的吗？ 要不要加锁？
5. unique_ptr 的对象大小是多少？如果指定了delete function 之后呢？
6. shared_ptr 的对象大小是多少？控制字段是new 出来的。







### 了解auto和[decltype](https://www.zhihu.com/search?q=decltype&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})吗？

### **谈一谈你对左值和右值的了解，了解左值引用和右值引用吗？**

### 了解移动语义和完美转发吗？ Effective cpp

移动不是真正的移动，完美转发也不完美。

### **[enum](https://www.zhihu.com/search?q=enum&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})** **和 enum class有什么区别？** 

【作用域: 一个是全局的，一个是不是】

### 了解列表初始化吗？

c++11 

### **对C++11的智能指针了解多少，可以自己实现一个智能指针吗？**





### 平时会用到function、bind、lambda吗，都什么场景下会用到？





### 对C++11的mutex和RAII lock有过了解吗？





### **一般什么情况下会出现内存泄漏？出现内存泄漏如何调试？**





### unique_ptr如何转换的所有权？

移动构造，移动赋值





### **谈一谈你对面向对象的理解**





### 什么场景下使用继承方式，什么场景下使用组合？



### C++ 线程库 

#### std::mutex (RAII)





## STL 

### C++直接使用数组好还是使用std::array好？std::array是怎么实现的？

vector 和 array 都是stl 中的容器，都重载了下标运算符，在内存方面都使用了连续内存，底层都是连续的数组。



array定义时需要指定模版参数为常量表达式，vector 可以是变量表达式

vector 可以进行动态扩容，array 和 数组容量固定。

array 对象和数组存储在栈中，vector 对象在堆上



array 在初始化和销毁的时候会自动调用每个对象的构造/析构函数! 封装了底层的数组，提供了更加安全的访问方式以及其他接口。



### **std::vector最大的特点是什么？**

特点是容量可变，随机访问，访问时间O(1) , 插入删除时间o(n).

#### 它的内部是怎么实现的？

维护一个T* 指向堆上的内存，可以进行动态扩容。并提供相应的接口 

以及随机访问迭代器。重载了下标运算符，支持和数组一样的下标访问。

#### resize **reserve的区别是什么？**

resize 方法 会在容器end 添加或者删除一些元素，使得容器达到指定的大小，使用resize 可能会导致capacity 改变，因为可能有扩容在。

reserve 只是改变了capacity 的值，如果是新增加的空间，里面的元素可能还没有被初始化，这样如果使用at 进行访问会std::out_of_range 因为stl 认为只有在0 ~ vec.size() 内的元素是合法的。 

#### **clear是怎么实现的？**

```cpp
void clear() 
{
    erase(begin(), end()); 
}

template <class T, class Alloc>
typename vector<T, Alloc>::iterator
	vector<T, Alloc>::erase(iterator first, iterator last) {
	auto i = mystl::copy(last, finish_, first);
	mystl::destroy(i, finish_);	
	finish_ = finish_ - (last - first);	
	return first;
}

```



```cpp
// dtor 
~vector() 
{
    _destroy_and_deallocate(); //
    
    erase(begin(), end()); 		// 先进行元素的析构动作。
    allocator::deallocate();	// 再空间的回收 
}

template <class T, class Alloc>
void vector<T, Alloc>::__destroy_and_deallocate() {
	mystl::destroy(start_, finish_);
	data_allocator::deallocate(start_, end_of_storage_ - start_);
}

```

#### push_back 和 emplace_back 的区别

emplace_back 

> Appends a new element to the end of the container. The element is constructed through [std::allocator_traits::construct](https://en.cppreference.com/w/cpp/memory/allocator_traits/construct), which typically uses placement-new to construct the element in-place at the location provided by the container. The arguments `args...` are forwarded to the constructor as [std::forward](http://en.cppreference.com/w/cpp/utility/forward)<Args>(args)....
>
> If the new [size()](https://en.cppreference.com/w/cpp/container/vector/size) is greater than [capacity()](https://en.cppreference.com/w/cpp/container/vector/capacity) then all iterators and references (including the past-the-end iterator) are invalidated. Otherwise only the past-the-end iterator is invalidated.

在容器提供的一块内存地址上【末尾】原地构造对象，使用placement-new来原地进行对象的构造。参数是对象构造函数的参数

如果size 大于了capacity, 所有迭代器和引用都将失效，否则只有指向最后的迭代器和引用会失效 【end(), back()】 

> Appends the given element `value` to the end of the container.
>
> \1) The new element is initialized as a copy of `value`.
>
> \2) `value` is moved into the new element.
>
> If the new [size()](https://en.cppreference.com/w/cpp/container/vector/size) is greater than [capacity()](https://en.cppreference.com/w/cpp/container/vector/capacity) then all iterators and references (including the past-the-end iterator) are invalidated. Otherwise only the past-the-end iterator is invalidated.

push_back 是将对象添加到末尾， 参数是对象，在这个过程中调用对象的拷贝构造函数或者移动构造函数。

如果size 大于了capacity, 所有迭代器和引用都将失效，否则只有指向最后的迭代器和引用会失效 【end(), back()】 

#### front() 和 begin() 的区别

front 返回第一个元素的引用，对空的容器调用front 是 ub 

 begin() 返回的是迭代器，指向第一个元素

#### 扩容的策略

两倍扩容，load factor 均摊分析。



### deque的底层[数据结构](https://www.zhihu.com/search?q=数据结构&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})是什么？它的内部是怎么实现的？

双端队列底层是维护一个T** 指针，指向多个`T*`  即数组，在向两端扩张同时维护这种表面看起来的连续性。

 



### **map和unordered_map有什么区别？分别在什么场景下使用？**





### list的使用场景？std::find可以传入list对应的迭代器吗？





### **string的常用函数**





### 迭代器的类型都有哪些







## 操作系统

进程与线程

进程间通信方式

线程共享资源的处理：同步互斥 

**进程的调度方式**

共享内存的处理

进程的虚拟地址空间，程序的局部性，内存分页。 《程序员的自我修养》 



- 进程和线程的区别？
- **操作系统是怎么进行进程管理的？**
- 操作系统是如何做到进程阻塞的？
- **进程之间的通信方式有哪些？**
- 线程是如何实现的？
- **线程之间私有和共享的资源有哪些？**
- 一般应用程序内存空间的堆和栈的区别是什么？
- **进程虚拟空间是怎么布局的？**
- 虚拟内存是如何映射到物理内存的？了解分页内存管理吗？
- **什么是上下文切换，操作系统是怎么做的上下文切换？**
- 什么是大端字节，什么是小端字节？如何转换字节序？
- **产生死锁的必要条件有哪些？如何避免死锁？**
- 信号和[信号量](https://www.zhihu.com/search?q=信号量&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})的区别是什么？
- **锁的性能开销，锁的实现原理？**

**编译原理，编译和链接的知识还是很重要的，解决编译和链接过程中的报错也是C++程序员的基本能力。**

- gcc hello.c 这行命令具体的执行过程，内部究竟做了什么？
- **程序一定会从main函数开始运行吗？**
- 如何确定某个函数有被编译输出？
- **动态链接库和[静态链接库](https://www.zhihu.com/search?q=静态链接库&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A1868370927})的区别是什么？**



## 算法

leetcode 前100， 有思路就刷，没思路看答案。

dp 熟悉几个套路

Top K 问题，可以用partition, 可以用heapsort 原地建立堆算法。



## git 操作





## cmake 的一些问题

如何生成动态库

调试 messages( ${var});





## 辣鸡牛客

刷刷选择题，看看容易出错的demo





## 生活节奏

1. 肉蛋奶多吃，还有水果 ✅
2. 早起一小时，比熬夜好很多，保证睡眠。  ✅
3. 注意休息。不要上班太累。 ✅
4. 早点洗澡，洗完了再干活。 
5. 前期不要一周面很多家，2-3为适中，后期可以加快节奏。

ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿ ኈ ቼ ዽ ጿጿ ኈ
