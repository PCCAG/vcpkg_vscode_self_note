## 参考自官网, 详细情况请细看 [官网文档](https://learn.microsoft.com/zh-cn/vcpkg/)

## 安装vcpkg

* [vcpkg](https://github.com/microsoft/vcpkg)
* 配置path变量(可选)

## vscode CMAKE集成

**官方介绍:**

![1696502899743](image/readme/1696502899743.png)

**示例setting.json:**

```json
{
  "cmake.configureSettings": {
    "CMAKE_TOOLCHAIN_FILE": "D:/vcpkg/scripts/buildsystems/vcpkg.cmake"
  },
  "cmake.buildDirectory": "${workspaceFolder}/cmakebuild",
  "cmake.automaticReconfigure": false
}
```

**CMAKE 工具链可以在这里看到:**

![1696508308917](image/readme/1696508308917.png)

## 配置 vcpkg.json (可选)

在项目文件夹下创建vcpkg.json文件, 里面放项目的依赖.

```json
{
  "dependencies": ["cxxopts", "fmt", "range-v3"]
}
```

在vcpkg.json文件所在目录运行命令:   vcpkg install 就会安装vcpkg.json中的依赖库 (国内记得开代理)

会在文件所在目录生成 vcpkg_installed文件夹, 目录结构如下 :

![1696510337592](image/readme/1696510337592.png)

## CMAKE 配置 CMakeLists.txt

**官方的示例生成代码main.cxx:**

```cpp
#include <cxxopts.hpp>
#include <fmt/format.h>
#include <range/v3/view.hpp>

namespace view = ranges::views;

int fib(int x)
{
    int a = 0, b = 1;

    for (int it : view::repeat(0) | view::take(x))
    {
        (void)it;
        int tmp = a;
        a += b;
        b = tmp;
    }

    return a;
}

int main(int argc, char **argv)
{
    cxxopts::Options options("fibo", "Print the fibonacci sequence up to a value 'n'");
    options.add_options()("n,value", "The value to print to", cxxopts::value<int>()->default_value("10"));

    auto result = options.parse(argc, argv);
    auto n = result["value"].as<int>();

    for (int x : view::iota(1) | view::take(n))
    {
        fmt::print("fib({}) = {}\n", x, fib(x));
    }
}
```

**CMakeLists.txt例子:**

```cmake
cmake_minimum_required(VERSION 3.20)
# cmake最低版本为3.20
project(fibonacci CXX)
#项目名 CXX表示C++项目

find_package(fmt REQUIRED) 
find_package(range-v3 REQUIRED)
find_package(cxxopts REQUIRED)

#find_package(库名 关键词)
#REQUIRED是find_package()命令的一个可选参数，用于指定所查找的库是否是必需的。
#如果指定了REQUIRED，但是CMake在系统中找不到该库，则会发生错误并停止构建过程。
#如果没有指定REQUIRED，则CMake会在找不到该库时发出警告，但不会停止构建过程。



set(CMAKE_CXX_STANDARD 23)
#设置C++标准为C++23

add_executable(fibo main.cxx)
#定义生成的可执行文件名为 fibo 源码来自 main.cxx

target_link_libraries(fibo
    PRIVATE
    fmt::fmt
    range-v3::range-v3
    cxxopts::cxxopts)
# 将fibo目标链接到 fmt、range-v3 和 cxxopts 库
# PRIVATE 关键字表示这些库仅在 fibo 目标中可见，而不会传播到依赖于 fibo 的其他目标

```

## CMAKE build

* 点底部bar选择生成编译工具 , 推荐选VS的  (😅, 选mingw运行fmt的例子失败, 不知道什么鬼原因)

![1696509072829](image/readme/1696509072829.png)

![1696509052405](image/readme/1696509052405.png)

* 选择生成目标:

![1696509172757](image/readme/1696509172757.png)

* 等待生成

![1696509186385](image/readme/1696509186385.png)

* 运行

![1696509246340](image/readme/1696509246340.png)
