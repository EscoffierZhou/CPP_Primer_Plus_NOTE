# Chap2 Start C++(Basic Grammar)

P54~P102

## 本章内容

1.创建C++程序。

2.C++程序的一般格式。

3.＃include编译指令。

4.main( )函数。

5.使用cout对象进行输出。

6.在C++程序中加入注释。

7.何时以及如何使用endl。

8.声明和使用变量。

9.使用cin对象进行输入。

10.定义和使用简单函数。

## 1.进入C++

```cpp
// 1.myfirst.cpp
#include <iostream> // 预处理器编译指令
int main()
{
    using namespace std;   
    cout << "C++ programme";
    cout << endl;
    cout << "A simple output" << endl;
    return 0;       // terminate main()
}
```

该示例包含:注释+预处理器编译指令+函数头+函数体+输入输出+结束语句

**C++98中会在`int main()`最后自动添加`return 0;`**

****

**1.作为接口的函数头main函数**

*函数体:`function(argument list)`*

`main()`函数不接受任何参数的传入

int类型表示函数最后的返回值是整型

>可替代的写法:`int main()`/`main()`/`int main(void)`/`void main()`
>
>**传统的ANSI/ISOC++标准是必须要有返回值的,现在倒是无所谓了**

`main()`函数在程序中必须存在,因为程序需要从该函数开始执行->未定义错误

如果是动态链接库(DLL)模块(控制器芯片)可能不要`main()`但仍有``_tmain()``

****

**2.C++注释**

使用`//`来作为行注释

使用`/* */`来作为块注释

****

**3.C++预处理器和iostream文件**

这里iostream文件是一种对基本C++进行编译的预处理器

(input-output-stream)输入输出流:包含`cout`,`cin`等用法

`#`开头的视作编译指令

<font color=red>原始文件并没有被修改,源代码文件和iostream组成复合文件进行编译</font>

****

**4.头文件h**

**头文件是支持一组特定工具的文件**

`iostream`是一种包含文件,包含于其它文件之中

| C++ old | iostream.h | C++可用                |
| ------- | ---------- | ---------------------- |
| C old   | math.h     | C/C++都可用            |
| C++ new | iostream   | C++可用(namespace std) |
| C new   | math       | C++可用(namespace std) |

****

**<font color=red>5.namespace</font>**

`using namespace std;`是using编译指令(Chap9再学)

namespace名称空间存在的意义就是对相同命名的函数进行选择

```
// 比如 CIEL 和 KAF 公司产品都有KAMITSUBAKI()函数
CIEL::KAMITSUBAKI("1st ONE-MAN LIVE")
KAF::KAMITSUBAKI("4th ONE-MAN LIVE")
```

**所以如果使用C++的标准组件,即名为`std::component()`**可以偷懒

```cpp
# 完整写法
std::cin >> CIEL;
std::cout << "I LOVE CIEL"<< std::endl;
# lazy approach
using namespace std;
cin >> CIEL;
cout << "I LOVE CIEL"<< endl;
```

****

**<font color=red>6.`cout`和函数运算符重载</font>**

(这里默认使用了lazy approach,即`std::cout`)

1.输出文字流:`cout << "My waifu CIEL";`

2.输出文字对象:`cout << sentence << endl`

**(其中`<<`将字符串发送给cout,cout是一个预定义的对象[类的示例])**

**函数运算符重载`<`:通过重载使得运算符根据上下文有不同的含义**

>   C中的&:可以表示地址运算符,可以表示按位AND运算符
>
>   C中的\*:可以表示乘法运算符,可以表示指针解出引用

****

**7.控制符`endl`**

`endl`是作用于`cout`的符号,因此被称作控制符(manipulator)

其主要作用就是将光标移动到下一行开头

可以在语句之后声明;可以单独起一行声明

**当然转义序列`\n`也可以在C++中使用,直接加入语句最后**

****

**8.C++的书写标准**

每个token之间必须分开,token之间不要产生冲突

空格和回车在不影响token的前提下可以随便整

每条语句占一行

每个函数都有两个花括号(每个花括号各占一行)

函数中的语句都相对于花括号进行缩进

