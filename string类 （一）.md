# string类 （一）

## 一、string类简介

`string` 类是 `STL`中 `basic_string` 模板实例化得到的模板类。其定义如下：

~~~C++
typedef basic_string <char> string;
~~~

我们先看看C++处理字符串的两种方式。第一种是来自C语言，常被称为C-风格字符串。另外一种就是基于`string`类库的方法。

我们先看一下C-风格字符串：

~~~C++
char cat[8] = {'h', 'e', 'l', 'l', 'o', '\0'};  //字符串：hello
~~~

在car数组事例中，将数组初始化为字符串的工作看上去冗长乏味，因为要使用大量的单引号，且必须加上空字符。因此我们有另外一种更好的、将字符数组初始化为字符串的方法，只需要使用双引号扩起字符串就好，这种字符串被称为字符串常量或者字符串字面值。如下：

~~~c++
char cat[] = "hello, world!";
~~~

`ISO/ANSI C++98`标准通过添加`string`类扩展了`C++`库，因此现在可以使用`string`类型的变量而不是字符数组来存储字符串。`string`类使用起来比数组简单，同时提供了一种将字符串作为一种数据类型的表示方法。

要使用`string`类，必须在程序中包含头文件`string`。`string`类位于命名空间`std`中，因此还需要使用`using`编译指令，或者使用`std::string`来引用。我们可以像处理普通变量那样处理字符串。简单举例：

~~~C++
#include <iostream>
#include <string>   //此处为演示，为了方便，后续将不再引入。
using namespace std;
void testString()
{
    string str1;
    str1 = "hello, string类.";
    cout << str1 << endl;
    string str2("我将要学习你。");
    cout << str2  <<endl;
}
~~~



## 二、string类的构造函数

对于类而言，有哪些方法可以创建对象是最重要的，因此我们首先看一下`string`的构造函数。C++11在string类7个构造函数的基础上有新增了两个，现在来认识以下这些构造函数。

| 构造函数                                                     | 描述                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| `string(const char* s)`                                      | 将`string`对象初始化为s指向的`NBTS`                          |
| `string(size_type n, char c)`                                | 创建一个包含n个元素的`string`对象，其中每个元素都被初始化为字符c |
| `string(const string& str)`                                  | 将一个`string`对象初始化为`string`对象`str`(拷贝构造函数)    |
| `string ()`                                                  | 创建一个默认的`string`对象，长度为0（默认构造函数）          |
| `string(const char* s, size_type n)`                         | 将`string`对象初始化为s指向的`NBTS`的前n个字符，即使超过了`NBTS`的结尾。 |
| `template<class Iter> string(Iter begin ,Iter end)`          | 将`string`对象初始化区间[begin,end]内的字符，其中begina和end的行为就像指针用于指定位置，包括begin，不包括end |
| `string(const string& str,string size_type pos= 0,size_type n=npos)` | 将一个`string`对象初始化为对象`str`中从`pos`到结尾或者从`pos`开始的n个字符。 |
| `string(string && str)noexcept`                              | 这是C++11新增的，它将一个`string`对象初始化为`string`对象`str`，并可能修改`str`（移动构造函数） |
| `string(initializer_list<char>il)`                           | C++11新增，它将一个`string`对象初始化为初始化列表il中的字符  |

上述即为`string`的构造函数，`size_type`是一个依赖实现的整型，在头文件`string`中定义。`NBTS`表示已空字符结束的字符串。下面用代码演示前7个构造函数：

~~~c++
// ctor为传统C++构造函数缩写
void stringTest1()
{
	//ctor #1
	string str1("That should be me !"); // 将str1初始化为：That should be me !
	cout << " str1: " << str1 << endl;

	//ctor #2
	string str2(20, '*'); // 将str2初始化为20个*
	cout << " str2: " << str2 << endl;

	//ctor #3
	string str3(str1); // 将str3的内容初始化为str1的内容（拷贝复制）
	cout << " str3: " << str3 << endl;

	//ctor #4
	string str4; // 创建对象str4
	str4 = "Do it !"; // 将 Do it !赋给str4
	cout << " str4: " << str4 << endl;

	//ctor #5
	char array[] = "I am a good student."; //初始化数组
	cout << " array[]:" << array << endl; //输出数组内的字符串
	string str5(array,11); // 截取字符串前11个元素（包括第11个）
	cout << " str5: " << str5 << endl;

	//ctor #6
	string str6(array + 5, array + 11); //从下标5开始截取字符串，到11停止;包括5但不包括11
	cout << " str6: " << str6 << endl;

	//ctor #7
	string str7(str1, 5, 12); //从字符串str1下标5开始截取12个元素复制到str7中
	cout << " str7: " << str7 << endl;
}
~~~

**C++11新增的构造函数**

构造函数`string(string&& str)`类似于拷贝构造函数，但与其不同的是它不保证`str`视为`const`。因此他被称为移动构造函数。

构造函数`string(initializer_list<char>il)`可以将列表初始化语法用于`string`类。

~~~c++
string pinao = {'L','i','s','t'};
~~~



## 三、赋值、拼接和附加（= 、 + 、+=）

string类重载了部分运算符，我们可以利用这些运算符进行一些简单操作。比如：’=‘、‘+’、‘+=’、‘[ ]’。

~~~c++
void TestString2()
{

	string song1("sing na na na na na;");
	cout << " song1: " << song1 << endl;

	string song2 = " na na na na na;"; // 利用‘=’直接赋值
	cout << " song2: " << song2 << endl;

	string song3(" sing it with me na na na na na.");
	cout << " song3: " << song3 << endl;

	song1 += song2; // 利用‘+=’在say的后面附加say2
	cout << " song1: " << song1 << endl;

	string song4 = song1 + song3; // 利用‘+’将say与say3拼接并赋值给say4
	cout << " song4: " << song4 << endl;
    cout << " song1[0]: " << song1[0] << endl; //输出song1的第一个元素 
}
~~~



