---
title: 读书笔记-侯捷C++面向对象高级开发(上)
comments: false
categories:
  - 读书
tags:
  - c++
date: 2021-06-15 18:52:33
---
已经好久没写C++了，一直在游戏前端漂浮，而技术虽然可以解决很多问题，但是一直没有沉淀下来，而感觉自己只能在游戏开发浮沉。
最近和一些大佬聊天，原来自己还可以选择游戏引擎开发，而这个貌似也挺符合自己的未来的规划，一是可以继续往技术深度发展，其次就是可以在图形学这些底层更加的深入学习。因此就重新开始学习C++基础，选择了侯捷老师的课程，这里是第一部分，关于面向对象部分的内容。
<!--more-->
说到面向对象，自然而然，就会拿C++和C去对比，两者最大的不同就是C的数据和函数都是全局的，也就是说大家都可以访问，而C++则不同，函数和数据都属于类的内部，外面能不能访问要看访问等级access level，也就是public， protect， private，说到这里就会引入了面向对象的特征类，class。

侯捷老师说，我们要写正规大气的代码，所以在每一个环节都要标准。
C++的文件形式有头文件(.h),标准库文件(.h)和正常的定义文件(.cpp)，我们编写一个类就会从头文件开始。一个好的编码习惯是有防卫式的代码，

    #ifndef __COMPLEX__
    #define __COMPLEX__
    class complex
    {
        private:
          double re, im;//复数的实部和虚部
    };
    #endif
这样就可以避免多个地方定义同一个类，或者同一个文件包含多次头文件，从而引发的报错。
构造函数是类一个比较重要的概念

    class complex
    {
      public:
        complex(double r = 0, double i = 0) : re(r) , im(r){}
    };
当然，这里一个类可能会存在多个构造函数，也就是重载，主要是方便一个类可以通过多个方式进行初始化，而类的后面是一个初始化列表，这样可以加快参数初始化的速度。

    class complex
    {
      public:
        double real() const { return re; }
        double image() const { return im; }
        void real(double r) { re = r; }
        void image(double i) { im = i;}
    };
这里的代码可以看出来，这里是提供了两个方法让外面可以获取里面的实部和虚部，然后后面带上了const，主要是防止外面去修改我们类的内部的参数。这里我也写了两个方法，其实和C#的set和get方法有点像，也凸显了重载的重要性，让同一个名字发挥更大的作用。

正常来讲，构建函数都是共有的，这样主要是方便外面新建的一个对象，但是也是有特殊的

    class complex
    {
      public:
        static complex& getInstance();
        void setup() { ... }
      private:
        complex(double r = 0, double i = 0) : re(r), im(r){}
    };
    complex& complex::getInstance()
    {
      static complex comp;
      return comp;
    }
我们可以通过这种方式实现了单例模式。这里也存在一个特殊的字符，类名后面带上&，这个是C++的一个特殊用法，引用，和指针的用法有点像，但是我们不用像指针那样处理各种繁琐的东西，但是用起来很方便。侯捷老师建议我们尽量用引用，这样就可以加快传递的速度了。但是也不是说随便使用的，像下面的例子：
  
    class complex
    {
      public:
        complex& operator += (const complex& r);
    };

    inline complex& complex::operator += (const complex& r)
    {
      this->re += r.re;
      this->im += r.im;
      return *this;
    }

    inline complex operator + (const complex& lhs, const complex& rhs)
    {
      return complex(lhs.re + rhs.re, lhs.im + rhs.im);
    }
这里用了几个方法描述了引用和非引用的区别,你看下面的返回值就不能用引用，为啥呢，因为里面的变量是临时产生的，在函数结束之后就会触发了对象被回收，自然外面也就没有了，所以只能通过值来传递了。
这里我们的实例是操作符重载，这里也可以说是C++比较重要的内容，你看我们自定义的类，如果可以通过操作符重载，就可以实现值相加，或者判定两个对象是否相等了。
这里还引出了一个关键字"inline"，这个主要是为了加快编译的速度，但是这里只是声明而已，最后能不能作为内联函数，还是要看编译器自己的决定的，里面不能有过于复杂的内容。

我们这里的实例complex里面没有指针，所以是没有问题的，但是看接下来的一个有指针的类String

    class String
    {
      public:
        String(const char* cstr = 0);
        String(const String& r);
        String& operator = (const String& str);
        ~String();
      private:
        char* m_data;
    }
这里引入了C++的三个特殊函数，也叫Big Three，也就是拷贝构造函数，拷贝赋值函数，还有析构函数。这里主要是因为存在指针的话，如果我们没有重写这三个函数，就会单纯的拷贝了m_data，这时候两个不同的对象里面的值是一样的，这样下来就是如果改变一个值，另一个也会被改变，这样就达不到两个对象的目的了。这里也引入一个新的概念，深拷贝和浅拷贝。

写程序的都知道，我们的程序的内存分两部分，一部分是栈的，另一部分就是堆。这两个的区别是，栈是程序自动分配的，存在于某个作用域的，也就是我们一个作用域内的参数，局部变量，还有返回值都会在栈内，而我们通过代码new出来的对象，则是在堆内生成的。

侯捷老师也给我们分析了一个类在内存上的表现，一个类除了自身的数据大小，还会包含头和尾两个cooking，一共是4x2个字节，而内存主要是以16进制来表示的，如果不够十六的倍数，则会往最近的16的倍数靠近。
另外就是我们会生成数组，所以生成的内存，会有4个字节来存储数组的大小，当然，我们也是要按照顺序去销毁的，否则就会造成了内存泄漏。

最后，侯捷老师给我们讲了面向对象的特性：
1. Composition-is a
2. Delegation-指针版本的Composition
3. Inheritance