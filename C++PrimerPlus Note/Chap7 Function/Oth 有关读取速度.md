# Oth 有关读取速度

在性能金字塔上，输入速度大致是这样的：

>   1.`fread `
>
>   一次性读入大块数据到缓冲区，然后自己处理缓冲区(最复杂速度上限最高）
>
>   2.`getchar_unlocked `
>
>   非线程安全的 `getchar`，仅限Linux/POSIX系统 或` getchar`
>
>   3.`scanf`
>
>   4.解除了同步绑定的`cin `
>
>   `(std::ios_base::sync_with_stdio(false); std::cin.tie(NULL);)`
>
>   4.默认的` cin`

## `getchar()`

```cpp
#include <cstdio>  // getchar()
#include <cctype>  // isdigit()

// --- 快速读入模板 ---
// 通常会写成模板函数以支持不同类型, 这里用 int 作为例子
// inline表示内联函数:给编译器的建议,适用于短小简单频繁调用的函数
inline int read() {
    int x = 0, f = 1; 
    // x 存储结果, f 代表符号 (1 为正, -1 为负)
    char ch = getchar(); // 读取第一个字符

    // 1. 跳过所有非数字字符，并检查负号
    while (!isdigit(ch)) {
        if (ch == '-') f = -1; // 遇到负号，记录下来
        ch = getchar();        // 继续读下一个字符
    }

    // 2. 读取数字部分
    while (isdigit(ch)) {
        // 核心部分：将字符转换为数字并累加
        // (ch - '0') 是将字符 '5' 转换为数字 5 的高效方法
        // x * 10 可以用位运算 (x << 3) + (x << 1) 优化，速度更快
        x = (x << 3) + (x << 1) + (ch - '0');
        ch = getchar(); // 继续读下一个字符
    }
    return x * f; // 返回结果，别忘了乘以符号
}

// --- 快速写出模板 ---
// 输出也可能成为瓶颈，所以也需要快速写
inline void write(long long x) {
    if (x < 0) {
        putchar('-'); // 如果是负数，先输出负号
        x = -x;
    }
    // 递归地输出高位
    if (x > 9) {
        write(x / 10);
    }
    // 输出最低位
    putchar(x % 10 + '0'); // 将数字 5 转换为字符 '5'
}


int main() {
    // 使用我们自己的快速读入函数
    int n = read(); 
    long long sum = 0; // 总和可能很大，使用 long long
    for (int i = 0; i < n; ++i) {
        int a = read(); // 循环读入每个数字
        sum += a;
    }
    // 使用我们自己的快速写出函数输出结果
    write(sum);
    putchar('\n'); 
    return 0;
}
```

****

## `fread `的核心思想

>   **尽可能减少与操作系统内核进行交互的次数（即系统调用**

`fread(buffer, size, count, stream); `

>   这个函数会尝试从输入流（如 stdin）中一次性读取 count 个大小为 size 的数据块，并把它们塞进你提供的一块巨大内存 buffer 中。

之后，你的程序就**不再需要频繁地向操作系统请求输入**了，而是直接从你自己的 buffer 这块内存里通过指针移动来“读取”字符。

只有当你自己的` buffer `被读完后，才需要再次调用` fread `来填充它。

****

**唯一的缺点:实现复杂度太高,必须手动管理所有缓冲区的细节**

>   1.需要巨大的全局缓冲区
>
>   2.需要指针来追踪位置
>
>   3.需要手动填充和重新填充缓冲区
>
>   4.处理EOF文件结尾变得复杂

```cpp
// 一种避免冲突,逻辑组织和封装的方法
namespace FastIO {
    const int BUF_SIZE = 1 << 20; // 定义一个 1MB 的缓冲区
    char buf[BUF_SIZE], *p_in = buf, *p_end = buf;

    // 我们需要自己写一个 get_char() 来代替 getchar()
    char get_char() {
        // 如果当前指针等于结束指针，说明缓冲区空了
        if (p_in == p_end) {
            // 调用 fread 重新填充缓冲区
            p_end = buf + fread(buf, 1, BUF_SIZE, stdin);
            p_in = buf; // 重置起始指针
            // 如果填充后两个指针还是一样，说明已经读到文件末尾
            if (p_in == p_end) return EOF;
        }
        // 返回当前指针指向的字符，并将指针后移一位
        return *p_in++;
    }

    // 然后，你的 read() 函数要调用上面那个复杂的 get_char()
    inline int read() {
        int x = 0, f = 1;
        char ch = get_char(); // 使用我们自己写的 get_char
        // ... 后面的逻辑和 getchar 版本一样 ...
        while (!isdigit(ch)) {
            if (ch == '-') f = -1;
            ch = get_char();
        }
        while (isdigit(ch)) {
            x = (x << 3) + (x << 1) + (ch - '0');
            ch = get_char();
        }
        return x * f;
    }
}
```

