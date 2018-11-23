---
layout: post
title: "make, makefile, cmake, qmake 的区分"
subtitle: ""
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - 
---

转载自[make ；makefile； cmake； qmake的区分](https://www.cnblogs.com/Jessica-jie/p/7689182.html)



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