**圆括号周围没有空白**

****

**9.HUAWEI华为C++编写标准**

>   ```cpp
>   变量
>   【规则】指针变量、表示资源描述符的变量、BOOL变量声明必须赋予初值
>   
>   【规则】指向资源句柄或者描述符的变量，在资源释放后立即赋予新值
>   
>   【规则】类成员变量必须在构造函数中赋予初值
>   
>   【规则】不对指针变量进行sizeof操作
>   
>   【建议】尽量使用const
>   
>   【建议】如果全局变量的访问涉及多个线程，需要考虑多线程竞争条件问题。
>   
>   【建议】同一个函数内，局部变量所占用的空间应小于16KB
>   ```
>
>   ```cpp
>   函数
>   【规则】数组作为函数参数时，必须同时将其长度作为函数的参数
>   
>   【规则】不对内容进行修改的指针型参数，定义为const
>   
>   【建议】谨慎使用不可重入函数
>   
>   【建议】字符串或指针作为函数参数时，请检查参数是否为NULL
>   ```
>
>   ```cpp
>   循环
>   【规则】循环必须有退出条件
>   ```
>
>   ```cpp
>   类
>   【规则】如果构造函数中分配了需要手动释放的资源，则必须有析构函数
>   
>   【规则】构造函数内不能做任何可能失败的操作
>   
>   【规则】严禁在构造函数中创建线程
>   
>   【规则】严禁出现delete this操作
>   
>   【建议】尽量避免定义public成员
>   ```
>
>   ```cpp
>   字符串/数组操作
>   【规则】确保有足够的存储空间
>   
>   【规则】对字符串进行存储操作，确保字符串有'\0'结束符
>   
>   【规则】外部数据作为数组索引时必须确保其在数组大小范围内
>   
>   【规则】外部数据作为复制内存操作函数的长度时，需要校验其合法性
>   
>   【规则】调用格式化函数时，禁止format参数由外部可控
>   
>   【规则】调用格式化函数时，format中参数的类型与个数必须与实际参数类型一致
>   ```
>
>   ```cpp
>   整数
>   【规则】整数之间运算时必须严格检查，确保不会出现溢出、符号反转或除以0。
>   
>   【规则】整型表达式比较或赋值为一种更大类型，必须先用这种更大类型对它进行求值
>   
>   【规则】禁止对有符号整数进行位操作运算
>   
>   【规则】禁止整数与指针间互相转化
>   
>   【规则】循环次数如果受外部数据控制，需要校验其合法性
>   
>   【规则】禁止对指针进行逻辑或位运算
>   ```
>
>   ```cpp
>   内存
>   【规则】内存申请前，必须对申请内存大小进行合法性校验
>   
>   【规则】内存分配后必须判断是否成功
>   
>   【规则】禁止引用未初始化的内存
>   
>   【规则】内存释放之后立即赋予新值
>   ```

## **2.C++语句**

**1.变量**

`int carrots;`:需要的内存+内存单元的名称(carrots为变量)

定义声明语句:使得编译器为变量分配内存情况

(复杂情况下还有引用声明)

**C++和C/Pascal不同,变量声明不需要只在开始全部声明**

****

**2.赋值语句**

赋值的顺序按照从右往左的顺序:

`yamaha = baldwin = steinway = 88`

实际情况为:88->steinway->baldwin->yamaha

赋值可以对变量的值进行修改:

`carrots = carrots - 1`

****

**3.`cout`/`printf`/`print`**

printf要求对输出的变量进行严格的类型定义:

`printf("print a string : %s\n","25");`

`printf("print a integer : %d\n",25);`

cout存在智能特性(来源于面向对象的特性),`<<`可以自行调整重载

print是C++13.2引入的规则,使用`{}`,存在智能特性

## 3.Oth C++语句

```cpp
#include <iostream>
{
    using namespace std;
    int carrots;
    cout << "How many carrots do you have" << endl;
    cin >> carrots;
    cout << "Here are two more";
    carrots = carrots + 2;
    cout << "Now you have " << carrots << "carrots" << endl;
    return 0;
}
```

