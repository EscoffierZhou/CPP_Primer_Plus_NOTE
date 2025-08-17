# Chap10.7 Abstract type

对于抽象数据类型(ADT)使用类实现是一种很好的方式

## 1.`Stack`栈

栈存储了多个数据项,其次也有对应的执行操作

>   创建空栈/压入数据/弹出数据/判断栈满/判断栈空

```cpp
// 10.10 stack.h
// class def(basic/function)
#ifndef STACK_H_
#define STACK_H_

typedef unsigned long Item;
class Stack
{
private:
    enum[MAX = 10];  // 类的常量定义
    Item items[MAX]; // 存储数据的空间
    int top;		 // 栈内容的index
public:
    Stack();
    bool isempty()const;
    bool isfull()const;
    bool push(const Item & item);
    bool pop(Item & item);
};
#endif
```

```cpp
// 10.11 stack.cpp
#include"stack.h"
Stack::Stack(){top = 0}
bool Stack::isempty()const
{
    return top==0;
}
bool Stack::isfull()const
{
    return top==MAX;
}
bool Stack::push(const Item & item)
{
    if(top < MAX)
    {
        items[top++] = item;
        return true;
    }
    else
        return false;
}
bool Stack::pop(Item &item)
{
    if(top > 0)
    {
        item = items[--top];
        return true;
    }
    else
        return false;
}
```

```cpp
// 10.12 stacker.cpp
#include<iostream>
#include<cctype>
#include"stack.h"
int main()
{
    using namespace std;
    Stack st;
    char ch;
    unsigned long po;
    while(cin >> ch && toupper(ch) != 'Q')
    {
        while(cin.get() != '\n')
            continue;
        if(!isalpha(ch))
        {
            cout << "\a";
            continue;
        }
        switch(ch)
        {
            case'A':
            case'a': cout << "Enter PO number to add";
                cin >> po;
                if(st.isfull())
                    cout << "stack already full\n";
                else
                    st.push(po);
                break;
            case'P':
            case'p':if(st.isempty())
                cout<< "Empty";
                else
                {
                    st.pop(po);
                    cout << po << "popped\n";
                }
                break;
        }
        cout << "Enter A to add a purchase order.\n"
    }
    cout << "Bye\n";
    return 0;
}
```

