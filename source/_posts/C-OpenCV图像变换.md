---
title: C OpenCV图像变换
date: 2020-11-16 20:10:45
categories: OpenCV
tags:
mathjax: true
---

## 基于OpenCV的边缘检测

### 边缘检测的一般步骤

1. 滤波
- 边缘检测的算法主要是基于图像强度的一阶和二阶导数，但导数通常对噪声很敏感，因此必须采用滤波器来改善与噪声有关的边缘检测器的性能。常见的滤波方法主要有高斯滤波，即采用离散化的高斯函数产生一组归一化的高斯核，然后基于高斯核函数对图像灰度矩阵的每一点进行加权求和。

2. 增强
- 增强边缘的基础是确定图像各点邻域强度的变化值。增强算法可以将图像灰度点邻域强度值有显著变化的点凸显出来。

3. 检测
- 经过增强的图像往往邻域中有很多点的梯度值比较大，而在特定的应用中，这些点并不是要找的边缘点，所以应该采用某种方法来对这些点进行取舍，实际工程中，常用的方法是通过阈值化方法来检测。

### canny算子

#### canny边缘检测步骤

1. 消除噪声，首先使用高斯平滑滤波器卷积降噪，以下是一个高斯内核示例。

$$
K=\frac{1}{139}
\begin{bmatrix}
    2&4&5&4&2\\
    4&9&12&9&4\\
    5&12&15&12&5\\
    4&9&12&9&4\\
    2&4&5&4&2
\end{bmatrix}

$$

2. 计算梯度幅度和方向
3. 非极大值抑制，这一步排除非边缘像素，仅仅保留了一些细线条（候选边缘）
4. 滞后阈值 滞后阈值需要使用两个阈值（高阈值和低阈值）
   1. 若某一像素位置的幅值超过高阈值，该像素被保留为边缘像素。
   2. 若某一像素位置的幅值小于低阈值，该像素被排除
   3. 若某一像素的幅值在两个阈值之间，该像素仅仅在连接到一个高于髙阈值的像素时被保留。

**对于Canny函数的使用，推荐的高低阈值比在2:1到3:1之间**

### 相关API函数

``` C++

void Canny(InputArray image,OutputArray edges,double threshold1,double threshold2,int apertureSize =3,bool L2gradient=false)

```

第三个参数，阈值1
第四个参数，阈值2
第五个参数 apertureSize 表示应用Sobel算子的孔径大小。默认值为3
第六个参数，bool类型L2gradients 一个计算图像梯度幅值的标识，有默认值false

**需要注意的是，阈值1和阈值2两者中较小的用于边缘连接，而较大的用于控制强边缘的初始段**

### Sobel算子

sobel算子是一个主要用于边缘检测的离散微分算子，他结合了高斯平滑和微分求导，用来计算图像灰度函数的近似梯度，在图像的任何一点使用此算子，都将会产生对应的梯度矢量或是其法矢量。

### sobel算子的计算过程

1. 分别在x和y两个方向求导
    1. 水平变化：将图像I与一个奇数大小的内核G<sub>x</sub>进行卷积。比如：当内核大小为3时，G<sub>x</sub>的计算结果为
   $$
    G_x = \begin{bmatrix}
        -1&0&1\\
        -2&0&2\\
        -1&0&1
    \end{bmatrix} * I
   $$
    2. 垂直变化：将I与一个奇数大小的内核进行卷积。比如：当内核大小为3时，G<sub>y</sub>的计算结果为：
   $$
    G_y = \begin{bmatrix}
        -1&-2&-1\\
        0&0&0\\
        1&2&1
    \end{bmatrix} * I
   $$
2. 在图像的每一点，结合以上两个结果求出近似梯度：
   $$
    G=\sqrt{G_x^2 + G_y^2}
   $$
3. 使用sobel算子
``` C++
void Sobel(InputArray src,OutputArray dst,int ddepth,int dx,int dy,int ksize=3,double scale=1,double delta=0,int boardType=BORDER_DEFAULT)
```

ddepth 代表输出图像的深度，填-1即可

dx dy代表在x，y方向上的差分阶数

ksize表示核的大小。

scale表示计算导数值时的缩放因子，默认为1

delta表示结果存入目标图前，可选的增量值，默认为0

