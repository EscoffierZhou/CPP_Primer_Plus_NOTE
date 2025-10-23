## 1.`const`和指针的搭配

**1.指向常量的指针(常量(即指针指向的内容)不可修改,指针位置不可改,)**

>   **声明:`cont int* p;`**
>
>   ```cpp
>   int a = 10;
>   const int *p = &a;
>   int b = 20;
>   p = &b // VALID,指针位置可修改
>   *P = 20 // INVALID,指针内容为常量
>   ```

**2.常量指针(常量指针不可修改,指针内容可修改)**

>   **声明:`int* const p;`**
>
>   ```cpp
>   int a = 10;
>   int *const p = &a;
>   int b = 20;
>   p = &b // INVALID,指针位置不可修改
>   *P = 20 // VALID,指针内容可修改
>   ```

**3.指向常量的常量指针**

>   **声明:`const int* const p;`**
>
>   ```cpp
>   int a = 10;
>   const int *const p = &a;
>   int b = 20;
>   p = &b // INVALID,指针位置不可修改
>       *P = 20 // INVALID,指针内容不可修改
>   ```

****

## 2.`const`与指针数组的搭配

**`const`和数组的搭配可以创建出不同限制的数组**

**1.数组元素为常量`const`,指针为变量**

>   `const int* arr[5];`

**2.数组元素为变量,指针为常量`const`**

>   `int *const arr[5] = {ele1,ele2,...}`

**3.数组元素为常量`const`,指针为常量`const`**

>   `const int* const arr[5] = {ele1,ele2,...}`

****

## 3.`const`在函数形参和实参中的区别

**1.按值传递(最常见)**

>   意义:函数会创建一个实参的副本,原始的实参不会受到任何印象
>
>   `void func(const int i)`

**2.按指针传递**

>   意义:函数内部不能通过指针p修改数据
>
>   (1)形参为指向常量的指针:`void func(const int* p)`
>
>   (2)形参为常量指针:`void func(int* const p)`
>
>   (3)实参和形参的匹配:使用一个`int*`的实参初始化`const int*`的形参
>
>   >   ```cpp
>   >   void function(const int* a,b)
>   >   const int* a = &b; // &b的类型是int*
>   >   // 但是需要注意不可用通过a来修改b,因为a是指向常量的指针
>   >   // 但是b可以直接修改,比如:b=20;
>   >   ```

