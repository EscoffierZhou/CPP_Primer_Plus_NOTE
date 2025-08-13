# Chap10.3 Constructors & Destructors

(构造函数和析构函数)

**首先目前还不能直接像结构体一样直接初始化**

>   因为部分数据是私有成员,需要使用成员函数来访问数据成员们

**所以产生了类构造函数(constructor)专门用于构造新对象,并且赋值!!**

## 1.声明和定义构造函数

由于对象提供了3个值,所以构造函数有3个参数

```cpp
// constructor prototype
Stock(const string & co,long n = 0;double pr = 0.0);
// 首先是指向字符串的指针,用于初始化company
// 然后是两个参数提供值,注意没有返回类型

// constructor definition
Stock::stock(const string & co,long n,double pr)
{
    company = co;
    if(n < 0)
    {
        std::cerr << "Can't be negative" << std::endl;
        shares = 0;
    }
    else
        shares = n;
    share_val = pr;
    set_tot();
}
```

>   **反正无论如何,都不能使用成员变量名来初始化,否则会`var = var;`失败**

## 2.使用构造函数(constructor)

**1.显式调用构造函数**

```cpp
Stock food = Stock{"World Cabbage",205,1.25};
```

**2.隐式调用构造函数**

```cpp
// 以下两种写法等价
Stock garment("Furry Mason".50,2.5);
Stock garment = Stock("Furry Mason".50,2.5);
```

**3.和new一起使用创建类对象(直接initialize)**

```cpp
Stock * pstock = new Stock("Electroshock Games",18,19.0);
```

**需要注意:创建的对象可以调用方法,但是无法调用构造函数**

## 3.默认构造函数

**默认构造函数实在未提供显式初始值用于初始化对象的构造函数**

```cpp
// 使用编译器自带的默认构造函数
// 在没有声明任何构造函数的情况下,C++编译器会自动生成一个
// 但是只要声明了一个,C++编译器就不会生成了

// 方法1:自己写,为所有参数提供默认参数
Stock(const string & co = "Error",int n = 0,double pr = 0.0);

// 方法2:重载,另写一个无参构造函数
Stock();
```

>   上述方法只能用一个,否则会导致`ambiguous call`的错误

创建了默认构造函数后,就可以声明对象变量了,而不需要显式初始化

```cpp
Stock first;				// default constructor implicity
Stock first = Stock();		// explicity
Stock *prelief = new Stock; // implicity
```

>   在implicity隐式调用的时候,不要使用`()`

## 4.析构函数

对象过期的时候,程序会自动调用一个成员函数(destructor)

析构函数的格式是在前面加上`~`

```cpp
// destructor prototype
~Stock();
// destructor definition
Stock::~Stock(){} //一般不需要加操作
// destructor definition
Stock::~Stock(){cout << "bye" << endl;}
```

**通常不需要代码中显式调用析构函数,如果是static静态,则程序结束自动调用**

**如果是自动类对象,则析构函数在代码块完成后调用**

**如果对象是`new`创建的,则使用`delete`释放的时候自动调用**

**和前面一样,如果不声明析构函数,C++编译器会帮你写一个**

## 5.改进Stock类

```cpp
// 10.4 stock10.h
#ifndef STOCK10_H_
#define STOCK01_H_
#include<string>

class Stock
{
private:
    std::string company;
    long shares;
    double share_val;
    double total_val;
    void set_tot(){total_val = shares * share_val;}
public:
    Stock();	// constructor
    Stock(const std::string & co,long n = 0;double pr = 0.0);
    ~Stock();	// destructor
    void buy(long num,double price);
    void sell(long num,double price);
    void update(double price);
    void show();
};
#endif
```

