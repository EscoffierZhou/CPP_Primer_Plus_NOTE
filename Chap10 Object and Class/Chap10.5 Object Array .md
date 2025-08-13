# Chap10.5 Object Array 

用户通常要创建同一个类的多个对象，声明对象数组的方法与标准类型数组相同

```cpp
Stock mystuff[4];
```

当程序创建未被显式初始化的类对象时，总是调用默认构造函数。

(这个类要么没有显式地定义任何构造函数要么定义了一个显式默认构造函数)

每个元素(mystuff[0]、mystuff[1]等）都是Stock对象可以使用Stock方法

```cpp
mystuff[0].update(); // 对第一个元素使用update()
mystuff[3].show();	 // 对第四个元素使用show()
const Stock* tops = mystuff[2].topval(mystuff[i]);
// 比较第二个和第三个元素的大小,然后让tops指针指向更大值
```

**可以使用构造函数来初始化数组元素,必须为每个元素调用构造函数**

```cpp
const int STKS = 4;
Stock stocks[STKS] = {
    Stock{"NanoSmart",12.5,20},
    Stock("Boffo Object",200,2.0),
    Stock("monolithic Obelisks",130,3.25),
    Stock("Fleep Enterprise",60,6.5)
};
// 如果类包含多个构造函数,则可以使用不同的构造函数
const int STKS = 10;
Stock stocks[STKS] = {
    Stock{"NanoSmart",12.5,20},
    Stock(),
    Stock("monolithic Obelisks",130,3.25),
};
// 其余元素将使用默认构造函数进行初始化
```

>   注意初始化的时候会产生临时变量,然后将临时对象内容复制到相应的元素中
>
>   **所以创建类对象数组,必须要有默认构造函数(否则无法自动初始化)**

该程序对4个数组元素进行初始化，显示它们的内容，并找出这些元素中总值最高的一个(每次检查两个元素,使用for循环检查整个数组)

```cpp
// 10.9 usestock2.cpp
#include<iostream>
#include"stock20.h"
const int STKS = 4;
int main()
{
    Stock stocks[STKS] = {
    Stock{"NanoSmart",12.5,20},
    Stock("Boffo Object",200,2.0),
    Stock("monolithic Obelisks",130,3.25),
    Stock("Fleep Enterprise",60,6.5)
	};
    std::cout << "Stock holdings:\n";
    int st;
    for(st = 0;st < STKS;st++)
        stocks[st].show();
    const Stock * top = &stocks[0];
    for(st = 1;st < STKS;st++)
        top = &top->topval(stocks[st]);
    std::out << "\nMost valuable holding:\n";
    top->show();
    return 0;
}
```

然后也可以将C++转化为C风格的代码

```cpp
void Stock::show() const
{
    cout << company
        << shares
        << share_val
        << total_val << '\n';
}

void show(const Stock *this)
{
    cout << this->company
        << this->shares
        << this->share_val
        << this->total_val << '\n';
}
top.show(); // C++
show(&top); // C
```

