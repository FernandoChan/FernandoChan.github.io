> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 https://www.zhihu.com/question/51745620 ![](https://pic3.zhimg.com/v2-7c7dceddb511e11ec9daa49293b772dc_xs.jpg) 禹洋

* * *

**1.axes subplot axis**

先说第一个疑惑 **Axes - Subplot - Axis 之间到底是个什么关系？**

因为我是努力在看英文的教程，所以刚开始对 axes 和 axis 是基本搞不清的，一个是轴的复数，一个是轴，好像设定图像属性的时候经常用 axes，具体到某个坐标轴的时候才会用 axis。然后教程还说，subplot 和 axes 基本就是一个意思。真是坑坑坑。。。

扛不住，翻了翻中文教程，好像有的教程就直接把 axes 翻译成子图了，好像这个世界就压根没有 subplot 和 axes 的区别。。看了半天，其实我还是觉得 axes 翻译成轴域比较贴切，下面就结合后来看到的各种教程来讲讲自己最后的理解。

* * *

**1.1 先明确 Figure 的概念**

![](https://pic1.zhimg.com/v2-c84a4df8b5e2d33daab103ff31678bd0_b.png)![](https://pic1.zhimg.com/v2-c84a4df8b5e2d33daab103ff31678bd0_r.jpg)

```
import numpy as np 
import matplotlib.pyplot as plt 
fig = plt.figure() 
plt.show()

```

我们先理清 figure 的概念。用画板和画纸来做比喻的话，figure 就好像是画板，是画纸的载体，但是具体画画等操作是在画纸上完成的。在 pyplot 中，画纸的概念对应的就是 Axes/Subplot。

![](https://pic4.zhimg.com/v2-1378cb00b6c7f87cfb12d5e9cd013907_b.jpg)![](https://pic4.zhimg.com/v2-1378cb00b6c7f87cfb12d5e9cd013907_r.jpg)

```
fig = plt.figure() 
ax = fig.add_subplot(111) 
ax.set(xlim=[0.5, 4.5], ylim=[-2, 8], title='An Example Axes', ylabel='Y-Axis', xlabel='X-Axis') 
plt.show()

```

![](https://pic4.zhimg.com/v2-756e05613d08cba99f9c5a611934d8eb_b.png)![](https://pic4.zhimg.com/80/v2-756e05613d08cba99f9c5a611934d8eb_hd.png)

所以就算我们只有一个子图，我们也可以生成一个 subplot，然后来在对这个 subplot 对象进行各种轴、标注、刻度等的设定。

* * *

**1.2 Axes 和 Subplot 的概念上细微的区别**

```
fig = plt.figure() 
ax1 = fig.add_subplot(211) 
ax2 = fig.add_subplot(212)
 print type(ax1) 
plt.show()

```

![](https://pic1.zhimg.com/v2-e84ac240f77fe48f41baad2faf595a64_b.png)![](https://pic1.zhimg.com/80/v2-e84ac240f77fe48f41baad2faf595a64_hd.png)

第一个例子是用 subplot() 方法。

subplot() 方法很好理解。里面传入的三个数字，前两个数字代表要生成几行几列的子图矩阵，底单个数字代表选中的子图位置。这个例子中我们生成了 2 行 1 列的子图矩阵。可以分别在两个 subplot 中画图。

```
fig = plt.figure() 
ax3 = fig.add_axes([0.1, 0.1, 0.8, 0.8]) 
ax4 = fig.add_axes([0.72, 0.72, 0.16, 0.16]) 
print type(ax3) plt.show()

```

![](https://pic1.zhimg.com/v2-90169e46e2eb585cd4316751d161cd58_b.png)![](https://pic1.zhimg.com/80/v2-90169e46e2eb585cd4316751d161cd58_hd.png)

第二个例子是用 add_axes() 方法。

我觉得轴域（Axes）的感念确实可以先理解成一些轴（Axis）的集合，当然这个集合还有很多轴（Axis）的属性，标注等等。我们用 add_axes() 方法生成一个轴域（Axes），括号里面的值前两个是轴域原点坐标（从左下角计算的），后两个是显示坐标轴的长度。当我们生成了轴域的时候，从结果上看确实是生成了一个可以画图的子图。我们可以分别在两个轴域（Axes）中画图。

对比两种方法，两种对象，我们可以总结总结：

*   两种对象确实是 “你中有我，我中有你” 的关系，生成子图（subplot）的时候，必然带着所谓的一套轴域（Axes）。而用轴域（Axes）方法，客观上就是生成了一个可以画图的子图。
*   add_subplot() 方法在生成子图过程，简单明了，而用 add_axes() 方法, 则生成子图的灵活性更强，完全可以实现 add_subplot() 方法的功能，可以控制子图显示位置，甚至实现相互重叠的效果。例如：

![](https://pic2.zhimg.com/v2-6f42edb4faa2245373de0ca4624fa2a9_b.png)![](https://pic2.zhimg.com/v2-6f42edb4faa2245373de0ca4624fa2a9_r.jpg)

* * *

**2 Axes 方法与 pyplot 函数**

用野路子法，也就是直接看代码，不懂的就查文档，看别人的代码的时候，图像的的各种特性经常用两套方法实现，对学习过真是毁灭性打击。所以遇到模仿的瓶颈的时候，还是要找些教程看看。这里基本照搬翻译，[https://github.com/matplotlib/AnatomyOfMatplotlib](https://link.zhihu.com/?target=https%3A//github.com/matplotlib/AnatomyOfMatplotlib) 教程中的 Part1 的 Axes methods vs. pyplot 一节。

```
plt.plot([1, 2, 3, 4], [10, 20, 25, 30], color='lightblue', linewidth=3)
plt.xlim(0.5, 4.5) 
plt.show()

```

![](https://pic3.zhimg.com/v2-fad98b88ea3425105386537e12b75376_b.png)![](https://pic3.zhimg.com/80/v2-fad98b88ea3425105386537e12b75376_hd.png)

```
fig = plt.figure() 
ax = fig.add_subplot(111) 
print type(ax) 
ax.plot([1, 2, 3, 4], [10, 20, 25, 30], color='lightblue', linewidth=3) ax.set_xlim(0.5, 4.5) 
plt.show()

```

![](https://pic1.zhimg.com/v2-87c789af32004061b7f38e631a87f6cc_b.png)![](https://pic1.zhimg.com/80/v2-87c789af32004061b7f38e631a87f6cc_hd.png)

本次画图涉及到的两步操作，画图和设定 x 轴的显示范围，分别用前后两种方法实现。

第一种，调用了 pyplot 中的 plot() 函数和 xlim() 函数,

第二种，使用了生成的 Subplot 对象的两种方法 .plot 和 .set_xlim 方法。

实际上，实现整个画图过程可以用两套工具来分别实现，其实这也是贯穿整个 python 编程的两种思路，函数式编程和对象式编程。我们在这里可以比较一下两套工具的优缺点：

*   以 plot() 为代表的函数式操作，表达简洁，但是没有体现出真正画图的实现过程，例如甚至当没有搞清楚 Figure Axes Subplot 等概念的时候，依然可以轻松的用 pyplot 函数画图。当子图较多的时候，对子图的操作容易陷入混乱，因为从代码上并不能字节观察出到底在操作那张子图。
*   以 .plot 为代表的对象式操作，表达明确，分步生成 Figure 和 Axes/Subplot，操作过程直接可以看出是在那张子图上操作。但是缺点就是，需要写的代码比较多，不够简洁。

这里要吐槽一下我看的这个教程，作者提出了在 [PEP20](https://link.zhihu.com/?target=https%3A//www.python.org/dev/peps/pep-0020/https%3A//www.python.org/dev/peps/pep-0020/) 中，“Python 之道”（The Zen of Python）提到了 “明了胜于晦涩”（Explicit is better than implicit），所以作者在整个教程中都是使用了对象式的方法。但是其实”Python 之道 “的下一句就是 “简洁胜于复杂”（Complex is better than complicated）。

所以，还是看你的使用场景，假如不需要画子图的时候，用一用简单的 pyplot 方法也没什么不好。但是初学者最好还是能够坚持先使用 Axes 对象属性的方法，这样对于画图的实现过程可以加深理解。

* * *

* * *

参考资料：

*   [https://github.com/matplotlib/AnatomyOfMatplotlib](https://link.zhihu.com/?target=https%3A//github.com/matplotlib/AnatomyOfMatplotlib)github 上的教程
*   [https://www.datacamp.com/community/tutorials/matplotlib-tutorial-python#gs.alh6j1c](https://link.zhihu.com/?target=https%3A//www.datacamp.com/community/tutorials/matplotlib-tutorial-python%23gs.alh6j1c) Datacamp 中带运行环境的教程

![](https://pic4.zhimg.com/v2-ae618b0cf05b2a9b636a1d3ea37d3838_xs.jpg) wort[Python 科学计算数据可视化模块 - Matplotlib](https://link.zhihu.com/?target=http%3A//www.voidcn.com/blog/baibaibai66/article/p-5973503.html), 这篇博客好像讲到点
![](https://pic3.zhimg.com/v2-3c0d7b48041864e265f752989385c54a_b.png)![](https://pic3.zhimg.com/80/v2-3c0d7b48041864e265f752989385c54a_hd.png)![](https://pic4.zhimg.com/da8e974dc_xs.jpg)weiqun zou

axis 顾名思义就是轴。

axes 简单说来就是灵活的子图。和 Subplot 的关系看官网上 axes 的说明，很清楚了：

Most of you are probably familiar with the **Subplot, which is just a special case of an Axes** that lives on a regular rows by columns grid of Subplot instances. If you want to create an Axes at an arbitrary location, simply use the [add_axes()](https://link.zhihu.com/?target=http%3A//matplotlib.org/api/figure_api.html%23matplotlib.figure.Figure.add_axes) method which takes a list of [left, bottom, width, height] values in 0-1 relative figure coordinates:

附链接：[Artist tutorial - Matplotlib 2.0.2 documentation](https://link.zhihu.com/?target=http%3A//matplotlib.org/users/artists.html%23axes-container)

![](https://pic3.zhimg.com/v2-72c863f5a10b8ccf60cc25564edca7b2_xs.jpg)Gdicheics

第一已经回答得很清楚了，我也被这个恶心过，说一种更好理解的方式。

可以把 fig 想象成 windows 的桌面，你可以有好几个桌面。然后 axes 就是桌面上的图标，subplot 也是图标，他们的区别在：axes 是自由摆放的图标，甚至可以相互重叠，而 subplot 是 “自动对齐到网格”。

但他们本质上都是图标，也就是说 subplot 内部其实也是调用的 axes，只不过规范了各个 axes 的排列罢了。



难道不是 axis 单数 axes 复数吗。。。。。。