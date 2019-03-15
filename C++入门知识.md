# C++入门知识



## 一、关键字

关于什么是关键字，就不在解释了。学习一门语言，认识关键字以及了解使用关键字的作用是必须的。

**C++98/03 关键字共63个**.

| asm        | do           | if               | return      | typedef  |
| ---------- | ------------ | ---------------- | ----------- | -------- |
| auto       | double       | inline           | short       | typeid   |
| bool       | dynamic_cast | int              | signed      | typename |
| break      | else         | long             | sizeof      | union    |
| case       | enum         | mutable          | static      | unsigned |
| catch      | explicit     | namespace        | static_cast | using    |
| char       | export       | new              | struct      | virtual  |
| class      | extern       | operator         | switch      | void     |
| const      | false        | private          | template    | volatile |
| const_cast | float        | protected        | this        | wchar_t  |
| continue   | for          | public           | throw       | while    |
| default    | friend       | register         | true        |          |
| delete     | goto         | reinterpret_cast | try         |          |

**C++11 关键字共73个。**

新增关键字：`alignas`、`alignof`、`char16_t`、`char32_t`、`constexpr`、`decltype`、`noexcept`、`nullptr`、`static_assert`、`thread_local`。 

`auto` 的意义改变。 

`register` 被视为过时的（可能在未来标准移除）。 

`export` 因为实现支持太少（仅Edison Design Group的前端支持），编译效率低下，取消原有意义（仍是关键字，但使用它的程序是错误的），改为保留给未来标准使用。

