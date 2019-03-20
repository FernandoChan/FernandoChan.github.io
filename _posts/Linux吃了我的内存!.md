![Linux ate my RAM!!](https://www.linuxatemyram.com/atemyram.png)

<center>
<span style="font-size: 64px; font-weight: bold; color: #F00000;">莫方!<br />不用担心你的内存!</span>
</center>

# 发生了什么?

Linux借用了你应用程序暂时不用的内存用来进行磁盘缓存, 这让你的系统看起来只有很少的内存, 但实际上并不是! 完全不用担心你的内存。 



# 为什么Linux要用缓存? 

磁盘缓存技术让你的系统变得更快更流畅, 除了让小白感到困惑以外这个技术没有任何的坏处。它也从来没有从你的应用那里偷走任何的内存， 从来没有！

# 那如果我想要运行更多应用程序怎么办？

如果你的应用程序需要使用更多的内存， 它们就直接找缓存拿回被借走的那块内存。任何时候缓存都能立即把占用的内存空间还回去。所以你不用担心会用完内存

# 还会进行交换吗？

不， 磁盘缓存只是借用了应用程序暂时不用的RAM，而不会用到交换技术。如果应用程序需要更多内存，只要从缓存那里拿回来就行了。

# 怎么让Linux停下来？ 

你不能关掉缓存。任何人如果想要关掉磁盘缓存， 那一定是因为他认为这样会偷走属于应用程序的内存，而这实际上并不会发生！缓存让应用程序加载更快而且运行更流畅，它永远也不会偷走他们的内存空间。因此，绝对没有任何理由关掉缓存！

# 既然内存并没有用完，为什么`top`和`free`要这么说？

这只是因为术语上的差别。你和Linux都认为应用程序拿走的那部分内存是被`"used"`的，而完全没有被任何程序占用的内存叫做`"free"`的。

但是你要怎么称呼那些目前被某些东西临时占用，但对于应用程序来说依然可用的内存？

你可能称之为空闲或者可用，也就是"free"或者"available"， 但是Linux管这叫"used but also available"，即被占用但依然有效。

| 内存实际上的状态       | 你管他叫             | Linux管他叫     |
| ---------------------- | -------------------- | --------------- |
| 被应用程序使用了       | used                 | used            |
| 被使用了但仍然有效     | free (或者available) | used(available) |
| 完全没有被任何程序使用 | free                 | free            |

前面提到的某些东西， 可能就是`top`和`free`称作**缓冲区**或者**缓存**的东西。由于你和Linux对这个术语的叫法不同， 你可能会误会你的内存很低。



# 怎么查看我还剩多少空闲的内存？ 

如果要看剩余的内存数量，你可以运行`free -m`命令，查看其中的`available`列的数值。

```shell
$ free -m
       total        used        free      shared  buff/cache   available
Mem:    1504        1491          13           0         855      792
Swap:   2047           6        2041
```

<p style="font-size: 75%">
（对于2016年以前的版本，对应的看"-/+ buffers/cache"这一行“free”列的数值）
</p>

这就是你系统上以MB为单位的空闲内存数量。如果你只是看“used”和“free”这两列的话，就可能天真地以为你的内存不是$47\% $而是高达$99\%$ ！

更多关于Linux对于“available”的细节和技术描述，参考[the commit that added the field](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=34e431b0ae398fc54ea69ff85ec700722c9da773).


# 那么, 什么时候你应该开始担心呢? 

一个有充裕内存的**健康的Linux系统**在运行一段时间后, 会表现出以下的特征, 当然这些特征都不会对你的电脑造成损害:

- `free`的内存接近于0 
- `used`的内存接近于全部
- 有充足的`available` 内存(比如说, 超过总内存的20%)
- `swap used`几乎保持不变

如果你留意到以下的信号, 那么就要开始警惕了:

- `available` 内存(或者 "free + buffers/cache") 接近于0
- `swap used` 增加或者波动较大
- `dmesg | grep oom-killer` 命令显示运行了OutOfMemory-killer





## 如何验证上面说的东西? 

有关更多详细信息以及如何查看磁盘缓存以显示上文描述的效果，请参阅此[这个页面](https://www.linuxatemyram.com/play.html)。 没有什么比在自己的电脑上亲自见证磁盘缓存带来的指数级的加速能更直观地让你感受到缓存技术的伟大之处了！







---

本文翻译自[LinuxAteMyRam.com](https://www.linuxatemyram.com/), 由[VidarHolen.net](http://www.vidarholen.net/) 呈现, 你也可以在[我们的GitHub](https://github.com/koalaman/linuxatemyram.com) 上**评论**或者`Pull requests`