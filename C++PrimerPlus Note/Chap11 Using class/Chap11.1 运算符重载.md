# Chap11.1 运算符重载

**运算符重载是一种形式的C++多态**

**将数组中的两个对象进行相加**

```cpp
// 未使用运算符重载
for(int i = 0;i < 20;i++)
    evening[i] = sam[i] + janet[i];
// 使用运算符重载
evening = sam + janet;
```

**在C++运算符重载使用`operator op`关键字(`op`就是有效的运算符)**

>   构造函数原型Review
>
>   ```cpp
>   // 在.h文件中的初始化
>   Class Stock{
>   public:
>       Stock(const std::string&co,long n = 0;double pr = 0.0);
>   private:
>       std::string company;
>       long shares;
>       double share_val;
>   }
>   // 在.cpp文件中的初始化(默认构造赋值方法)
>   // 构造函数的主体
>   Stock::Stock(const std::string& co, long n, double pr) {
>       company = co;
>       shares = n; 
>       share_val = pr;
>   }
>   // 在.cpp文件中的初始化(直接拷贝构造)
>   // 成员初始化列表构造函数的主体
>   Stock::Stock(const std::string& co, long n, double pr) 
>   	:company(co),shares(n),share_val(pr);
>   {
>       company = co;
>       shares = n; 
>       share_val = pr;
>   }
>   ```
>
>   **成员初始化列表的必要使用**
>
>   >1.初始化`const`变量
>   >
>   >2.初始化引用成员变量
>   >
>   >3.初始化没有默认构造函数的类成员

```cpp
class INTArray{
private:
    int *data;
    int size;
public:
    // 构造函数
    IntArray(int s) 
        : size(s) 
    { // <--- 这是成员初始化列表
        // 这是构造函数体
        if (s <= 0) {
            throw std::invalid_argument("size must be positive.");
        }
        data = new int[size];
        // 初始化数组元素为 0
        for (int i = 0; i < size; ++i) {
            data[i] = 0;
        }
    }
    
    // 析构函数
    ~IntArray() {
        delete[] data;
    }
    // 重载函数
    INTAarray operator +(const INTArray& other)const{
        if(size != other.size){
            throw std::invalid_argument("Positive");
        }
        // 创建一个新的 IntArray 存储结果
        IntArray result(size);
        for (int i = 0; i < size; ++i) {
            result.data[i] = this->data[i] + other.data[i];
        }
        return result;
    }
};
```