**1.`cin`语句**

也是`iostream`中的一个智能对象,可以从输入流中抽取字符

**2.拼接`cout`语句**

一种面对结果的编程方法

**3.类简介**

类是C++中面向对象编程(OOP)的核心概念

要定义类,需要描述`表示什么信息`和`可对数据执行的操作`

类描述了一种数据类型的全部属性,对象是根据这下描述创建的实体

对于`int carrots`:

>   创建了一个类型为`int`的变量(carrots)

对于`cout`:

>   它是一个`ostream`类对象(实体)

对于`cin`:

>   它是一个`istream`类对象(实体)

## ４.函数

**1.函数简介**

这里主要讨论有返回值和无返回值的两种函数

函数的具体内容在Chap7和Chap8

****

**2.有返回值的函数**

有返回值的函数将生成一个值,这个值可以在任意表达式/变量中使用

例如`sqrt()`的表达式中会进行`sqrt()`的**函数调用**

`()`中的值是传递给函数的值,也叫参数,比如`sqrt(4)`

**函数原型(prototype):指出涉及的函数中,值类型的限定**

>   比如`sqrt()`的原型:`double sqrt(double)`
>
>   所以调用的时候也应该是`double`数据类型
>
>   ```cpp
>double x; 
>   x = sqrt(3.8);
>```

**函数原型和函数定义的区别:**

>   函数原型只描述函数接口,描述的是发送值和返回值
>
>   函数定义包含函数的具体代码

**函数原型的使用**

>   ```cpp
>#include <iostream>
>   
>// 函数原型,更方便维护
>   double calculate_area(double radius);
>
>   int main() {
>double r = 5.0;
>   // 在这里调用函数，因为前面有原型，编译器知道它是合法的
>   double area = calculate_area(r);
>   std::cout << "The area is: " << area << std::endl;
>   return 0;
>   }
>   
>   // 函数定义
>   double calculate_area(double radius) {
>return 3.14159 * radius * radius;
>   }
>```
>   
>   1.对于大项目更容易维护
>   
>2.对于编译器友好
>   
>**(能写尽写)**
>   
>   **(如果函数定义在main之前就可以不用写原型)**
>   
>   **(如果头文件中定义了原型,可以直接调用)**
>   
>   **<font color=red>函数原型入口是参数列表,如果参数列表相同无法重载</font>**

```cpp
// 2.4 sqrt.cpp
#include <iostream>
#include <cmath> // old one:<math.h>
int main()
{
    using namespace std;
    double area;
    cout << "Enter the floor area , in squar feet , of your home";
    cin >> area;
    double side;
    side = sqrt(area);
    cout << "That's the equilvalent of a square" << side
         << " feet to the side" << endl;
    cout << "How fascinating!" << endl;
    return 0;
}
```

`typename variable-name`是变量声明的语法,先声明再赋值

`typename variable-name = expression`是变量初始化的语法,直接赋值

****

**2.函数变体**

1.传入双参数的函数:`double pow(double,double)`计算底和对应的幂

2.不接受参数的函数:`int rand(void)`一般直接调用`guess  = rand()`

3.没有返回值的函数:`void print_num(double)`直接调用

**没有返回值的函数称作:过程(procedure)或子程序(subroutine)**

****

**3.自定义函数**

>   ```cpp
>   // 2.5 ourfunc.cpp
>   #include <iostream>
>   void simon(int) //prototype
>   int main()
>   {
>       using namespace std;
>       simon(3);
>       cout << "Pick an integer: ";
>       int count;
>       cin >> count;
>       simon(count);
>       cout << "Done!" << endl;
>       return 0;
>   }
>   void simon(int n)
>   {
>       using namespace std;
>       cout << "Simon says touch your toes" << n << "times." << endl;
>   }
>   ```
>
>   **函数格式**
>
>   ```cpp
>   type functioname(argumentlist)
>   {
>       statements
>   }
>   ```
>
>   **函数头决定了如何调用该函数**
>
>   比如`void simon(int n)`不能通过`num = simon(3)`调用,无返回值

**有关`main()`函数**