如果想要详细了解[C++关键字](https://baike.baidu.com/item/C++%E5%85%B3%E9%94%AE%E5%AD%97/5773813)，可以百度百科。



## 二、命名空间

在C/C++中，变量、函数和类都是大量存在的，这些变量、函数和类的名称都将存在与全局作用域中，可能会导致很多冲突。使用命名空间的目的是对标识符的名称进行本地化，以避免命名冲突或名字污染。

定义命名空间，需要使用`namespace`关键字。具体用法为`namespace + 命名空间名称{}`即可，{}中为命名空间的成员。

我们先定义一个普通的命名空间，

~~~c++
// namespace:关键字; N1:命名空间名称; a和Add为空间内的成员;
namespace N1{
    int a;
    int Add(int left, int right){
        return left+right;
    }
}
~~~

命名空间可以定义在几个不同的部分中，因此命名空间是由几个单独定义的部分组成的。一个命名空间的各个组成部分可以分散在多个文件中。所以，如果命名空间中的某个组成部分需要请求定义在另一个文件中的名称，则仍然需要声明该名称。

当然命名空间也可以嵌套，可以在一个命名空间中定义另一个命名空间；

~~~c++
namespace N2{
    int a;
    int b;
    int Add(int left, int right){
        return left+right;
    }
    namespace N3{
        int c;
        int d;
        int Sub(int left,int right){
            return left-right;
        }
    }
}
~~~

另外，同一个工程允许存在多个相同名称的命名空间，但是编译器最后会合成为同一个命名空间。

此时，问题便来了。我们定义那么多相同名称的变量和函数，那应该如何使用呢？

命名空间的使用有三种方式：

* 加命名空间名称以及作用域限定符

~~~c++
int main(){
    printf("%d",N1::a);  //命名空间::变量名;  ::为作用域符
    return 0;
}
~~~

我们在编写程序的时候，推荐使用这种方式。但是，这种方式有自己的缺陷，就是当我们需要大量使用同一个命名空间的变量或者函数时，每次都需要使用`::`，此时就比较繁琐。

* 使用using将命名空间中的成员引入

~~~c++
using N1::a;
int main(){
	printf("%d",a);
    return 0;
}
~~~

此时，每个using声明引入命名空间中的一个成员，因此每个名字都需要独立的using声明。

* 使用`using namespace`

~~~C++
using namespace N1;  //当命名空间太长时，可以给其设置一个别名；
int main(){
	printf("%d",a);
	return 0;
}
~~~

使用此种方式虽然方便，但却使得整个命名空间的成员都可见。



## 三、C++的输入与输出

C语言的输出使用的是`printf`输入使用`scanf`，但是在C++中我们引入了新的输入&输出：`cout`、`cin`。

请先看代码展示：

~~~c++
#include <iostream>
// using namespace std;
int main(){
    int money;
    std::cout<<"hello, world!"<<std::endl;  // 输出：hello, world!
    
    std::cout<<"how much money do you have?"
             <<" please input:"<<std::endl;
    std::cin>>money;     // 输入数字
    std::cout<<"money = "<<money<<std::endl;
    
    return 0;
}
~~~

该代码包含下述元素：

* 注释，由`//`标识。
* 预处理器编译指令`#include`。
* 函数头：`int main()`。
* 编译指令 `using namespace`。
* 函数体，用`{}`括起。
* 使用C++的`cout`输出；使用`cin`输入。
* 控制符`endl`
* 结束`main()`函数的`return`语句。

关于注释、函数头、函数体、以及`return`语句就不多说了，主要看下其他几个。

（1）`#include`为预处理编译指令，`#include <iostream>`意味预处理器将`iostream`文件的内容添加到程序中。`iostream`中包含输入输出`cin`、`cout`，若要使用输入输出该头文件不可缺少。在C++中也可以使用C语言中的输入输出或者其他函数，此时通常将头文件前加c。例如`#include <cstdio>`，此时据不需要加`.h`。

（2）`using`编译指令现在接受就好，不详细说明。C++标准程序库中的所有标识符都被定义于一个名为`std`的`namespace`中，倘若我们要使用`cin`、`cout`，如果没有此条指令是无法使用的。当然还有其他使用命名空间成员的方法我们在“二”中已经说明。

（3）`cout`是C++的输出语句。`cout`的对象属性包括`<<`，我们可以这样的理解：它将一串信息插入到输出流输出。因为是插入信息所以信息流动方向向左指向我们的`cout`。在使用`cout`时，`#include<iostream>`以及`using namespace std;`不可缺少。

（4）`cin`是输入语句。它的对象属性为`>>`，信息从`cin`流向money。注意的地方与`cout`相同。

（5）`endl`与C中的`\n`一样，表示换行。但是`endl`在执行时，换行的同时还会调用`flush`清槽，把缓冲槽里的内容输出到控制台。我们要习惯于应用`endl`。`endl`与`cout`、`cin`一样在`iostream`中定义，且位于命名空间`std`中。



## 四、缺省函数

所谓缺省参数，顾名思义，就是在声明函数的某个参数的时候为之指定一个默认值，在调用该函数的时候如果没有指定实参，则采用默认值，否则就使用指定的参数。

~~~C++
void func(int a = 100){
    std::cout<<a<<std::endl;
}
int main(){
    func(); //未传参使用参数的默认值。
    func(10); // 传参，使用指定的实参。
	return 0;
}
~~~

缺省参数可以分为全缺省参数和半缺省参数；

~~~c++
//全缺省参数
void func1(int a = 100, b = 200, c = 300){
    std::cout<<"a = "<<a<<std::endl;
             <<"b = "<<b<<std::endl;
             <<"c = "<<c<<std::endl;
}
~~~

~~~C++ 
//半缺省参数
void func2(int a, int b = 20, c = 30){
    std::cout<<"a = "<<a<<std::endl;
             <<"b = "<<b<<std::endl;
             <<"c = "<<c<<std::endl;
}
~~~

注意:（1）半缺省参数必须从右往左依次给出，不能间隔着给。

​         （2）缺省参数不能再函数声明和定义中同时出现。

~~~c++
//a.h
void testfunc(int a = 10);
//a.cpp
void testfunc(int a = 20)
{}
~~~

​         （3）缺省值必须是常量或者全局变量。

​         （4）C语言不支持。



## 五、重载

函数重载是一种特殊情况，C++允许在同一作用域中声明几个类似的同名函数，这些同名函数的形参列表（参数个数，类型，顺序）必须不同，常用来处理实现功能类似数据类型不同的问题。在C++中，不仅函数可以重载，运算符也可以重载。

函数重载的规则：

- 函数名称必须相同。
- 参数列表必须不同（个数不同、类型不同、参数排列顺序不同等）。
- 函数的返回类型可以相同也可以不相同。
- 仅仅返回类型不同不足以成为函数的重载。

~~~c++
//重载只被C++支持，C不支持

int Add(int a, int b){
	return a + b;
}

double Add(double a, double b){
    return a + b;
}
int Add(int a,int c,int d ){
    return a + c - d;
}
int main(){
	Add(10,20);
    Add(10.0,20.0);
    Add(10,30,10);
    return 0;
}
~~~

重载涉及到多态，目前不过多阐述，以后会在进行说明阐述。



以上文章如有问题，欢迎大家提出修改意见，谢谢。