### Laplacian算子

同数学上的定义

$$
Laplacian(f) = \frac{\partial^2f}{\partial x^2}+\frac{\partial^2f}{\partial y^2}
$$

同时由于Laplacian使用了图像梯度，其内部代码实际上调用了Sobel算子。

**让一幅图像减去它的Laplacian算子可以增强对比度**

使用Laplacian算子
``` C++
void Laplacian(InputArray src,OutputArray dst,int ddepth,int ksize=3,double scale=1,double delta=0,int boardType=BORDER_DEFAULT)
```

### scharr滤波器

我们一般直接称scharr为滤波器而不是算子，它主要是配合Sobel算子的运算而存在的。因为当内核大小为3时，sobel算子可能会产生比较明显的误差。为解决这一问题，opencv提供了scharr函数，但该函数只作用于大小为3的内核，该函数结果更加精确。

内核如下：

$$
G_x = \begin{bmatrix}
        -3&0&3\\
        -10&0&10\\
        -3&0&3
    \end{bmatrix} * I
   $$

   $$
    G_y = \begin{bmatrix}
        -3&-10&-3\\
        0&0&0\\
        3&10&3
    \end{bmatrix} * I
   $$

使用scharr算子
``` C++
void Scharr(InputArray src,OutputArray dst,int ddepth,int dx,int dy,double scale=1,double delta=0,int boardType=BORDER_DEFAULT)
```

相比sobel，scharr仅少了ksize项。

## 霍夫变换

霍夫变换适用于识别图像中的几何形状。

### 霍夫变换概述

霍夫变换是图像处理中的一种特征提取技术，该过程在一个参数空间中通过计算累计结果的局部最大值得到一个符合该特定形状的几何作为霍夫变换结果。

### OpenCV中霍夫线变换

首先霍夫线变换的输入是边缘二值图像。
OpenCV中支持三种不同的霍夫线变换：
1. 标准霍夫变换SHT
2. 多尺度霍夫变换MSHT（多尺度下一个变种）
3. 累积概率霍夫变换PPHT（在一定范围内，进行霍夫变换，计算单独线段的方向和范围从而减少计算量。）

### 霍夫线变换的原理

一条直线在图像二维空间可以有两个变量表示：

1. 笛卡尔坐标系：斜率，截距
2. 极坐标系：极径，极角

对于霍夫变换，我们使用第二种方式来表示直线。

因此直线表达式为：
$$
y=(-\frac{cos\theta}{sin\theta})x+(\frac{r}{sin\theta})\\
r=xcos\theta+ysin\theta\\
其中r是原点到直线的距离，x，y为直线上一点。
对于点x_0,y_0可以将通过这个点的一族直线统一定义为\\
r_\theta=x_0cos\theta+y_0sin\theta
$$

对于一个给定点，我们在极坐标对极径极角平面绘出所有通过它的直线，将得到一条正弦曲线。

我们可以对图像中所有的点进行上述操作。如果两个不同点进行上述操作后得到的曲线相交，这就意味着它们通过同一条直线，这样来说，一条直线能够通过在平面寻找交于一点的曲线数量来检测，越多曲线交于一点也就意味着这个交点表示的直线由更多的点组成。一般来说我们可以通过设置直线上点的阈值来定义多少条曲线交于一点，这样才认为检测到了一条直线。

### 相关API函数

```C++
void HoughLines(InputArray image, OutputArray lines, double rho, double theta, int threshold, double srn=0, double stn=0 )
```

第一个参数，InputArray类型的image，输入图像，即源图像，需为8位的单通道二进制图像，可以将任意的源图载入进来后由函数修改成此格式后，再填在这里。

