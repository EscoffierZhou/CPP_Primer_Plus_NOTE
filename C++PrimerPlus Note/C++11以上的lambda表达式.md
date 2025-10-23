# C++ 中的 Lambda 表达式

在C++11和更高的版本中,lambda表达式是一种**在被调用的位置或作为参数传递给函数的位置**定义匿名函数对象的简便方法(在传入参数的位置被调用)

## lambda表达式的各个部分

```cpp
#include<algorithm>
#include<cmath>
void abssort(float *X,unsigned n)
{
	std::sort(x,x+n,
              [](float a,float b){
                  return(std::abs(a) < std::abs(b));
              }
    );
}
```

>   其实这里就可以看出来,这里lambda表达式被作为参数了

```cpp
[=]()mutable throw()->int
{
	int n = x + y;
    x = y ;
    y = n;
    return n;
}
```

>   `[]`:表示`capture`子句(lambda引导,决定带值引入/带引用传入引入)
>
>   `()`:表示参数列表==(可选)==
>
>   `mutable`:表示mutable规范,是否允许修改副本而不能修改原始项==(可选)==
>
>   `throw()`:exception-specification==(可选)==
>
>   `-> int`:trailing-return-type==(可选)==
>
>   `{}`:Lambda 体。

## Capture子句

Lambda表达式可以:引入新的变量/引入周边范围的变量

>如果是`[=]`表示:按值访问
>
>如果是`[&]`表示:按引用访问
>
>如果是`[]`表示:主体不访问封闭范围的变量

如果想要**通过引用访问外部变量`total`**+**通过值访问外部变量`factor`**

>```cpp
>[&total,factor] // 正常写法
>[factor,&total] // 换位置不变
>[&,factor]		// 全部外部变量都默认引用访问 + 但引用factor的值
>[=,&total]		// 全部外部变量都默认值访问 + 但total按引用访问
>```

Capture语句的原则

>1.唯一性原则:捕获列表的每个变量只能出现一次
>
>2.默认捕获和显式捕获不冲突:设置了默认捕获方法,显式捕获不能与默认相同
>
>3.`this`的特殊性:`this`默认按值捕获,所以如果是按值捕获不能显式写`this`
>
>```cpp
> struct S { void f(int i); };
>
>void S::f(int i) {	// 默认是按值
>    [&, i]{};      // OK:默认引用,i额外按值引用
>    [&, &i]{};     // ERROR:冗余表达
>    [=, this]{};   // ERROR:冗余表达
>    [=, *this]{ }; // OK (C++17):默认按值,创建副本后按值
>    [i, i]{};      // ERROR:唯一性准则
>}
>// 包拓展,使用...表示
>template<class... Args>
>void f(Args... args) {
>    auto x = [args...] { return g(args...); };
>    x();
>}
>```

多线程lambda的注意点:

>引用捕获可用于修改外部变量，而值捕获却不能实现此操作
>
>引用捕获会反映外部变量的更新，而值捕获不会。
>
>引用捕获引入生存期依赖项，而值捕获却没有生存期依赖项

通用捕获(C++14)

>   在C++14中可在Capture子句中引入并初始化新的变量,和封闭范围无关
>
>   初始化可以使用任何任意表达式表示,所以可以捕获周围移动变量
>
>   ```cpp
>   pNums = make_unique<vector<int>>(nums);
>   auto a = [ptr = move(pNums)]()
>   {
>   	// use ptr
>   }
>   ```

## 参数列表

lambda既可以捕获变量也可以接受输入参数,大多时候可选的,类似函数参数列表

```cpp
auto y = [](int first,int second)
{
    return first + second;
};
```

>   在不将自变量传递到Lambda表达式,并且Lambda声明符中不包含`exception-specification`/`trailing-return-type`/`mutable`可以省略`[]`

在C++14中,如果参数类型是泛型,则可以使用`auto`关键字作为类型关键字

```cpp
auto y = [](auto first,auto second)
{
	return first + second;
};
```

>   **lambda表达式还支持嵌套lambda表达式(见高阶lambda表达式)**

## mutable规范

通常Lambda的函数调用运算符是`const-by-value`

如果利用`mutable`关键字.lambda表达式的主体可以修改通过值捕获的变量

## 异常规范

可以使用`noexcept`来指示lambda表达式不会引发任何异常

```cpp
int main()
{
    []()noexcept{throw 5;}();
}
```

## 返回类型

lambda表达式的返回类型是自动推导的,无需使用`auto`关键字,除非使用了`trailing-return-type`(就是后面通过->指定的返回类型)

```cpp
auto x1 =[](int i){return i;};
auto x2 =[]{return{1,2};}		//ERROR
// 禁止从{}列表自动推导lambda的返回类型
```

