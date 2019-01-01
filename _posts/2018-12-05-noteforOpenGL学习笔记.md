---
layout: post
title: "计算机图形学"
subtitle: "noteForOpenGL项目式学习"
author: "Fernando"
header-img: "img/post-bg-universe.jpg"
header-mask: 0.2
tags:
  - 图形学

typora-copy-images-to: ..\img
---







## 复习记忆内容



### 1.Graphics Application :

1. Video Games. Since more and more realistic games are developed , player can now enjoy playing amazing video games.
2. Scientific Visualization:  graphically illustrate scientific data to enable scientists to understand, illustrate, and glean insight from their data
3. CAD: Computer Aided Design. Computers play a role in production or display of the artwork. Thanks to the rapid development of computer graphics , designer can render amazing graphics on computer.

### 2.Raster graphics:

> How to generate a line using raster?
>
> 1. A line is represented by assigning some pixels a value of 1
> 2. The entire line is specified by the pixel values.

### 3.OpenGL basic conventions, Coordinate system, Geometric Primitives

**Why use OpenGL ?**

It's well supported and simpler than DirectX.

**Conventions :**

Variables written in CAPITAL letters • Example: GLUT_SINGLE, GLUT_RGB • usually constants • use the bitwise or command (x | y) to combine constants



**Why we need transformation?** 

to transform the vertex from the real world to the computer window

**How to Manipulate Matrix Stack?**

use transformation matrix to avoid repeatedly reloading the same sequence of transformations. GL use a special stack to maintain the matrix.

`glpushMatrix() `

`glpopMatrix()`

**Write a rectangle in OpenGL???**

```
glBegin(mode) and glEnd( ) delimit an object mode can be one of the following: 

• GL_POINTS   点
• GL_LINES 
• GL_POLYGON 实心的多边形
• GL_LINE_STRIP 线段
• GL_TRIANGLE_STRIP 
• GL_TRIANGLES 
• GL_LINE_LOOP(poly line )
• GL_QUADS 
• GL_QUAD_STRIP
• GL_TRIANGLE_FAN

glPushMatrix();

glColor3f(1.0,.8,.6)
glBegin(GL_TRIANGLES)
	glVertex2i()
	glVertex2i()
	glVertex2i()
	
	glVertex3fv()
	
	
	
	
	int mode = GLUT_RGB|GLUT_SINGLE; 
	glutInitDisplayMode(mode); glutInitWindowSize(500,500); glutCreateWindow(“Simple”); init();
glutDisplayFunc(display); glutKeyboardFunc(key); glutMainLoop();
```





### 4.2D Modeling Transformations, Matrix Representation,Homogeneous Coordinates

​    What is the major transformations??? And what is their Matrix Representation

- scale 
- rotate
- translate
- shear

Q4：What is the Matrix Representation for the 3D Transformations?



Q5: How to realize the transformations for a given geometrical primitives (line, plane, curve etc. )

transformations will be applied by all points of the primitive with the same transformation matrix. 

Q6: What is the effect of Affine transformation and projective transformation? 

Properties of projective transformations: 

1. Origin does not necessarily map to origin

    • Lines map to lines 

2. Parallel lines do not necessarily remain parallel 

3. Ratios are not preserved

    • Closed under composition


### 5.Rendering3D Scenes Pipeline, 3D Scene Representation, Euclidean Spaces, 3D Geometric Primitives

Input :  3D Model Primitives description  

output : image 

rendering pipeline有哪些步骤？以direction illuminate说明各个step

1. Modeling transformation : the models are transformed from local coordinate 3D world coordinate system. 
2. Lighting : calculate direct illumination on each primitive and object.
3. Viewing transformation :  Transform into 3D camera coordinate system 
4. Projection transformation : Transform into 2D screen space
5. Clipping : clip the view outside the view window. 
6. Rasterization ; draw pixels on screen .including texturing , hidden surface using depth buffer


### Triangle Rasterization, Edge Equations

Describe Edge Equation and given the code (What is the key idea of Edge Equation)

Raster Graphics Algorithm: 

为何: Scan conversion of a primitive 每次的操作都是以最小图元作为单位的

是何 : 连续到离散 , sampling problem