```cpp
// 10.5 stock10.cpp
#include<iostream>
#include"stock10.h"
Stock::Stock()		// constructor
{
	std::cout << "Default" << "\n";
    company = "no name";
    shares = 0;
    share_val = 0.0;
    total_val = 0.0;
}
Stock::Stock(const std::string & co,long n,double pr)
{
	std::cout << "Constructor using" << co << "called\n";
    company = co;
    if(n < 0)
    {
        std::cout << "Number of shares can't be negative"
            << company << "share set to 0\n";
        shares = 0;
    }
    else
        shares = n;
    share_val = pr;
    set_tot();
}
// destructor
Stock::~Stock()
{
    std::cout << "Bye" << company << "\n";
}
void Stock::buy(long num,double price)
{
	if(num < 0)
    {
        std::cout << "Can't be negative" 
            << "Transaction aborted\n";
    }
    else
    {
		shares += num;
        share_val = price;
        set_tot();
    }
}
void Stock::sell(long num,double price)
{
	using std::cout;
    if(num < 0)
    {
        cout << "Can't be negative" 
            << "Transaction aborted\n";
    }
    else if(num > shares)
    {
        cout << "Can't have more" 
            << "Transaction aborted\n";
    }
    else
    {
        shares -= num;
        share_val = price;
        set_tot();
    }
}
void Stock::update(double price)
{
	share_val = price;
    set_tot();
}
void Stock::show()
{
	using std::cout;
    using std::ios_base;
    ios_base::fmtflags orig = 
        cout.setf(ios_base::fixed,ios_base::floatfield);
    std::streamsize prec = cout.precision(3);
    cout << "Company" << company
         << "Shares" << shares << '\n';
    cout << "Share Price" << share_val;
    cout.precision(2);
    cout << "Total worth" << total_val << '\n';
    cout.setf(orig,ios_base::floatfield);
    cout.precision(prec);
}
```

**然后是用户文件**

```cpp
// 10.6 usestock2.cpp
#include<iostream>
#include"stock10.h"
int main()
{
    {
		using std::cout;
        using std::endl;
        cout << "Using constructor" << endl;
        Stock stock1("NanoSmart",12,20.0);
        stock1.show();
        Stock stock2("Boffo Objects",2,2.0);
        stock2.show();
        cout << "Assigning stock1 to stock2\n";
        stock2 = stock1;
        stock1.show();
        stock2.show();
        cout << "Using constructor reset\n";
        stock1 = Stock("Niffy Foods",10,50.0);
        cout << "Revised stock\n";
        stock1.show();
        cout << "Done"<< endl;
    }
    return 0;
}
```

>   C++允许使用显式创建初始化成员/隐式创建初始化成员/直接转指针
>
>   ```cpp
>   Stock stock1 = Stock("Niffy Foods",10,50.0);
>   Stock stock1("Niffy Foods",10,50.0);
>   stock2 = stock1;
>   ```
>
>   C++允许使用构造函数来重新赋值**(总是会创建临时变量)**会被自动析构掉
>
>   ```cpp
>   Stock stock1 = Stock("Niffy Foods",10,50.0); // Initialize
>   stock1 = Stock("Niffy Foods",10,50.0);		 // renew
>   ```

## 5.列表初始化

```cpp
Stock stock1 = {"Niffy Foods",10,50.0}; // Initialize
// 等价于
Stock::Stock(const std::string &co,long n = 0;double pr = 0.0);
```

>   `std::initialize_list`后面会讲

## 6.`const`成员

如果对一个对象使用了`const`,然后使用方法(哪怕只是展示)也会失败

>   编译器无法缺点对象不被修改,所以需要修改函数声明
>
>   **(修改参数为const引用或者指向const的指针)**
>
>   **对于没有参数的方法,`const`关键字应该在后面**
>
>   ```cpp
>   void show()const;
>   ```

## 7.总结

构造函数是一种特殊的类成员函数，在创建类对象时被调用。构造函数的名称和类名相同，但通过函数重载，可以创建多个同名的构造函数，条件是每个函数的特征标（参数列表）都不同。另外，构造函数没有声明类型。通常，构造函数用于初始化类对象的成员，初始化应与构造函数的参数列表匹配。

**构造函数原型**

```cpp
Bozo(const char * fname,const char * lname);
```

**初始化对象**

```cpp
Bozo bozetta = bozo("Bozotta","Biggema");
Bozo fufu{"Fufu","Dweeb"};
Bozo *pc = new Bozo{"Popo","Le Peu"};
```

默认构造函数没有参数，因此如果创建对象时没有进行显式地初始化，则将调用默认构造函数。如果程序中没有提供任何构造函数，则编译器会为程序定义一个默认构造函数；否则，必须自己提供默认构造函数。默认构造函数可以没有任何参数；如果有，则必须给所有参数都提供默认值,对于未被初始化的对象，程序将使用默认构造函数来创建

就像对象被创建时程序将调用构造函数一样，当对象被删除时，程
序将调用析构函数。每个类都只能有一个析构函数。析构函数没有返回
类型（连void都没有），也没有参数，其名称为类名称前加上~。例
如，Bozo类的析构函数的原型如下：

```cpp
~Bozo()
```

如果构造函数使用了new，则必须提供使用delete的析构函数。