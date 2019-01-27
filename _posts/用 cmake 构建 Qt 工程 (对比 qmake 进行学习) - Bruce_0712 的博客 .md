> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://blog.csdn.net/Bruce_0712/article/details/53574170 <link rel="stylesheet" href="https://csdnimg.cn/release/phoenix/template/css/ck_htmledit_views-f76675cdea.css">

## <a></a>cmake vs qmake

*   qmake 是为 Qt 量身打造的，使用起来非常方便
*   cmake 使用上不如 qmake 简单直接，但复杂换来的是强大的功能
    *   内置的 out-of source 构建。（目前 QtCreator 为 qmake 也默认启用了该功能。参考：[浅谈 qmake 之 shadow build](http://blog.csdn.net/dbzhang800/archive/2011/04/23/6343838.aspx)）

    *   为各种平台和场景提供条件编译
    *   可处理多个可执行文件情况，和很好配合 QtTest 工作

如何选择?

[Using CMake to Build Qt Projects](http://developer.qt.nokia.com/quarterly/view/using_cmake_to_build_qt_projects) 一文中说：

*   对简单的 Qt 工程，采用 qmake
*   对复杂度超过 qmake 处理能力的，采用 cmake

尽管如此，如果简单 Qt 的工程都不知道怎么用 cmake 构建，复杂的工程，就更不知道如何使用 cmake 了。还是从简单的学起吧

## <a></a><a></a>简单的 Qt 程序

<pre dir="ltr">#include <QtCore/QCoreApplication>
#include <QtCore/QDebug>
int main(int argc, char** argv)
{
    QCoreApplication app(argc, argv);
    qDebug()<<"hello qt!";
    app.exec();
}</pre>

如果不使用构建工具，直接调用编译器来编译的话，只需要类似这样的一条命令：

<pre>g++ main.cpp -Ie:\Qt\4.7.0\include -o main -Le:\Qt\4.7.0\lib -lQtCore4</pre>

指定头文件目录，以及需要链接的库

### <a></a><a></a>qmake

qmake 需要一个 .pro 文件:

<pre>CONFIG += qt
QT -= gui
SOURCES += main.cpp</pre>

*   因为我们需要 Qt 的库和头文件，所以需要<tt> CONFIG += qt </tt>。

    *   这是默认项，可直接去掉该行
*   启用 qt 后，可以通过 QT -= gui 来进一步细调我们需要的模块
    *   默认是 core gui。我们不需要 gui 模块，故去掉。

### <a></a><a></a>cmake

cmake 需要一个 CMakeLists.txt 文件：

<pre dir="ltr">PROJECT(example)
FIND_PACKAGE(Qt4 REQUIRED)
SET(QT_DONT_USE_QTGUI TRUE)
INCLUDE(${QT_USE_FILE})
ADD_EXECUTABLE(example main.cpp)
TARGET_LINK_LIBRARIES(example ${QT_LIBRARIES})</pre>

*   FIND_PACKAGE 来启用 Qt4
*   默认使用了 core 和 gui，故手动禁用 QTGUI
    *   这两行可以直接使用 <tt>FIND_PACKAGE(Qt4 COMPONENTS QtCore REQUIRED)</tt>，未指定的模块将被禁用

*   包含一个 CMake 为 Qt 提供的配置文件，${QT_USE_FILE} 变量是一个文件名
*   添加可执行程序目标
*   链接到 Qt 的库

## <a></a><a></a>复杂一点

考虑一个常规 Qt 程序：

*   main.cpp
*   mainwindows.ui
*   mainwindows.h
*   mainwindows.cpp

如果手动编译的话：

*   mainwindow.ui 需要使用 uic 预处理

<pre>uic mainwindow.ui -o ui_mainwindow.h</pre>

*   mainwindow.h 需要 moc 预处理

<pre>moc mainwindow.h -o moc_mainwindow.cpp</pre>

*   调用编译器进行编译

<pre>g++ main.cpp mainwindow.cpp  moc_mainwindow.cpp -Ie:\Qt\4.7.0\include -o main  -Le:\Qt\4.7.0\lib -lQtCore4 -lQtGui4</pre>

### <a></a><a></a>qmake

使用 qmake 的的话，一个简单的 pro 文件

<pre>TARGET = example
TEMPLATE = app
SOURCES += main.cpp mainwindow.cpp
HEADERS  += mainwindow.h
FORMS    += mainwindow.ui</pre>

HEADERS 中的文件是否需要 moc 进行预处理，qmake 运行时会根据其是否含有 Q_OBJECT 自动判断。

这也是为什么 很多人添加 Q_OBJECT 宏后不重新运行 qmake 会出错误的原因。

### <a></a><a></a>cmake

看看相应的 cmake 的 CMakeLists.txt 文件

<pre dir="ltr">PROJECT(example)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
FIND_PACKAGE(Qt4 REQUIRED)
INCLUDE(${QT_USE_FILE})
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})
QT4_WRAP_CPP(example_MOCS mainwindow.h)
QT4_WRAP_UI(example_UIS mainwindow.ui)
ADD_EXECUTABLE(example main.cpp mainwindow.cpp ${example_MOCS})
TARGET_LINK_LIBRARIES(example ${QT_LIBRARIES})</pre>

