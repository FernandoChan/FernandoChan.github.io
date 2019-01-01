---
layout: post
title: ""
subtitle: ""
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - 
typora-copy-images-to: ..\img
---



Multiprogramming的目的

除了三个基本的running blocked waiting 状态以外的进程状态

5 

7 模型: 加入挂起suspend



进程的异常Error: 内部机制

中断: 外部故障: 比如系统断电等



**(5 分) 下列各种事件中，一定产生进程状态改变的事件是**

 A、 运行的进程正常退出

 B、 运行的进程因种种原因而阻塞



 D、 阻塞的进程被唤醒

 E、 运行的进程时间片用完



为什么引入线程? 

1. 应用的需要
2. 开销
3. 性能

线程是处理器调度的基本单位

进程是CPU资源调度的基本单位





**) 进程运行时，其硬件状态保存在相应寄存器中；当它被切换下 CPU 时，其硬件状态保存在PCB中而不是内核栈。**











# Chapter4 File system

物理介质: 物理block (cluster)

![1544661453666](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544661453666.png)

尽管有很多header, 但是只有一个磁头处于活动状态, 为什么? 

输入输出的数据格式: bit string .

sectors 组成: title +data + ECC 

![1544661983387](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544661983387.png)

访盘地址: 

读写操作, 磁盘地址(device , cylinder ,m header, sector 鸽 的编号, 内存地址(源和目录





### 9.4  文件控制块及文件目录

FCB: 文件管理二设置的数据结构, 保存管理文件所需要的所有有关信息

![1544669814858](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544669854836.png)

FAT

![1544670097116](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544670097116.png)

i-node 和FAT的区别:

i-node 是顺序表示(一个文件的所有连续物理块都列在索引表里面)  每个文件都有一个i-node

FAT是链接表示, 里面存着所有文件的链接

索引表放在FCB可以不可以? 

- i-node不等长  
- 在FCB中派生出一个字段, 专门存放索引表的位置
- ![1544670471021](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544670471021.png)

i-node 的优化: 直接和间接索引



![1544670685376](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544670685376.png)UNIX的三级索引结构

为什么是256个物理块?

主索引表存在FCB里面一般比较小, 只有30个字节, 然而存放在物理块里面的索引表就比较大,有512字节,  512/2 = 256 个索引项

​    

空闲快管理相关的数据结构: 存放在

1. bitmap 位图
2. 空闲快表: 空闲起始块号 + 块数
3. 空闲块链表(也是书中的方法) : 把所有的空闲块连接成一个链, 扩展: 成组链接法

![1544668706892](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544668706892.png)

# Chapter5 Input Output

keyword: double interleaving, cylinder skew, track-to-track seek, 

disk: heads, cylinders, sector,  track skew , data transfer rate 

clock interrupt handler, .





### Cylinder Skew:





### Disk Interleaving

![1544662140777](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544662140777.png)

### Disk Arm Scheduling Algorithms

#### FIFO

#### SSF(Shortest Seek First最近优先)