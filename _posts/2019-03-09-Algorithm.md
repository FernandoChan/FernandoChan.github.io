---
layout: post
title: "面试算法之符号表到红黑树"
subtitle: ""
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - 红黑树
  - ST符号表
  - BST二分查找树
typora-copy-images-to: ..\img
---









# 红黑树

二三树的基础上解决了平衡的问题



典型的一个红黑树: 

其定义的充分条件为:

- 所有节点都为红色或者黑色
- 根节点和叶节点(空链接nil)都是黑色的
- **红色**节点的所有子节点都是**黑色**的
- 从一个节点到他所有的空链接的子节点的路径包含有同样数量的黑节点
  - 根据这个定义, 可以分别定义这个树的height 和 black-height 

![red-black tree](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img\1552128768318.png)

同时有以下的性质:

- 最短路径$P_s​$ 全是黑色

- 最长路径$P_l​$  红黑相间

- $P_l <= 2* P_s$ 

  证明如下: 设$black_height = N$ , 即所有路径黑节点数量为$N$个, 显然$最短路径为P_s = N$ , 即整条路径都没有红色节点

- 

红黑树的操作: 

红黑树也可以做搜索, 插入, 和删除的操作, 其中插入和删除, 需要进行旋转

- 空间复杂度$O(N)$ 因为不需要额外的空间.
- 