第二个参数，InputArray类型的lines，经过调用HoughLines函数后储存了霍夫线变换检测到线条的输出矢量。每一条线由具有两个元素的矢量表示，其中，rho是离坐标原点((0,0)（也就是图像的左上角）的距离。 theta是弧度线条旋转角度（0~垂直线，π/2~水平线）。

第三个参数，double类型的rho，以像素为单位的距离精度。另一种形容方式是直线搜索时的进步尺寸的单位半径。

第四个参数，double类型的theta，以弧度为单位的角度精度。另一种形容方式是直线搜索时的进步尺寸的单位角度。

第五个参数，int类型的threshold，累加平面的阈值参数，即识别某部分为图中的一条直线时它在累加平面中必须达到的值。大于阈值threshold的线段才可以被检测通过并返回到结果中。

第六个参数，double类型的srn，有默认值0。对于多尺度的霍夫变换，这是第三个参数进步尺寸rho的除数距离。粗略的累加器进步尺寸直接是第三个参数rho，而精确的累加器进步尺寸为rho/srn。

第七个参数，double类型的stn，有默认值0，对于多尺度霍夫变换，srn表示第四个参数进步尺寸的单位角度theta的除数距离。且如果srn和stn同时为0，就表示使用经典的霍夫变换。否则，这两个参数应该都为正数。


```C++
void HoughLinesP(InputArray image, OutputArray lines, double rho, double theta, int threshold, double minLineLength=0, double maxLineGap=0 )
```

第一个参数，InputArray类型的image，输入图像，即源图像，需为8位的单通道二进制图像，可以将任意的源图载入进来后由函数修改成此格式后，再填在这里。

第二个参数，InputArray类型的lines，经过调用HoughLinesP函数后后存储了检测到的线条的输出矢量，每一条线由具有四个元素的矢量(x_1,y_1, x_2, y_2）  表示，其中，(x_1, y_1)和(x_2, y_2) 是是每个检测到的线段的结束点。

第三个参数，double类型的rho，以像素为单位的距离精度。另一种形容方式是直线搜索时的进步尺寸的单位半径。

第四个参数，double类型的theta，以弧度为单位的角度精度。另一种形容方式是直线搜索时的进步尺寸的单位角度。

第五个参数，int类型的threshold，累加平面的阈值参数，即识别某部分为图中的一条直线时它在累加平面中必须达到的值。大于阈值threshold的线段才可以被检测通过并返回到结果中。

第六个参数，double类型的minLineLength，有默认值0，表示最低线段的长度，比这个设定参数短的线段就不能被显现出来。

第七个参数，double类型的maxLineGap，有默认值0，允许将同一行点与点之间连接起来的最大的距离。

### 霍夫圆变换

霍夫圆变换的基本原理和上面讲的霍夫线变化大体上是很类似的，只是点对应的二维极径极角空间被三维的圆心点x, y还有半径r空间取代。

在OpenCV中，我们常常通过霍夫梯度法来解决园变换问题。

### 霍夫梯度法

霍夫梯度法的原理是这样的。

1. 首先对图像应用边缘检测，比如用canny边缘检测。

2. 然后，对边缘图像中的每一个非零点，考虑其局部梯度，即用Sobel（）函数计算x和y方向的Sobel一阶导数得到梯度。

3. 利用得到的梯度，由斜率指定的直线上的每一个点都在累加器中被累加，这里的斜率是从一个指定的最小值到指定的最大值的距离。

4. 同时，标记边缘图像中每一个非0像素的位置。

5. 然后从二维累加器中这些点中选择候选的中心，这些中心都大于给定阈值并且大于其所有近邻。这些候选的中心按照累加值降序排列，以便于最支持像素的中心首先出现。

6. 接下来对每一个中心，考虑所有的非0像素。

7. 这些像素按照其与中心的距离排序。从到最大半径的最小距离算起，选择非0像素最支持的一条半径。
8. 如果一个中心收到边缘图像非0像素最充分的支持，并且到前期被选择的中心有足够的距离，那么它就会被保留下来。

### 相关API函数

``` C++
C++: void HoughCircles(InputArray image,OutputArray circles, int method, double dp, double minDist, double param1=100,double param2=100, int minRadius=0, int maxRadius=0 )
```

第一个参数，InputArray类型的image，输入图像，即源图像，需为8位的灰度单通道图像。

第二个参数，InputArray类型的circles，经过调用HoughCircles函数后此参数存储了检测到的圆的输出矢量，每个矢量由包含了3个元素的浮点矢量(x, y, radius)表示。

第三个参数，int类型的method，即使用的检测方法，目前OpenCV中就霍夫梯度法一种可以使用，它的标识符为CV_HOUGH_GRADIENT，在此参数处填这个标识符即可。

第四个参数，double类型的dp，用来检测圆心的累加器图像的分辨率于输入图像之比的倒数，且此参数允许创建一个比输入图像分辨率低的累加器。上述文字不好理解的话，来看例子吧。例如，如果dp= 1时，累加器和输入图像具有相同的分辨率。如果dp=2，累加器便有输入图像一半那么大的宽度和高度。

第五个参数，double类型的minDist，为霍夫变换检测到的圆的圆心之间的最小距离，即让我们的算法能明显区分的两个不同圆之间的最小距离。这个参数如果太小的话，多个相邻的圆可能被错误地检测成了一个重合的圆。反之，这个参数设置太大的话，某些圆就不能被检测出来了。

第六个参数，double类型的param1，有默认值100。它是第三个参数method设置的检测方法的对应的参数。对当前唯一的方法霍夫梯度法CV_HOUGH_GRADIENT，它表示传递给canny边缘检测算子的高阈值，而低阈值为高阈值的一半。

第七个参数，double类型的param2，也有默认值100。它是第三个参数method设置的检测方法的对应的参数。对当前唯一的方法霍夫梯度法CV_HOUGH_GRADIENT，它表示在检测阶段圆心的累加器阈值。它越小的话，就可以检测到更多根本不存在的圆，而它越大的话，能通过检测的圆就更加接近完美的圆形了。

第八个参数，int类型的minRadius,有默认值0，表示圆半径的最小值。

第九个参数，int类型的maxRadius,也有默认值0，表示圆半径的最大值。

## 重映射

重映射，就是把一幅图像中某位置的像素放置到另一个图片指定位置的过程。为了完成映射过程, 我们需要获得一些插值为非整数像素的坐标,因为源图像与目标图像的像素坐标不是一一对应的。一般情况下，我们通过重映射来表达每个像素的位置 (x,y)，像这样 :

g(x,y) = f ( h(x,y) )

在这里， g( ) 是目标图像, f() 是源图像, 而h(x,y) 是作用于 (x,y) 的映射方法函数。

### 相关API

``` C++
C++: void remap(InputArray src, OutputArray dst, InputArray map1, InputArray map2, int interpolation, intborderMode=BORDER_CONSTANT, const Scalar& borderValue=Scalar())
```

第一个参数，InputArray类型的src，输入图像，即源图像，填Mat类的对象即可，且需为单通道8位或者浮点型图像。

第二个参数，OutputArray类型的dst，函数调用后的运算结果存在这里，即这个参数用于存放函数调用后的输出结果，需和源图片有一样的尺寸和类型。

第三个参数，InputArray类型的map1，它有两种可能的表示对象。
表示点（x，y）的第一个映射。
表示CV_16SC2 , CV_32FC1 或CV_32FC2类型的X值。

第四个参数，InputArray类型的map2，同样，它也有两种可能的表示对象，而且他是根据map1来确定表示那种对象。若map1表示点（x，y）时。这个参数不代表任何值。表示CV_16UC1 , CV_32FC1类型的Y值（第二个值）。

第五个参数，int类型的interpolation,插值方式，之前的resize( )函数中有讲到，需要注意，resize( )函数中提到的INTER_AREA插值方式在这里是不支持的，所以可选的插值方式如下：
- INTER_NEAREST - 最近邻插值
- INTER_LINEAR – 双线性插值（默认值）
- INTER_CUBIC – 双三次样条插值（逾4×4像素邻域内的双三次插值）
- INTER_LANCZOS4 -Lanczos插值（逾8×8像素邻域的Lanczos插值）

第六个参数，int类型的borderMode，边界模式，有默认值BORDER_CONSTANT，表示目标图像中“离群点（outliers）”的像素值不会被此函数修改。

第七个参数，const Scalar&类型的borderValue，当有常数边界时使用的值，其有默认值Scalar( )，即默认值为0

## 仿射变换

仿射变换（Affine Transformation或 Affine Map），又称仿射映射，是指在几何中，一个向量空间进行一次线性变换并接上一个平移，变换为另一个向量空间的过程。它保持了二维图形的“平直性”（即：直线经过变换之后依然是直线）和“平行性”（即：二维图形之间的相对位置关系保持不变，平行线依然是平行线，且直线上点的位置顺序不变）。

一个任意的仿射变换都能表示为乘以一个矩阵(线性变换)接着再加上一个向量(平移)的形式。

那么, 我们能够用仿射变换来表示如下三种常见的变换形式：

旋转，rotation (线性变换)
平移，translation(向量加)
缩放，scale(线性变换)

### 仿射变换的求法

$$
A=\begin{bmatrix}
    a_{00}&a_{01}\\
    a_{10}&a_{11}
\end{bmatrix}\\

B=\begin{bmatrix}
    b_{00}\\
    b_{10}
\end{bmatrix}\\

M=\begin{bmatrix}
    A&B
\end{bmatrix}\\

T = A\cdot\begin{bmatrix}
    x\\y
\end{bmatrix}+B
=M\begin{bmatrix}
    x&y&1
\end{bmatrix}^T\\
\therefore
T = \begin{bmatrix}
    a_{00}x+a_{01}y+b_{00}\\
    a_{10}x+a_{11}y+b_{10}
\end{bmatrix}
$$

仿射变换表示的就是两幅图片之间的一种联系 . 关于这种联系的信息大致可从以下两种场景获得:

1. 已知 X和T，而且我们知道他们是有联系的. 接下来我们的工作就是求出矩阵 M
2. 已知 M和X，要想求得 T. 我们只要应用算式T=MX即可. 对于这种联系的信息可以用矩阵 M 清晰的表达 (即给出明确的2×3矩阵) 或者也可以用两幅图片点之间几何关系来表达。

### 相关API函数

``` C++
C++: void warpAffine(InputArray src,OutputArray dst, InputArray M, Size dsize, int flags=INTER_LINEAR, intborderMode=BORDER_CONSTANT, const Scalar& borderValue=Scalar())
```

第一个参数，InputArray类型的src，输入图像，即源图像，填Mat类的对象即可。

第二个参数，OutputArray类型的dst，函数调用后的运算结果存在这里，需和源图片有一样的尺寸和类型。

第三个参数，InputArray类型的M，2×3的变换矩阵。

第四个参数，Size类型的dsize，表示输出图像的尺寸。

第五个参数，int类型的flags，插值方法的标识符。此参数有默认值
- INTER_LINEAR(线性插值)，可选的插值方式如下：
- INTER_NEAREST - 最近邻插值
- INTER_LINEAR - 线性插值（默认值）
- INTER_AREA - 区域插值
- INTER_CUBIC –三次样条插值
- INTER_LANCZOS4 -Lanczos插值
- CV_WARP_FILL_OUTLIERS - 填充所有输出图像的象素。如果部分象素落在输入图像的边界外，那么它们的值设定为 fillval.
- CV_WARP_INVERSE_MAP –表示M为输出图像到输入图像的反变换，即 。因此可以直接用来做象素插值。否则, warpAffine函数从M矩阵得到反变换。

第六个参数，int类型的borderMode，边界像素模式，默认值为BORDER_CONSTANT。

第七个参数，const Scalar&类型的borderValue，在恒定的边界情况下取的值，默认值为Scalar()，即0。

``` C++
C++: Mat getRotationMatrix2D(Point2fcenter, double angle, double scale)
```

第一个参数，Point2f类型的center，表示源图像的旋转中心。

第二个参数，double类型的angle，旋转角度。角度为正值表示向逆时针旋转（坐标原点是左上角）。

第三个参数，double类型的scale，缩放系数。
 
## 直方图均值化

直方图均值化是通过拉伸像素强度分布范围来增强图像对比度的一种方法。

均衡化处理后的图像只能是近似均匀分布。均衡化图像的动态分布范围扩大了，但其本质是扩大了量化间隔，而量化级别反而减少了，因此，原来灰度不同的像素经过处理后可能变得相同，形成了一片灰度相同的区域，各区域之间有明显的边界从而出现了伪轮廓。

在原始图像对比度本来就很高的情况下，如果再均值化则灰度调和，对比度会降低，在泛白缓和的图像中，均衡化会合并一些像素灰度，从而增大对比度，均衡化后的图片如果再对其均值化，则图像不会有任何变化。

### 相关API

``` C++
void equalizeHist(InputArray src,OutputArray dst)
```