>在`int main()`的时候设置了返回值为整型int,如何调用返回值?
>
>计算机操作系统看作调用程序,因此`main()`返回给操作系统(退出值)

****

**4.用户定义的有返回值的函数**

>   ```cpp
>   // 2.6 convert.cpp
>   #include <iostream>
>   int stonetolib(int); //prototype
>   int main()
>   {
>       using namespaces std;
>       int stone;
>       cout << "Enter the weight in stone: ";
>       cin >> stone:
>       int pounds = stonetolib(stone);
>       cout << stone << " stone = ";
>       cout << pounds << " pounds." << endl;
>       return 0;
>   }
>   int stonetolib(int sts)
>   {
>       return 14 * sts;
>   }
>   ```

****

**5.在多函数程序中使用using编译指令**

`using namespace std;`来访问位于名称空间std中的cout定义

位置:可以放在最前面作用于所有函数/只放在`main()`函数输出

using编译指令的几种形式:

>   1.作用于所有函数:`using namespace std`放在最前面
>
>   2.作用于特定函数:`using namespace std`放在函数前面
>
>   3.使用指定的元素:`using std::cout`
>
>   4.完全不使用using编译指令:`std::cout`

## 5.Review

有多种类型的C++语句，包括下述6种。
>声明语句：定义函数中使用的变量的名称和类型。
>
>赋值语句：使用赋值运算符（=）给变量赋值。
>
>消息语句：将消息发送给对象，激发某种行动。
>
>函数调用：执行函数。被调用的函数执行完毕后，程序返回到函数
>调用语句后面的语句。
>
>函数原型：声明函数的返回类型、函数接受的参数数量和类型。
>
>返回语句：将一个值从被调用的函数那里返回到

类和对象:

>类是用户定义的数据类型规范，它详细描述了如何表示信息以及可
>对数据执行的操作。对象是根据类规范创建的实体，就像简单变量是根
>据数据类型描述创建的实体一样。

库函数和类:

>C++提供了两个用于处理输入和输出的预定义对象（cin和cout），
>它们是istream和ostream类的实例，这两个类是在iostream文件中定义
>的。为ostream类定义的插入运算符（<<）使得将数据插入到输出流成
>为可能；为istream类定义的抽取运算符（>>）能够从输入流中抽取信
>息。cin和cout都是智能对象，能够根据程序上下文自动将信息从一种形
>式转换为另一种形式。
>
>C++可以使用大量的C库函数。要使用库函数，应当包含提供该函
>数原型的头文件。

## 6.题目

1．C++程序的模块叫什么？

>   函数

2．预处理器编译指令`#include<iostream>`是做什么用的？

>   在最终编译的时候和`iostream`文件一起调用

3．语句`using namespace std;`是做什么用的？

>   程序可以使用std空间中的定义

4．什么语句可以用来打印短语“Hello，world”，然后开始新的一
行？

>   `cout << "Hello world" << endl;`

5．什么语句可以用来创建名为cheeses的整数变量？

>   `int cheeses;`

6．什么语句可以用来将值32赋给变量cheeses？

>   `int cheeses = 32;`

7．什么语句可以用来将从键盘输入的值读入变量cheeses中？

>   `cin >> cheeses`

8．什么语句可以用来打印“We have X varieties of cheese,”，其中X
为变量cheeses的当前值。

>   `cout << "We have " << cheeses << " varieties of cheese"<<endl;`

9．下面的函数原型指出了关于函数的哪些信息？

>```cpp
>int froop(double t); //返回值为int 传入参数为 double
>void rattle(int n);  //无返回值,传入参数为int
>int prune(void);     //返回值为int,直接调用
>```

10．定义函数时，在什么情况下不必使用关键字return？

>   void()

11．假设您编写的main( )函数包含如下代码：`cout<< "Please enter your PIN"`而编译器指出cout是一个未知标识符。导致这种问题的原因很可能
是什么？指出3种修复这种问题的方法。

>1.可能没有导入标准库iostream->`#include<iostream>`
>
>2.可能cout不在命名空间->`using namespace std;`
>
>3.可能没有引入标识符->`using std::cout;`

## 7.Code&Homework

略