主要是为了**避免悬垂引用 (Dangling References)** 的风险。

>   <font color=deeppink>如果编译器被允许将 return {1, 2}; 的返回类型推导为 `std::initializer_list<int>`，那么这个` std::initializer_list` 会引用一个临时的、**在 Lambda 函数返回后立即被销毁的底层数组**。</font>接收到这个 `std::initializer_list` 后，它内部的指针将指向无效的内存，任何对它的访问都将导致未定义行为 

```cpp
// 改正方法(1):尾置具体类型
auto x2_vector = []() -> std::vector<int> {
    return {1, 2};	// 显式返回一个vector
};
auto x2_pair = []() -> std::pair<int, int> {
    return {1, 2};  // 显式返回一个pair
};

// 改正方法(2):在Lambda表达式内部声明具体类型
auto x2_il = []() {
    auto list = {1, 2}; 
    // 特殊规则生效，list 被推导为 std::initializer_list<int>
    return list;   
};
```

## Lambda体

```cpp
#include <iostream>
using namespace std;

int main()
{
   int m = 0;
   int n = 0;
   [&, n] (int a) mutable { m = ++n + a; }(4);
   // m通过按引用捕获,n通过值捕获(创建副本)
   // mutable表示允许在Lambda函数体内修改"按值捕获的变量"
    	// 默认情况下，按值捕获的变量是const
   // (4)表示直接调用Lambda函数,并将4传递给a
   cout << m << endl << n << endl;
}
```

## `generate`和变量生命周期

**1.自动存储持续时间(Automatic Storage Duration)**

>   即局部变量,在`{}`时创建,离开代码块立即被销毁

**2.静态存储持续时间 (Static Storage Duration)**

>   即全局变量或<font color=red>使用`static`关键字修饰的局部变量</font>,程序结束时才被销毁。
>
>   **都不是在`main()`函数中声明的,要在`#define`后面的那种**

**3.Lambda表达式和变量声明周期**

>   **Lambda只能捕获自动变量**,如果是全局变量,直接在函数体内调用就行
>
>   **捕获的本质是延长一个本应被销毁的局部变量的生命周期**

**4.`generate`和`static`对`vector`的迭代**

>   `std::generate`是一个算法,会使用一个生成器函数重复调用的结果来填充一个容器,生成器每次被调用的时候都会返回下一个值并且能记住上一次生成的数
>
>   ```cpp
>   #include <iostream>
>   #include <vector>
>   #include <algorithm> 
>   
>   int main() {
>       std::vector<int> v(5);
>       std::generate(v.begin(), v.end(), []() {
>           // generate 第一次调用 lambda 时执行一次。
>           static int counter = 0;
>           // 在 lambda 体内使用这个静态变量counter,没有被列在捕获列表中！
>           // static提供跨多次调用的持久状态避免被刷新
>           return counter++; 
>       });
>       for (int i : v) {
>           std::cout << i << " ";
>           // 0 1 2 3 4
>       }
>       std::cout << std::endl;
>       return 0;
>   }
>   ```
>
>   ```cpp
>   // 更简单的写法
>   void fillVector(vector<int>& v) // 如果v是5个空,0 1 2 3 4
>   {
>       static int nextValue = 0;
>       generate(v.begin(), v.end(), [] { return nextValue++; });
>   }
>   ```
>
>   ```cpp
>   // 完整的generate_n的用法
>   // 该 lambda 表达式将 vector 对象的元素指派给前两个元素之和
>   // compile with: /W4 /EHsc
>   #include <algorithm>
>   #include <iostream>
>   #include <vector>
>   #include <string>
>   
>   using namespace std;
>   
>   // 基于通用容器C编写的,可以打印任何通过print输出的容器
>   // typename C是一个占位符,后序会换成具体容器名
>   // 但是需要和auto配合(判断内部类型)
>   template <typename C> void print(const string& s, const C& c) {
>       cout << s;
>       for (const auto& e : c) {
>           cout << e << " ";
>       }
>       cout << endl;
>   }
>   
>   void fillVector(vector<int>& v)
>   {
>       static int nextValue = 1;
>       generate(v.begin(), v.end(), [] { return nextValue++; });
>   }
>   
>   int main()
>   {
>       const int elementCount = 9;
>   
>       vector<int> v(elementCount, 1);
>       int x = 1;
>       int y = 1;
>   
>       generate_n(v.begin() + 2,
>           elementCount - 2,
>           [=]() mutable throw() -> int { 
>           int n = x + y;
>           x = y;
>           y = n;
>           return n;
>       });
>       print("vector v after call to generate_n() with lambda: ", v);
>   
>   
>       cout << "x: " << x << " y: " << y << endl;
>   
>       fillVector(v);
>       print("vector v after 1st call to fillVector(): ", v);
>       fillVector(v);
>       print("vector v after 2nd call to fillVector(): ", v);
>   }
>   ```