*   需要 moc 的文件，用 QT4_WRAP_CPP 处理
    *   生成的文件放入变量 example_MOCS 中，最后一块链接到可执行程序
*   需要 uic 的文件，用 QT4_WRAP_UI 处理

### <a></a><a></a>Windows

因为 windows 下链接时分为 console 和 windows 两个子系统，所以 windows 下有些问题需要特殊处理。

用 qmake 时：

*   默认是 windows 子系统
*   可以通过 CONFIG += console 使用 console 子系统

用 cmake 是：

*   默认是 console 子系统
*   使用 windows 子系统需要

<pre>SET(QT_USE_QTMAIN TRUE)
ADD_EXECUTABLE(example WIN32 main.cpp mainwindow.cpp ${example_MOCS})</pre>

前者启用 qtmain.lib 库来提供 windows 下的 WinMain 入口函数。后者链接 windows 子系统

## <a></a><a></a>再复杂一点

*   main.cpp
*   mainwindows.ui
*   mainwindows.h
*   mainwindows.cpp
*   main.qrc
*   main.rc

前面已经用到了 Qt 的 moc 和 uic，这次增加了资源系统 需要用 rcc

<pre>rcc main.qrc -o qrc_main.cpp</pre>

同时，使用了 windows 下的资源文件 .rc (比如给程序添加图标)

*   MVSC 中使用 rc.exe 对 .rc 文件进行处理
*   MinGW 中使用 windres.exe 处理 .rc 文件

### <a></a><a></a>qmake

<pre>TARGET = example
TEMPLATE = lib
HEADERS = mainwindow.h widget.h
SOURCES = main.cpp widget.cpp  mainwindow.cpp
RESOURCES = main.qrc
RC_FILE = main.rc</pre>

### <a></a><a></a>cmake

<pre dir="ltr">PROJECT(example)
CMAKE_MINIMUM_REQUIRED(VERSION 2.6)
FIND_PACKAGE(Qt4 REQUIRED)
SET(QT_USE_QTMAIN TRUE)
INCLUDE(${QT_USE_FILE})
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_BINARY_DIR})

if(MINGW)
  set(CMAKE_RC_COMPILER_INIT windres)
  ENABLE_LANGUAGE(RC)
  SET(CMAKE_RC_COMPILE_OBJECT
    "<CMAKE_RC_COMPILER> <FLAGS> -O coff <DEFINES> -i <SOURCE> -o <OBJECT>")
endif(MINGW)

SET(example_SRCS main.cpp mainwindow.cpp widget.cpp res/main.rc)
SET(example_MOC_SRCS mainwindow.h widget.h)
QT4_WRAP_CPP(example_MOCS ${example_MOC_SRCS})
QT4_ADD_RESOURCES(example_RCC_SRCS main.qrc)
SET(example_SRCS ${example_SRCS} ${example_MOCS} ${example_RCC_SRCS})

ADD_EXECUTABLE(example WIN32 main.cpp mainwindow.cpp ${example_SRCS})
TARGET_LINK_LIBRARIES(example ${QT_LIBRARIES})</pre>

*   对 Qt 的资源文件，使用 QT4_ADD_RESOURCES 来调用 rcc 进行预处理
*   对 Windows 资源文件，直接和源文件一样，添加到列表中即可。只是：
    *   MinGW 下仅仅这么做还不行，上面的 MinGW 块用来修复这个问题

## <a></a><a></a>Debug 与 Release

### <a></a><a></a>qmake

使用 qmake 时，可以在 pro 文件内分别为两种模式设置不同的选项。

使用时，可以直接 make release 或 make debug 来编译不同的版本

### <a></a><a></a>cmake

不同于 qmake，由于 cmake 采用 out-of-source 方式。故：

*   建立 debug release 两目录，分别在其中执行 cmake -DCMAKE_BUILD_TYPE=Debug（或 Release）
*   需要编译不同版本时进入不同目录执行 make

对生成 msvc 工程的情况, CMAKE_BUILD_TYPE 不起作用。生成工程后使用 IDE 自带的模式选择。

## <a></a><a></a>参考

*   [http://developer.qt.nokia.com/quarterly/view/using_cmake_to_build_qt_projects](http://developer.qt.nokia.com/quarterly/view/using_cmake_to_build_qt_projects)

*   [http://www.cmake.org/cmake/help/cmake-2-8-docs.html](http://www.cmake.org/cmake/help/cmake-2-8-docs.html)

*   [http://www.qtcentre.org/wiki/index.php?title=Compiling_Qt4_apps_with_CMake](http://www.qtcentre.org/wiki/index.php?title=Compiling_Qt4_apps_with_CMake)

*   [http://stackoverflow.com/questions/3526794/how-do-i-build-a-win32-app-with-a-resource-file-using-cmake-and-mingw](http://stackoverflow.com/questions/3526794/how-do-i-build-a-win32-app-with-a-resource-file-using-cmake-and-mingw)

*   [http://stackoverflow.com/questions/1372105/cmake-variable-or-property-to-discern-betwen-debug-and-release-builds](http://stackoverflow.com/questions/1372105/cmake-variable-or-property-to-discern-betwen-debug-and-release-builds)