如何: 

1. Find a finite point set which approximates the primitive optimally
2. Converse a continuous primitive to the set of discrete


   7


Edge walking算法以及终止条件, 还有缺点

stop when bottom vertex or edge is reached



**Edge Equation** 

Define `Ax+by+c= e` for each edge `e`  //定义线公式

this line equation define 2 half-space for e>0 and e<0 //定义半平面

A triangle can be defined as the intersection of three positive half-spaces:  // 通过平面交集来定义图形

if simply turn on **those pixels** for which **all edge equations  > 0**:    //打开所有满足条件的pixel

```c++
findBoundingBox(&xmin, &xmax, &ymin, &ymax); 
setupEdges (&a0,&b0,&c0,&a1,&b1,&c1,&a2,&b2,&c2); /* Optimize this: */
for (int y = yMin; y <= yMax; y++) { 
	for (int x = xMin; x <= xMax; x++) { 
	float e0 = a0*x + b0*y + c0; 
	float e1 = a1*x + b1*y + c1; 
	float e2 = a2*x + b2*y + c2; 
	if (e0 > 0 && e1 > 0 && e2 > 0) setPixel(x,y);
}}
```







### 7:   Clipping concepts, Cohen-Sutherland Line Clipping

What is Clipping? 

Calculating the portions of primitives inside the view window.

Trivial Accepts:

if both endpoints are inside all edges of the view window. 

If both endpoints on the wrong side of same edge 

otherwise, 

how to give the outing code : 

**Assign each region with a 4-bit out code.** 

```c++
int ComputeOutCode(float x, float y)
{ int code =0; if y > Ymax then code = 8 else if y < Ymin code = 4 if x > Xmax code = code + 2; else if x < Xmin = code + 1; return code;
}
```

Clipping 算法:

```
assign an out code to each endpoint.

if both outcode = 0000 (bitwise OR operation), trivial accept.  1
else bitwise AND operation on both           2
	if result!=0, trivial reject, drop this line.     3 
	else split line segments, repeat 1       4
```

 





CH8:RGB color model

CH9:Basic concepts, Light Source Models, Ambient Light , Surface Reflectance, Phong lighting model

### CH10:Basic concepts, Global Illumination, Algorithm of ray tracing



**ray tracing 算法, 终止条件是什么?** 

1. cast a ray R  from View point to pixel p
2. for each , calculate the visible intersection P among ray R and the scene . 
3. for each intersection . calculate its color with phong model. Lc
4. cast a ray Rr from intersection P to the direction of **reflectance**, and a ray Rt from P the direction of **refraction**
5. recursively calculate Lr and Lt 
6. combine the color as p = Lr+Lt+Lc

**Termination conditions:**

1. when the ray does not intersect with any object in the scene, or ???\
2. when the contribution of the ray is small enough , or its attenuation is strong enough
3. when the recursive depth is over the threshold.



### C H11:Three common shading

**                ****Questions discussion on class ******

** **

**Q1****：****What isBarycentric coordinates? Why we use it? How to calculate Barycentriccoordinates?**

Alocal coordinates system which 

Apoint can be defined as (t1, t2, t3) corresponding to its position with respecttriangle vertices.

 

 

Becausewe can associate any additional information or data (points, color, vectors,etc.) to each one of its vertices in the triangle

** **

**Q2****：****What is shading?And what are the main shading models? What are the main difference betweenthem?**

Shadingis illumination rendering;

3common shading model:

1.    Flatshading , compute the phong lighting only once for each entire primitive

2.    GouroudShading , compute the phong lighting at the vertices, and interpolate the lighting value across polygons.

3.    PhongShading ,perform phong lighting for each pixels, and interpolate the normal across polygons,

      Themajor difference is perform computing on how many points and their interpolationmethod.

** **

**Q3****：****What is texture?Why we need texture mapping? How to realize the texture mapping?**

Textureis the image painted on polygons.

Texturemapping can help producing more realistic scenes. To enhance the visualeffect of a scene

 

Weuse texel , Texture map is an image, two-dimensional array of color values(texels)

**Project texel (u,v)texture coordinates to polygon’s (s,t)space.**

** **

**Q4****：****What is aliasing?How to achieve antialiasing? **

