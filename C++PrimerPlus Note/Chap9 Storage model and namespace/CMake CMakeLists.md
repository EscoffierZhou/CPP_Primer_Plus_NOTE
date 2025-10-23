# CMake CMakeLists

简单来说，CMake 是一个开源的、跨平台的自动化构建系统。**它本身不直接编译您的代码，而是根据一个名为 CMakeLists.txt 的配置文件**，生成特定编译器（如` GCC, Clang, MSVC`）和平台（如 `Windows, Linux, macOS`）所需的构建文件（例如` Makefile `或` Visual Studio `的项目文件）。

**CMakeLists.txt 就是 CMake 的“菜谱”，您需要在这个文件中告诉 CMake 您的项目包含了哪些源文件、需要链接哪些库、头文件的路径在哪里等等。**

>```cmake
># 指定构建该项目所需的 CMake 最低版本
>cmake_minimum_required(VERSION 3.28) 
>
># 定义项目的名称。这会创建一个名为 my_project_name 的变量，
># 可以在后续的命令中通过 ${PROJECT_NAME} 来引用它。
>project(my_project_name)
>
># 告诉编译器我们希望使用 C++17 标准来编译项目。
># 可以根据需要修改为 11, 14, 20 或 23
>set(CMAKE_CXX_STANDARD 17)
>
># 用于生成一个可执行文件.out
>add_executable(my_project_name main.cpp)
>```

**对于以下文件结构**

```ABAP
my_project/
├── CMakeLists.txt
├── src/
│   ├── main.cpp
│   └── namesp.cpp
└── include/
    └── my_namespace/
        └── namesp.h
```

```cmake
cmake_minimum_required(VERSION 3.28)
project(my_project_name)

set(CMAKE_CXX_STANDARD 17)

# 添加可执行文件，并指定其源文件
add_executable(my_project_name src/main.cpp src/namesp.cpp)

# 为指定的目标（我们的可执行文件）添加头文件搜索路径
target_include_directories(my_project_name PRIVATE include)
```

