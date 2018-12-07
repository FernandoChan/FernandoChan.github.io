滤波是图像处理很重要的一部分，主要分为空间滤波和频域滤波，此节主要说明空间滤波。空间滤波可以分为线性空间滤波和非线性空间滤波。所用函数包括：**imfliter, colfit, padarray, fspecial, ordfilt2, medfilt2。**

# 滤波函数

## 线性空间滤波

线性空间滤波是对区域的像素做先行处理，将领域内的每个像素与对应的系数相乘。其中所用到的矩阵，是滤波器或者说是掩模。线性空间滤波主要有两种方式，一是相关，另一个是卷积。二者的区别是滤波器是否发生反转，介绍可以参阅信号。

### imfilter

*   函数语法：imfilter:

    ​    功能：对任意类型数组或多维图像进行滤波

    ​    用法：g = imfilter(f, w, filtering_mode, boundary_options, size_options)

    ​     其中，f 为输入图像，w 为滤波掩模，g 为滤波后图像。filtering_mode 用于指定在滤波

     过程中是使用 “相关” 还是“卷积”。

    ​    boundary_options 用于处理边界充零问题，边界的大小由滤波器的大小确定。具体参数

     选项见下表：

      

     

    ![img](https://img-blog.csdn.net/20170829200046548?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvd2VpeGluXzM2MzQwOTQ3/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

    *   注意：显示图像类型问题，可能会出现显示发白问题。
        [http://blog.csdn.net/linxid/article/details/78145516](http://blog.csdn.net/linxid/article/details/78145516)
*   代码示例：

```
img1 = imread('key2.jpg');
imshow(img1);
img2 = rgb2gray(img1);
imshow(img2);
w = [0 1 0;1 -4 1;0 1 0];
%默认的边界选项，滤波
gd = imfilter(img2,w);
figure,imshow(gd,[ ]);
%通过复制边界进行填充
gr = imfilter(img2,w,'replicate');
figure,imshow(gr,[]);
%通过边界对称填充
gs = imfilter(img2,w,'symmetric');
figure,imshow(gs,[ ]);
%包括循环边界...

%注意图像类型
```

## <a></a>非线性空间滤波

线性空间滤波是基于计算像素的乘积之和，是一个线性运算。而非线性空间滤波，是对像素做函数运算。两个常用的非线性滤波函数是 colfilt 和 nlfilter。由于 colfilt 执行起来比 nlfilter 快得多，所以我们多采用 colfilt。

### <a></a>colfilt

*   函数语法：
    `g = colfilt(f, [m n], 'sliding', @fun, parameters);`

    *   f：输入图像
    *   [m n]：滤波区域的维数
    *   ‘sliding’: 滤波器逐个像素滑动
    *   ‘@fun’：‘@’称为函数句柄，fun 为处理函数
    *   ‘parameters’：函数可能用到的参数

colfiltu 不同于 imfilter，填充方式没有确定，进行滤波之前，我们需要自行对图像进行填充，这里我们使用 padarray 函数。
除此之外此函数很重要的一部分是定义所用到的函数，首先要定义非线性函数。

### <a></a>padarray

*   函数语法：
    `fp = padarray(f, [ r c ], method, direction);`

    *   f，fp：输出和填充后的图像
    *   [r c]：填充 f 的行数和列数，是填充的数目，不是填充后矩阵的数目
    *   method 和 direction：

# <a></a>空间滤波器

MATLAB 的图像工具箱可以自动生成一些规定的滤波器。

## <a></a>线性空间滤波器

### <a></a>fspecial

该函数可以生成工具箱预定义的二维线性空间滤波器。

*   函数语法：
    `w = fspecial('type', parameters);`

‘type’为滤波器类型，‘parameters’为定义参数，详细情况如下：

![](http://upload-images.jianshu.io/upload_images/665202-8f2475daa29b8a97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
经典的一个滤波器是拉普拉斯滤波器，定义算子和通用的近似数字表达式，可查阅信号课本。
**1.** 对图像做卷积操作可实现此变换，掩模如下所示：
![](http://upload-images.jianshu.io/upload_images/665202-dada8788c5c8d03f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**2.** 另一种定义考虑对角线元素，掩模是：
![](http://upload-images.jianshu.io/upload_images/665202-0654739d98c84d65.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
**3.** fspecial 实现一个更为常见的拉普拉斯算子掩模：
![](http://upload-images.jianshu.io/upload_images/665202-b7fa64ca76184dc3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
fspecial 仅仅是为了生成一个滤波器，滤波器同样可以手动设置。我们在使用拉普拉斯算子处理后，可以**用原图减去操作后的结果，还原失去的灰度的色调**。通过变换后我们可以得到更清晰的图像。

*   **代码示例：**

```
img1 = imread('night.jpg');
%定义两个不同的滤波方法
w4 = fspecial('laplacian',0);
w8 = [1 1 1;1 -8 1;1 1 1];
%进行类型转换，保证显示图像更好
img1 = im2double(img1);
%拉普拉斯滤波后的图像
img_f1 = imfilter(img1,w4,'replicate');
img_f2 = imfilter(img1,w8,'replicate');
%进行图像相减
img_out1 = img1 - img_f1;
img_out2 = img1 - img_f2;
imshow(img1);
figure,imshow(img_out1);
figure,imshow(img_out2);
```

## <a></a>非线性空间滤波器

用于生成非线性空间滤波的一个工具是函数 ordfilt2，可以生成统计排序的滤波器，而这些都是非线性滤波器。

*   函数语法：
    `g = ordfilt2(f, order, domain);`
    f 为输入图像，函数用一组元素中第 order 个元素来代替 f 中的**每一个元素**。domain 来指定领域，是一个 m*n 的矩阵。对领域内的元素进行排序后，再代替。常用此类滤波器：
    **1\. 最小滤波器：**
    `g = ordfilt2(f,1,ones(m, n));`
    ones(m,n) 用于创建矩阵。
    **2\. 最大滤波器：**
    `g = ordfilt2(f,m*n,ones(m,n));`
    **3\. 中值滤波器：**
    `g = ordfilt2(f,median(1:m*n),ones(m,n));`
    median(1:m*n) 是指 1 到 m *n 的中值。median 的通用语法为：
    `v = median(A,dim);`
    v 是一个向量，是 A 矩阵沿行（或列）的中值所构成的向量。dim = 1，取相应列的中值，dim = 2，取相应行的中值。
    中值滤波是一种非常重要的非线性滤波器。是降低图像椒盐噪声的一种有效工具。
*   **medfilt2**：
    工具箱所提供的一个专门的二维中值滤波函数。
    `g = medfilt2(f,[m n],padopt);`
    [m n] 定义领域矩阵，padopt 指定边界填充选项。
    **中值滤波前后对比效果：**

![](http://upload-images.jianshu.io/upload_images/665202-f35dd0544a1f60fe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/665202-9aadb2b306d8ba4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

<link href="https://csdnimg.cn/release/phoenix/mdeditor/markdown_views-a47e74522c.css" rel="stylesheet">