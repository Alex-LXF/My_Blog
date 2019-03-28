# string类 （二）

接上一篇文章，本文将继续对`string`类常用接口进行说明。

## 一、string类对象的容量操作

| 函数名称                         | 功能说明                                             |
| -------------------------------- | ---------------------------------------------------- |
| `size_t size() const`            | 返回字符串的有效字符长度                             |
| `size_t size() const`            | 返回字符串的有效字符长度                             |
| `size_t capacity() const`        | 返回空间的总大小                                     |
| `bool empty() const`             | 检测字符串释放为空串，是返回`true`，否则返回`false`  |
| `void clear()`                   | 清空有效字符                                         |
| `void resize(size_t n, char c)`  | 将有效字符串的个数改为`n`个，多出的空间用字符`c`填充 |
| `void resize(size_t n)`          | 将有效字符串的个数改为`n`个，多出的空间用`0`填充     |
| `void reserve(size_t res_arg=0)` | 为字符串预留空间                                     |

**示例：**

~~~C++
void TestString3()
{

	// 测试 size()、length()、capacity()
	cout << "测试 size()、length()、capacity()" << endl;
	string s("hello, world");
	cout << " s: " << s << endl;
	cout << " s.size() = " << s.size() << endl;
	cout << " s.length() = " << s.length() << endl;
	cout << " s.capacity() = " << s.capacity() << endl;

	// 测试clear()
	cout << "\n测试clear()" << endl;
	s.clear(); // 将s中的字符串清空，清空只是将size清0，不该表空间(容量)的大小
	cout << " s: " << s << endl;
	cout << " s.size() = " << s.size() << endl;  
	cout << " s.capacity() = " << s.capacity() << endl;

	//测试resize(size_t n, char c)、resize(size_t n)
	cout << "\n测试resize(size_t n, char c)、resize(size_t n)" << endl;
	s.resize(10, 'a'); //将s中的有效字符长度改为10，多出位置用'a'填充
	cout << " s: " << s << endl;
	cout << " s.size() = " << s.size() << endl;
	cout << " s.capacity() = " << s.capacity() << endl;

	s.resize(20); //将s中的有效字符长度改为20，多出位置用'\0'填充
	cout << " s: " << s << endl;
	cout << " s.size() = " << s.size() << endl;
	cout << " s.capacity() = " << s.capacity() << endl;

	//测试reserve（）
	cout << "\n测试reserve" << endl;
	string s1;
	s1.resize(100); //测试reserve是否会改变string中有效元素个数
	cout << " s1.size() = " << s1.size() << endl;
	cout << " s1.capacity() = " << s1.capacity() << endl;

	s1.reserve(50); //测试reserve参数小于底层空间大小时，是否会将空间大小缩小
	cout << " s1.size() = " << s1.size() << endl;
	cout << " s1.capacity() = " << s1.capacity() << endl;

}
~~~

**注意：**

1、`size()`与`length()`方法底层实现原理相同，引入`size()`是为了与其他容器的借口保持一致，一般情况下基本都是使用`size()`。

2、`clear()`只是将`string`中的有效字符清空，不改变底层空间大小。

3、`resize(size_t n)`与`resize(size_t n,char c)`都是将字符串中的有效个数改为`n`个，不同的是当字符个数增多时：`resize(size_t n)`用`0`来填充多出的空间，而`resize(size_t n, char c)`是用字符来填充多出的元素空间。注意：`resize`在改变元素个数时，如果元素个数增多可能会改变底层容量大小，如果将元素个数减少，底层空间大小不改变。

4、`reserve(size_t res_arg=0)`：为`string`预留空间，不改变有效元素个数，当`reserve`的参数小于`string`的底层空间总大小时，`reserve`不会改变容量大小。



## 二、string类对象的访问操作

| 函数名称                             | 功能说明                                        |
| ------------------------------------ | ----------------------------------------------- |
| `char& operator[](size_t pos)`       | 返回`pos`位置的字符，`const string` 类对象调用  |
| `const char& operator[](size_t pos)` | 返回`pos`位置的字符，非`const string`类对象调用 |

