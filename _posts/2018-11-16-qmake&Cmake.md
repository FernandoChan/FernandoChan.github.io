---
layout: post
title: "make, makefile, cmake, qmake 的区分"
subtitle: ""
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - make
  - 编译原理

typora-copy-images-to: ..\img
---

转载自[make ；makefile； cmake； qmake的区分](https://www.cnblogs.com/Jessica-jie/p/7689182.html)

# 大型编程项目的构建原理

`.c`被C编译器编译成一个目标文件`.o` 含有目标机器的二进制代码, C中没有类似于Java字节代码的东西.

1. Preprocessor C预处理器

   对头文件扩展宏, 处理条件编译并将结果传递给编译器. 

2. Compiler 编译器

3. Linker链接器

   将所有`.o`文件组合成单个可以执行的二进制文件

make程序读入Makefile. 

> Make的作用是在构建OS二进制码时检查需要哪个目标文件, .c 或者.h头文件如果已经有修改的话就把这些目标文件重新编译, 

![1546427477451](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1546427477451.png)



## make ；makefile； cmake； qmake 的区分

1. **make 是用来执行 Makefile 的**。
2. Makefile 是类 unix 环境下 (比如 Linux) 的类似于批处理的 "脚本" 文件。其基本语法是: **目标 + 依赖 + 命令**，只有在**目标**文件不存在，或**目标**比**依赖**的文件更旧，**命令**才会被执行。由此可见，Makefile 和 make 可适用于任意工作，不限于编程。
3. **Makefile+make 可理解为类 unix 环境下的项目管理工具**，但它太基础了，抽象程度不高，而且在 windows 下不太友好 (针对 visual studio 用户)，于是就有了跨平台项目管理工具 cmake
4. cmake 是跨平台项目管理工具，它用更抽象的语法来组织项目。虽然，仍然是目标，依赖之类的东西，但更为抽象和友好，比如你可用 math 表示数学库，而不需要再具体指定到底是 math.dll 还是 libmath.so，在 windows 下它会支持生成 visual studio 的工程，在 linux 下它会生成 Makefile，甚至它还能生成 eclipse 工程文件。也就是说，从同一个抽象规则出发，它为各个编译器定制工程文件。
5. **cmake 是抽象层次更高的项目管理工具**，**cmake 命令执行的 CMakeLists.txt 文件。**
6. **qmake 是 Qt 专用的项目管理工具**，对应的工程文件是 *.pro，在 Linux 下面它也会生成 Makefile，当然，在命令行下才会需要手动执行 qmake，完全可以在 qtcreator 这个专用的 IDE 下面打开 *.pro 文件，使用 qmake 命令的繁琐细节不用你管了。

### 总结
总结一下，make 用来执行 Makefile，cmake 用来执行 CMakeLists.txt，**qmake 用来处理 \*.pro 工程文件**。Makefile 的抽象层次最低，cmake 和 qmake 在 Linux 等环境下最后还是会生成一个 Makefile。
cmake 和 qmake 支持跨平台，**cmake 的做法是生成指定编译器的工程文件，而 qmake 完全自成体系。**

#### 怎么选用
具体使用时，
Linux 下，小工程可手动写 Makefile，大工程用 automake 来帮你生成 Makefile，要想跨平台，就用 cmake。如果 GUI 用了 Qt，也可以用 qmake+*.pro 来管理工程，这也是跨平台的。当然，cmake 中也有针对 Qt 的一些规则，并代替 qmake 帮你将 qt 相关的命令整理好了。另外，需要指出的是，make 和 cmake 主要命令只有一条，make 用于处理 Makefile，cmake 用来转译 CMakeLists.txt，而 qmake 是一个体系，用于支撑一个编程环境，它还包含除 qmake 之外的其它多条命令 (比如 uic，rcc,moc)。



> 上个简图，其中 cl 表示 visual studio 的编译器，gcc 表示 linux 下的编译器

![img](https://images2017.cnblogs.com/blog/1034872/201710/1034872-20171018213217037-2085649960.png)



## CMake笔记

教程来自华师潘师兄的[CMake实战](http://www.hahack.com/codes/cmake/)



## 安装

`sudo apt install cmake`

### 包含子目录

根目录中的 CMakeLists.txt ：

```
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)

# 项目信息
project (Demo3)

# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_SRCS 变量
aux_source_directory(. DIR_SRCS)

# 添加 math 子目录
add_subdirectory(math)

# 指定生成目标 
add_executable(Demo main.cc)

# 添加链接库
target_link_libraries(Demo MathFunctions)
```

该文件添加了下面的内容: 第3行，使用命令 `add_subdirectory` 指明本项目包含一个子目录 math，这样 math 目录下的 CMakeLists.txt 文件和源代码也会被处理 。第6行，使用命令 `target_link_libraries` 指明可执行文件 main 需要连接一个名为 MathFunctions 的链接库

子目录中的 CMakeLists.txt：

```
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
aux_source_directory(. DIR_LIB_SRCS)

# 生成链接库
add_library (MathFunctions ${DIR_LIB_SRCS})
```

在该文件中使用命令 `add_library` 将 src 目录中的源文件编译为静态链接库。 





```shell
fernando@WIN10-608190930:~/cmake-demo/Demo3$ cmake .
-- The C compiler identification is GNU 7.3.0
-- The CXX compiler identification is GNU 7.3.0
-- Check for working C compiler: /usr/bin/cc
^@^@-- Check for working C compiler: /usr/bin/cc -- works
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Detecting C compile features
-- Detecting C compile features - done
-- Check for working CXX compiler: /usr/bin/c++
-- Check for working CXX compiler: /usr/bin/c++ -- works
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Configuring done
-- Generating done
-- Build files have been written to: /home/fernando/cmake-demo/Demo3
fernando@WIN10-608190930:~/cmake-demo/Demo3$ ls
CMakeCache.txt  CMakeFiles  CMakeLists.txt  Makefile  cmake_install.cmake  main.cc  math
fernando@WIN10-608190930:~/cmake-demo/Demo3$ make
Scanning dependencies of target MathFunctions
[ 25%] Building CXX object math/CMakeFiles/MathFunctions.dir/MathFunctions.cc.o
[ 50%] Linking CXX static library libMathFunctions.a
[ 50%] Built target MathFunctions
Scanning dependencies of target Demo
[ 75%] Building CXX object CMakeFiles/Demo.dir/main.cc.o
[100%] Linking CXX executable Demo
[100%] Built target Demo
fernando@WIN10-608190930:~/cmake-demo/Demo3$ ls
CMakeCache.txt  CMakeFiles  CMakeLists.txt  Demo  Makefile  cmake_install.cmake  main.cc  math
```

也就是说`cmake .`生成下面 的`Makefile `文件

`CMakeCache.txt  CMakeFiles  CMakeLists.txt  Makefile  cmake_install.cmake  main.cc  math`

`make` 编译生成了`Demo ` 这个名字是由`CMakeLists.txt  `这个文件里面的`add_executable(Demo main.cc)`决定的

 