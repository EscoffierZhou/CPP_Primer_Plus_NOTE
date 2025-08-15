# Chap10.6 Class scope

目前学过的:在全局(文件)作用域的变量和局部(代码)的作用域

>   全局就在文件任何地方使用,局部变量只能在其所属的代码块中使用

**新的一种作用域:类作用域**

## 在类中必须声明的作用域

在类中定义的名称（如类数据成员名和类成员函数名）的作用域都为整个类，

**作用域为整个类的名称只在该类中是已知的，在类外是不可知的。**

>   因此，可以在不同类中使用相同的类成员名而不会引起冲突。

**另外，类作用域意味着不能从外部直接访问类的成员，公有成员函数也是如此。**

>   也就是说，要调用公有成员函数，必须通过对象

```cpp
Stock sleeper("Exclusive Ore",100,0.25);
sleeper.show(); // Use object to invoke a member function
show(): // INVALID
```

**然后在定义成员函数的时候,必须使用作用域解析运算符**

```cpp
void Stock::update(double price)
{
    //...
}
```

**最后,在类声明或成员函数定义中,可以使用未修饰的成员名称**

>   也就是说在类定义中声明了`set_tot()`,那么类方法的声明就不需要作用域
>
>   ```cpp
>   class Stock {
>   private:
>       double total_val;
>   public:
>       void set_tot();
>       void sell(int num, double price) {
>           set_tot(); // 这里没有使用Stock::Stock set_tot()
>       }
>   };
>   ```
>
>   在其他情况下,必须根据上下文使用:
>
>   直接成员运算符(`.`)
>
>   ```cpp
>   Stock myStock;	// 类对象
>   myStock.sell(10, 25.0); // 使用对象修饰
>   ```
>
>   间接运算符(`->`)
>
>   ```cpp
>   Stock* stock_ptr = new Stock(); // 类对象指针
>   stock_ptr->sell(10, 25.0); // 使用指针限定
>   ```
>
>   作用域解析运算符(`::`)(在类外部定义函数)

```cpp
// Eg:对上面作用域的说明
class Ik
{
private:
    int fuss;
public:
    Ik(int f = 9){fuss = f;}
    void ViewIk() const;
};
void Ik::ViewIk() const
{
    cout << fuss << endl;
}
int main()
{
    Ik * pik = new Ik;['']
    Ik ee = Ik(0);
    ee.ViewIk();
    pik->ViewIk();
}
```

## 1.作用域为类的常量

**目标:在所有对象中创建一个共享的常量**

```cpp
class Bakery
{
private:
    const int Months = 12; //INVLALID
    double costs[Months];
}
```

>   声明类的时候并没有创建对象,没有存储值的空间

**常量声明方法1:在类中声明一个枚举**

>   (因为类声明中声明的**枚举的作用域为整个类**)
>
>   ```cpp
>   class Bakery
>   {
>   private:
>       enum{Months = 12};
>       double costs[Months];
>   }
>   ```

**常量声明方法2(标准做法):使用关键字`static`**

>   ```cpp
>   class bakery
>   {
>   private:
>       static const int Months = 12;
>       double costs[Months];
>   }
>   ```

## 2.作用域内枚举(C++11)

传统的枚举如果两个枚举定义中的枚举量相同(哪怕部分)则会发生冲突

>   ```cpp
>   enum egg{Small,Medium,Large,Jumbo};
>   enum t_shirt{Small,Medium,Large,Jumbo}; // CONFLICT!!
>   ```
>
>   (所以可以通过声明作用域避免这种冲突)(也可以使用`struct`代替`class`)
>
>   ```cpp
>   enum class egg{Small,Medium,Large,Jumbo};
>   enum class t_shirt{Small,Medium,Large,Jumbo}; // 声明为类
>   // 使用方法:需要作用域+变量名
>   egg choice = egg::Large;
>   t_shirt Floyd = t_shirt::Large;
>   ```
>
>   有些情况下常规枚举将自动转换为整型,**但作用域内枚举不能隐式转换为整型**
>
>   ```cpp
>   enum egg_old = {Small,Medium,Large,Jumbo};
>   enum class t_shirt {Small,Medium,Large,Xlarge};
>   egg_old one = Medium;
>   t_shirt rolf = t_shirt::Large;
>   int king = one;
>   int ring = rolf;
>   if(king < Jumbo)
>       std::cout << "Jumbo converted to int before comparison,\n";
>   if(king < t_shirt::Medium)
>       std::cout << "N"
>   ```
>
>   