## `constexpr`Lambda表达式

在常量表达式中允许初始化捕获或引入的每个数据成员时，可以将 Lambda 表达式声明为 **`constexpr`**（或在常量表达式中使用它）。

如果传入的参数是编译期常量，整个 Lambda 可以在**编译期间**被求值。这对于元编程和性能优化非常有用。

```cpp
constexpr auto add = [](int x, int y) { return x + y; };

// 下面这行代码在编译时就会计算出结果 5
constexpr int result = add(2, 3);
```

## Lambda表达式的后序以及语法糖

**1.C++14:泛型Lambda函数**

>   使用auto关键字作为Lambda参数的类型
>
>   使其能接受任何类型的参数，就像一个迷你的函数模板。
>
>   ```cpp
>   // 一个可以打印任何类型参数的 Lambda
>   auto print_anything = [](const auto& value) {
>       std::cout << value << std::endl;
>   };
>   
>   print_anything(10);       // OK, value 的类型是 int
>   print_anything(3.14);     // OK, value 的类型是 double
>   print_anything("hello");  // OK, value 的类型是 const char*
>   ```

**2. 无状态 Lambda (Stateless Lambdas) 与函数指针**

>   无状态指的是`[]`内部为空的
>
>   C++ 标准规定，无状态 Lambda 可以被隐式转换成一个常规的函数指针。
>
>   目的:C风格的代码只认识函数指针
>
>   ```cpp
>   void take_a_function_pointer(void (*func_ptr)(int)) {
>       func_ptr(42);
>   }
>   
>   int main() {
>       auto my_lambda = [](int x) {
>           std::cout << "Lambda called with: " << x << std::endl;
>       };
>   
>       take_a_function_pointer(my_lambda); // OK! 隐式转换为函数指针
>       take_a_function_pointer([](int x){ /* ... */ }); // 直接传递也可以
>   }
>   ```

**3.立即调用 Lambda 表达式 (IILE)**

>   对于`[](){...}();`它有一个专门的名字叫 IILE。
>
>   它最经典的一个用途是：**在复杂的逻辑中初始化一个 const 变量**。
>
>   ```cpp
>   // 假设我们需要根据条件初始化一个常量
>   const int complicated_value = [] {
>       int temp = some_function();
>       if (temp > 10) {
>           return temp * 2;
>       } else {
>           return temp + 5;
>       }
>   }(); // <-- 立即调用
>   ```

## 工程中的使用原则

**1.保持简洁，就近原则:Lambda 的最大优势是就近定义**

**2.谨慎使用默认捕获**: 在大型函数或类方法中，[=] 和 [&] 可能隐藏风险。

>   [=] 可能会意外拷贝大的对象，造成性能问题；
>
>   [&] 在异步编程（多线程）中尤其危险，如果 Lambda 的生命周期超过了它所引用的局部变量，就会导致**悬垂引用 (Dangling Reference)**

**3.优先按值捕获简单类型**: 对于int, double等基础类型优先使用按值捕获

## 算法竞赛的使用原则

1.  **STL 算法的“超级胶水”**: 这是 Lambda 最核心的用途。几乎所有的 STL 算法（sort, find_if, for_each, generate_n 等）都可以和 Lambda 完美配合，让你用极少的代码实现复杂的逻辑。
2.  **大胆使用默认捕获**: 在一个简短的算法题函数体中，上下文非常清晰，变量的生命周期一目了然。使用 [&] 和 [=] 可以节省大量的打字时间，而且几乎没有风险。
3.  **用 Lambda 实现递归和 DFS/BFS**: 这是一个非常强大的技巧。通过捕获列表，你可以让一个 Lambda 递归地调用自己，并且可以轻松访问外部的状态（如 visited 数组、结果集等），而不需要把它们作为冗长的参数传来传去

>   ```cpp
>   vector<vector<int>> adj;
>   vector<bool> visited;
>   // 使用 std::function 来定义一个可递归的 lambda
>   function<void(int)> dfs = [&](int u) {
>       visited[u] = true;
>       for (int v : adj[u]) {
>           if (!visited[v]) {
>               dfs(v); // 递归调用
>           }
>       }
>   };
>   dfs(0);
>   ```

4.   **状态生成器**: 就像我们看到的 `generate_n` 生成斐波那契数列的例子，利用 `mutable` 和捕获，Lambda 可以成为一个强大的、带有内部状态的生成器。