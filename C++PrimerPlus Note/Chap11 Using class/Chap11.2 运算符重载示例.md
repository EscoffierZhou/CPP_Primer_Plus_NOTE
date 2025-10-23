# Chap11.2 运算符重载示例

**通过重载op来计算小时+分钟组合**

```cpp
// mytime0.h
#ifndef MYTIME0_H_
#define MYTIME0_H_

class Time
{
private:
    int hours;
    int minutes;
public:
    Time();
    TIme(int h,int m = 0);
    void AddMin(int m);
    void ADdHr(int h);
    void Reset(int h = 0,int m = 0);
    Time Sum(const Time &t)const;
    void Show()const;
};
#endif
```

**使用`iostream`的`cout`,比直接导入`std::out`到整个空间更经济**

 ```cpp
 // mytime0.cpp
 #include<iostream>
 #include"mytime0.h"
 
 Time::Time() // constructor(0 initialize)
 {
     hours = minutes = 0;
 }
 Time::Time(int h,int m)
 {
 	hours = h;
     minutes = m;
 }
 void Time::AddMin(int m)
 {
     minutes += m;
     hours += minutes / 60;
     minuts %= 60;
 }
 void Time::AddHr(int h)
 {
     hours += h;
 }
 void Time::Reset(int h,int m)
 {
 	hours = h;
     minutes = m;
 }
 // 优先按引用传递,速度更快
 Time Time::Sum(const Time &t)const
 {
     Time sum;
     sum.minutes = minutes + t.minutes;
     sum.hours = hours + t.hours + sum.minutes / 60;
     sum.minutes %= 60;
     return sum;
 }
 void Time::show()const
 {
 	std::cout << hours <<" " << minutes; 
 }
 ```

```cpp
// 11.3 usetime0.cpp
#include<iostream>
#include"mytime0.h"
int main()
{
	using std::cout;
    using std::endl;
    Time planning;
    Time coding(2,40);
    Time fixing(5,55);
    TIme total;
    
    planning.Show();
    coding.Show();
    fixing.Show();
    total = coding.Sum(fixing);
    total.Show();
    
    return 0;
}
```

## 1.添加加法运算符

```cpp
// 11.4 mytime1.h
#ifndef MYTIME1_H_
#define MYTIME1_H_
class Time
{
private:
    int hours;
    int minutes;
public:
	Time();
    Time(int h,int m = 0);
    void AddMIn(int m);
    void AddHr(int h);
    void Reset
}
```