** **

** Aliasing : **Approximation of lines and circleswith discrete points often gives a staircase appearance, it will produce primtivesthat are not smooth looking****

**Anti-aliasing technique:like super sampling or Area sampling **






>  **1、OpenGL渲染管线！！！！！！**
>
>  **2、物体-世界-相机-裁剪-视口 坐标变换！！！！！**

>  先实现一遍底层的光栅化的3D渲染算法，只用一个画像素的功能，把插值，zbuffer，alpha，阴影，三角形的光栅化，全自己底层实现一遍，之后进阶挑nvidia 的sdk里的例子逐句看，siggraph早期的gpgpu的论文挑自己觉得有趣的去实现，shaderX，gpu gems里头拿感兴趣的实现
>
>  

目标: 

- 了解缓存对象 VAO 和 VBO
- GLSL 着色器程序的编译、链接和使用方法
- OpenGL 绘图的基本流程

![img](https://img-blog.csdn.net/20160504205508618)绘图流水线pipeline, 关注其中的Vertex Shader 和Fragment Shader

Vertex Shader 和Fragment Shader的绘图流程: 

(1) 用户在程序中指定或者加载vertex属性数据 
(2) 将顶点属性数据传送到 GPU，由Vertex Shader处理顶点数据 
(3) 由Fragment Shader负责最终图形的颜色 

The main purpose of a **vertex shader** is to transform points (x, y, and z coordinates) into different points

The main purpose of a **fragment shader** is to calculate the color of each pixel that is drawn.

## VBO 和 VAO

在 OpenGL 程序中指定或者加载的data是存储在 CPU 中的，要加快图形渲染需要将`data`发送到 GPU 中

### VBO 

在 GPU 中，VBO 即 `vertex buffer object`, 顶点缓存对象，负责实际数据的存储

### VAO 

即 `vertex array object` 记录数据的存储和如何使用的细节信息

### VAO 和 VBO 的关系

VAOs are the link between the VBOs and the shader variables. VAOs describe what *type* of data is contained within a VBO, and which *shader variables* the data should be sent to



OpenGL 是一个状态机 (state machine)，我们绘制图形时需要在不同的状态之间切换。例如上一节中通过 `glClearColor` 设置清除颜色缓冲区时设定的颜色，OpenGL 则记住了这一状态，当调用 glClear 时则使用这个颜色重置颜色缓冲区。直到再次使用 glClearColor 设置不同颜色为止，OpenGL 会一直使用这个状态值



## What are shaders? 

Shaders are little programs, made from GLSL code, that run on the **GPU** instead of the **CPU**



















## OpenGL  

OpenGL 做什么: 

* put things in the scene (points, colored lines , textured poly) 
* describe the camera 相机的位置/方向/`fov`视野
* listen for keyboard events 
* render: draw the scene with: 
  * Geometric primitives
  * Bitmaps and images (texturing把几何图元与image联系起来)

OpenGL用什么数学工具: 

> 矩阵, matrix

**MODELVIEW matrix:** 同一个矩阵既用于坐标轴转换又用于物体的变换, 所以这个矩阵作用相当于Modeling和Viewing两个transformation. ` Certain model transformations are shared by many models`. 

* ​

**PROJECTION_MATRIX**

![1544059742664](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544059742664.png)







### 为什么有这么多变换? 

![transformation](https://img-blog.csdn.net/20140918202127843)

 这个图可以帮助理解, 变换的结果就是: to transform the vertices from the real world to the computer screen.

我也想用Unity来解释:

>  一个我们想要渲染一个画面, 首先要①通过缩放平移和旋转摆放物体, 确定物体在世界中的相对位置(model  transformation), 在世界坐标系下,② 要把相机摆在一个正确的位置才能拍到物体(viewing projection), ③投影(project transformation) 就是进行裁剪, 将视锥(cone)以外的部分clip掉减少计算量, ④进行透视除法来保证物体的大小比例真实. ⑤将以上的画面投影到2d的电脑屏幕上





## 4 Transformations 

### 4.1 Modeling Transformation: 坐标系变换+坐标变换以及他们的矩阵表示 

#### 坐标变换

我们要求可以自由地移动物体, 这些操作包括:

* scale 按比例缩放
* translate 平移
* rotate 旋转

#### 坐标系变换(参考[官方教程](https://open.gl/transformations))

如果用相机来类比计算机, 在渲染中第一步就是要确定我们物体在世界中的位置:

> Modeling Transformation := Local coordinate => World Coordinate

#### Scale

Scaling Operation : 矩阵形式: 相应坐标乘倍数, 简单
$$
\left[

 \begin{matrix}

   x' \\

   y' 
  \end{matrix}

  \right] \tag{1} =  
  
  \left[

 \begin{matrix}

   ax \\

   by 
  \end{matrix}

  \right]
$$


#### Rotation(以3D vertex为例)

绕着y轴旋转:  
$$
\begin{bmatrix}
   x' \\
   y' \\
   z' \\
   w' \\
\end{bmatrix} = \begin{bmatrix}
   \cos\phi & 0 &-\sin\phi&0 \\ 0&1&0&0\\ \sin\phi & 0 & \cos\phi&0 \\ 0&0&0&1
\end{bmatrix}\begin{bmatrix}
   x \\
   y \\
   z \\
   w \\
\end{bmatrix}
  
  \tag{2}
$$
绕着x轴旋转: 
$$
\begin{bmatrix}
   x' \\
   y' \\
   z' \\
   w' \\
\end{bmatrix} = \begin{bmatrix}
  1&0&0&0 \\
  0& \cos\phi & -\sin\phi & 0 \\
  0& \sin\phi & \cos\phi &0 \\
   0 &0 & 0 &1 \\
\end{bmatrix}\begin{bmatrix}
   x \\
   y \\
   z \\
   w \\
\end{bmatrix}
  
  \tag{3}
$$
绕着z轴旋转: 
$$
\begin{bmatrix}
   x' \\
   y' \\
   z' \\
   w' \\
\end{bmatrix} = \begin{bmatrix}
   cos\phi & -sn\phi & 0&0 \\
   sin\phi & cos\phi &0&0\\
   0&0&1&0 \\
   0 &0 & 0 &1 \\
\end{bmatrix}\begin{bmatrix}
   x \\
   y \\
   z \\
   w \\
\end{bmatrix}
  
  \tag{4}
$$

### Translation

$$
\begin{bmatrix}
   x' \\
   y' \\
   z' \\
   w' \\
\end{bmatrix} = \begin{bmatrix}
  1&0&0& t_x\\
  0& 1 &0& t_y\\
  0& 0& 1& t_z \\
   0 &0 & 0 &1 \\
\end{bmatrix}\begin{bmatrix}
   x \\
   y \\
   z \\
   w \\
\end{bmatrix}
  
  \tag{5}
$$
### Mirror 

$$
\begin{bmatrix}
   x' \\
   y' \\
   z' \\
   w' \\
\end{bmatrix} = \begin{bmatrix}
  -1&0&0& 0\\
  0& 1 &0& 0\\
  0& 0& 1& 0 \\
   0 &0 & 0 &1 \\
\end{bmatrix}\begin{bmatrix}
   x \\
   y \\
   z \\
   w \\
\end{bmatrix}

  \tag{6}
$$

### 仿射变换和投影变换矩阵

scale , mirror , 和 rotate 的组合被称为 linear transformation, 性质是:

- Origin maps to origin
- • Lines map to lines
- • Parallel lines remain parallel 
- •Ratios are preserved 
- Closed under composition

但是Translate属于非线性, 原因是经过translate的原点不再对应.



绕着任意点旋转: 运用Matrix composition: 将这个点平移到原点上, 绕着原点旋转, 再做平移的逆变换

![1544442780128](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544442780128.png)





### Homogeneous 齐次坐标系





代码: 

```c++
// 在调用任何的变换函数之前, 首先要调用以下两句, 这样接下来的所有变换函数将影响的是model变换矩阵
// 也说明一件事就是, 我们不需要额外用变量来存储模型变换和投影变换的矩阵
glMatrixMode(GL_MODELVIEW);
glLoadIdentity();
```





### 4.2 Projection

两种投影: Perspective 和 Orthogonal 

透视投影: 点光源照射在一个物体上投射到一个墙壁上的影子, 近大远小. 图中的右边的绿色是

![1544067473019](C:\Users\ADMINI~1\AppData\Local\Temp\1544067473019.png){:height="50%" width="50%"}![透视投影-w150](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544067178381.png){:height="50%" width="50%"}

 

正交投影: 平行光源

![正交投影](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544067168888.png)

```c++
//
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluPerspective(vfov, aspect,
               near,far);
// vfov
// aspect是这个截面的宽度除以高度, 如果是正方形区域, aspect=1.0
// 
```







## 5 Geometric Primitives & Operations , 渲染管线





## 6 Rasterization





## 7 Clipping

Analytically calculating the portions of primitives within the view window







## 8 Coloring



## 9 Local(direct) Illumination.

 OpenGL的光照模型把光分成4种独立的成分, 环境光+散射光+镜面光+发射光

### Emission

发射光种类: Point + Directional + spot

### Ambient: 

经过多次反射已经无法分清入射光源. OpenGL的处理方式是当做一个发射光源处理. 这个光源在物体表面的Scattering方式是**均匀发散**

环境光是场景中光源给定或者全局给定的一个光照常量，它一般很小，主要是为了模拟即使场景中没有光照时，也不是全部黑屏的效果。场景中总有一点环境光，不至于使场景全部黑暗，例如远处的月亮，远处的光源。 
环境光的实现为：

```c++
// 环境光成分
float   ambientStrength = 0.1f;
vec3    ambient = ambientStrength * lightColor * objectColor;
```

### Lambert's law & Snell's law 散射光+镜面光

#### 漫反射`Lambert 's law` 

漫反射Diffuse reflection
$$
R_s(\theta,\phi ,\gamma,\psi,\lambda)
$$

>  入射角` angle of incident light `$( \theta,\phi)$ 出射角`angle of leaving light ` ($\gamma,\psi$)  波长$\lambda$

当$\theta = 90$ 时物体表面与光线方向平行，此时光线照射不到物体，光的强度最弱；当$θ>90$后，物体的表面转向到光线的背面，此时物体对应的表面接受不到光照。入射角度如下图所示： 

![img](https://img-blog.csdn.net/20160611220411265)







初中物理的入射角和出射角余弦定律
$$
I_{\rm dffuse} = k_d I_{\rm light} \cos \theta =  k_d I_{\rm light} （\vec n \cdot \vec l)
$$

#### 镜面反射高光`Snell's law`: 

高光与观察者的角度相关, 图中只有在R角度才能看得到高光, 而观察者在其他角度都看不到. 当 R 和 V 的夹角`θ`越小时，人眼观察到的镜面光成分越明显。

![img](https://img-blog.csdn.net/20160611220215053)

![img](https://img-blog.csdn.net/20160612224338214) 

`s` 越大, 可以看到高光的角度范围越狭窄

镜面光的计算公式: $I{\rm spec} = k_s *I {\rm in} * specFactor$

镜面反射系数`specFactor` 定义为: $specFactor=\cos(\theta )^ s $ ,`s`表示为镜面高光系数（shininess ）, a purely empirical constant that varies the rate of **falloff** , $\theta $ 为观察者和反射光线之间的夹角.



### Lighting Model

phong模型![img](https://img-blog.csdn.net/20160611214522357)

在不同**角度**和不同**高光系数**下phong模型反射率: ![1544160644052](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544160644052.png)

多个光源下的phong模型等式:![1544161328826](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544161328826.png)

phong模型的OpenGL代码:

```c++
vec3    specular = specularStrength * specFactor * lightColor * objectColor; //通过颜色来反应物体的表面反射Material surface reflectance

//材料性质,以(face , pname , params)的向量形式
void glMaterialfv( GLenum face, GLenum pname, const GLfloat * params);
face: Specifies which face or faces are being updated. Must be one of 
GL_FRONT, GL_BACK, or GL_FRONT_AND_BACK

pname: Specifies the material parameter of the face or faces that is being updated. Must be one of GL_AMBIENT, GL_DIFFUSE, GL_SPECULAR, GL_EMISSION, GL_SHININESS, GL_AMBIENT_AND_DIFFUSE, or GL_COLOR_INDEXES. 
  
Params: parameter values.
  
 
// 法线向量
void glNormalfv(normal[]);


// 光线向量
void glLightfv( GLenum light, GLenum pname, GLfloat *params ); 
light: Number ID of light

pname: 参数名, 必须是其中之一:GL_AMBIENT, GL_DIFFUSE, GL_SPECULAR, GL_POSITION GL_SPOT_DIRECTION,
GL_SPOT_EXPONENT GL_SPOT_CUTOFF
衰减 GL_CONSTANT_ATTENUATION GL_LINEAR_ATTENUATION GL_QUADRATIC_ATTENUATION


// 上述三种光成分叠加后，成为最终物体的颜色，片元着色器中实现为：
vec3 result = ambient + diffuse + specular 
color = vec4(result , 1.0f);
```



`Snell's law`出射角=入射角
$$
\theta {\rm in} = \theta {\rm out}
$$


## 10 Global Illumination

- Shadows
- Refractions
- Inter-Object reflections






## 11 Shading & Sampling 







## 线性代数: 计算机图形学向(来自[王定桥的专栏向量矩阵要点](https://blog.csdn.net/wangdingqiaoit/article/details/51383052#commentBox), [线性代数入门（计算机图形学向）](https://zhuanlan.zhihu.com/p/41951467))



### 自由度的概念

在矩阵里面, 有多少个变量, 就有几个自由度, 自由度越高, 矩阵的效果也就负责越多的功能



**注意转换矩阵应用顺序** 当用矩阵A,B,C转换向量v时，

如果v用行向量记法，则矩阵按转换顺序从           左往右列出，表达为vABC;

如果v采用列向量记法，则转换矩阵应该放在左边，并且转换从右往左发生，对应的转换记为CBAv。





课程复习点: 

lCH1:  Computer graphics applications

lCH2: Raster Graphics

lCH3:  OpenGL basic convections,Coordinate system, Geometric Primitives

lCH4:  2D Modeling Transformations, MatrixRepresentation, Homogeneous Coordinates

l CH5: Rendering3D Scenes Pipeline, 3D Scene Representation, Euclidean Spaces, 3D GeometricPrimitives

lCH6: **Triangle Rasterization, Edge Equations**

lCH7:   Clipping concepts, **Cohen-Sutherland Line Clipping**

lCH8: RGB color model

lCH9: Basic concepts, Light Source Models, Ambient Light , Surface Reflectance, Phong lighting model

lCH10: Basic concepts, Global Illumination, **Algorithm of ray Tracing**

lCH11: Three common shading



## 问题

```
问答题
1.图形学的应用有？举三个例子
2.rendering pipeline有哪些步骤？以direction illuminate说明各个step
3.basic 2d transformation有哪些？给出rotation和scaling的2*2 matrix
4.direction illumination的light source有哪几种？分别解释一下。
5.shading 方式有哪几种？分别解释一下。
6.怎样表示3d 线段，3d 射线，3d 直线
7.反射有几种？分别解释一下

算法题:
p1.edge equation用于traingle rasterization的key idea是什么？给出key code。（第二问真是万万妹想到，完全没看。。）

p2.cohen-s clipping的区域outcode是怎么给出的？写出clipping算法。
p3.用openGL写（给出顶点的坐标） a）一个矩形 b）一个矩形还有一条对角线
（1,0）               （1,1）         （1,0）                   （1,1）          






（0,0）                （0,1)          （0,0）                  （0,1)
p4.写出ray tracing的算法。算法终止条件是什么？



简答题

Q1：What is Barycentric coordinates? Why we use it? How to calculate Barycentric coordinates?
A local coordinates system which we usually used for shading


Q2：What is shading? And what are the main shading models? What are the main difference between them?
Shading is illumination rendering;
3 common shading model:
1.	Flat shading , the simplest one, only calculate illumination for a single point in each polygon
2.	 

Q3：What is texture? Why we need texture mapping? How to realize the texture mapping?



Q4：What is aliasing? How to achieve antialiasing? 

 
```

![1544191783464](H:\OneDrive\Project(sync with Github)\FernandoChan.github.io\img/1544191783464.png)