~~~c++
void TestString4()
{
	string s1("hello world.");
	const string s2("HELLO WORLD.");
	cout << " s1: " << s1 << "  " << " s2: " << s2 << endl;
	cout << " s1[0]: " << s1[0] << "  " << " s2[0]: " << s2[0] <<endl;

	s1[0] = 'H'; //修改s1首元素
	cout << " s1: " << s1 << endl;

	//s2[0] = "h";  //编译失败； const类型对象不可被修改
}
~~~



## 三、string类对象的修改操作

| 函数名称                                              | 功能说明                                                   |
| ----------------------------------------------------- | ---------------------------------------------------------- |
| `void push_back(char c)`                              | 在字符串后尾插字符c                                        |
| `string& append(const char* s)`                       | 在字符串后追加一个字符串                                   |
| `string& operator+=(const string& str)`               | 在字符串后追加字符串str                                    |
| `string& operator+=(const char* s)`                   | 在字符串后追加c个数字符串                                  |
| `string& operator+=(char c) `                         | 在字符串后追加字符c                                        |
| `const char* c_str()const`                            | 返回c格式字符串                                            |
| `size_t find(char c, size_t pos = 0)const`            | 从字符串pos位置开始往后找字符c，返回该字符在字符串中的位置 |
| `size_t rfind(char c, size_t pos = npos)`             | 从字符串pos位置开始往前找字符c,返回该字符在字符串中的位置  |
| `string substr(size_t pos = 0, size_t n = npos)const` | 在str中从pos位置开始，截取n个字符，然后将其返回            |

~~~c++
void TestString5()
{
	string str;
	//测试push_back()
	str.push_back(' '); // 在字符串str后插入空格

	//测试append()
	str.append("hello "); // 在字符串str后插入字符串:hello

	//测试 += 字符 or 字符串
	str += 'w'; // 在字符串后插入字符:w
	str += "orld"; // 在字符串str后插入字符串:orld

	cout << str << endl;

	//测试c_str()
	cout << str.c_str() << endl; // 以c语言的方式打印字符串
	
	//测试find()
	size_t pos = str.find('w'); // 从前往后找w的位置
	cout << " pos = " << pos << endl;
    //测试rfind()
	size_t pos1 = str.rfind('w'); // 从后往前找w的位置
	cout << " pos1 = " << pos1 << endl;


	//测试substr()
	string str2 = str.substr(pos, str.size() - pos); // 从str中从pos位置开始截取str.size()-pos个字符
	cout << " str2: " << str2 << endl;
}
~~~

注意：

1、在`string`尾部追加字符时，`s.push_back(c) / s,append(1,c) / s+='c'`三种实现方式差不多，一般情况下`string`类的`+=`操作用的比较多，`+=`可以连接字符，也可以连接字符串。

2、对`string`操作时，如果能大概预估到放多少字符，可以先通过`reserve`把空间预留好。



## 四、string类输入

对于C-风格字符串，有3种输入方式：

~~~c++
char info[100];
cin >> info;
cin.getline(info, 100); //读取一行，去掉换行符
cin.get(info, 100); //读取一行，保留换行符
~~~

`getline()`和`get()`这两个函数都读取一行的输入，直到达到换行符。然而不同的是`getline()`将丢弃换行符，而`get()`将换行符保留在输入序列中。

对于`string`类对象，有两种方式：

~~~c++
string stuff;
cin >> stuff;
getline(cin,stuff);
~~~

在功能上主要区别在于，`string`版本的`getline()`将自动调整目标`string`对象的大小，使之刚好能储存输入的字符：

~~~C++ 
char fname[10];
string lname;
cin >> fname; // 不能超过10个元素
cin >> lname; // 自动调整大小
cin.getline(fname, 10); // 会截断输入
getline(cin.fname); // 不会截断
~~~

