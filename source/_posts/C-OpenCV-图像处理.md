---
title: C OpenCV 图像处理
date: 2020-11-05 14:22:30
categories: OpenCV
tags:
---

## 图像滤波与滤波器

图像滤波，指在尽量保留图像细节特征的条件下对目标图像的噪声进行抑制。

消除图像中的噪声成分叫做叫做图像的平滑化或者滤波操作。信号或图像的能量大部分集中在幅度谱的低频和中频段，而在较高频段，有用的信息经常被噪声淹没。因此一个能降低高频成分幅度的滤波器就能减弱噪声的影响。

opencv中提供了常见的五种滤波器：

- 方框滤波 BoxBlur
- 均值滤波 Blur
- 高斯滤波 GaussianBlur
- 中值滤波 medianBlur
- 双边滤波 bilateralBlur

## 滤波和模糊

滤波可以分为高通和低通，高斯滤波是指用高斯函数作为滤波函数的滤波操作，至于是不是模糊，要看是高斯低通还是高斯高通，低通就是模糊，高通就是锐化

## 线性滤波

### 方框滤波

``` C++

void boxFilter(InputArray src,OutputArray dst,int ddepth,size ksize,Point anchor = Point(-1,-1),boolnormalize = true,int borderType=BOARDER_DEFAULT);

```

size 代表内核大小 3×3，5×5等，anchor指平滑锚点，默认值为中间，normalize表示是否归一化，true的时候代表归一化，也就是**均值滤波**，不归一化是用于计算像素邻域内的积分特性。

### 高斯滤波

高斯滤波就是对整幅图像进行加权平均。用一个具体模版（掩膜）扫描图像中的每一个像素，用模版确定的领域内扫描图像中的每一个像素，用模版确定领域内像素的加权平均灰度值去替代模版中心的像素值。

``` C++
void GaussianBlur(InputArray src,OutputArray dst,Size ksize,double sigmaX,double sigmaY=0,int BORDER_DEFAULT)
```

## 非线性滤波

### 中值滤波

中值滤波是用像素点邻域灰度值的中值来代替该像素点的灰度值。

中值滤波与均值滤波比较：在均值滤波器中，用于噪声成分被放入平均计算中，所以输出受到了噪声的影响。但是在中值滤波器中，由于噪声成分很难被选上，所以几乎不会影响到输出。因此，中值滤波的消除噪声能力更胜一筹，中值滤波无论是在消除噪声还是保存边缘上都是一个不错的方法。但是中值滤波花费的时间是均值滤波的5倍以上。

``` C++
void medianBlur(InputArray src,OutputArray dst,int ksize)

```

### 双边滤波

双边滤波的好处，是可以做边缘保存，双边滤波相比于高斯滤波多了一个高斯方差sigma-d，它是基于空间分布的高斯滤波函数，所以在边缘附近，离得较远的向世俗不会对边缘上的像素值影响太多。但是，由于保存了过多的高频信息，对于彩色图像里的高频噪声，双边滤波器不能干净的滤掉，只能对于低频信息进行较好的滤波。简单来说，双边滤波中不仅考虑掩膜中点对于中心距离的影响，也考虑点本身像素与中心像素值的差的影响。

``` C++
void bilateralFilter(InputArray src,OutputArray dst,int d,double sigmaColor,double sigmaSpace,int boarderType=BORDER_DEFAULT)
```

d代表过滤过程中邻域像素的直径。
sigmaColor 代表颜色空间的sigma值，sigmaSpace代表坐标空间的sigma值

## 形态学滤波

### 形态学概述

形态学操作就是基于形状的一系列图像处理操作。

### 膨胀

膨胀就是求局部最大值的操作，这样的操作会使图像中的高亮区域逐渐增长。

### 腐蚀

腐蚀操作就是求局部最小值的操作。

### 相关API函数

``` C++

void cv::erode(InputArray src,OutputArray dst,InputArray kernel,Point anchor,int iterations,int boardtype,constScalar& boardvalue)
void cv::dilate(InputArray src,OutputArray dst,InputArray kernel,Point anchor,int iterations,int boardtype,constScalar& boardvalue)

```

第三个参数kernel为操作的核，当为NULL时，表示的是使用参考点位于中心3×3的核，第四个参数iteration代表迭代次数，默认为1，
anchor是锚点，默认为-1也就是中心。

### 开运算

开运算实际上就是先腐蚀后膨胀的过程

dst=open(src,element)=dilate(erode(src,element))

开运算可以用来消除小物体，在纤细处分离物体，并且在平滑较大物体的边界的同时不明显改变其面积。

### 闭运算

闭运算实际上就是先膨胀后腐蚀的过程。

dst=close(src,element)=erode(dilate(src,element))

闭运算能够排除小型黑洞（黑色区域）。

### 形态学梯度

形态学梯度是膨胀图与腐蚀图之差，对二值图像进行梯度操作可以将团块（blob）的边缘突出出来。我们可以用形态学梯度来保留物体的边缘轮廓。

### 顶帽

顶帽运算是原图像与上文刚刚介绍的开运算效果之差。
因为开运算带来的效果是放大了裂缝或者局部低亮度的区域。因此，从原图中减去开运算后的图，得到的效果图突出了比原图轮廓周围的区域更明亮的区域。
顶帽运算往往用来分离比邻近点亮一些的斑块。在一幅图像具有大幅的背景，而微小物品比较有规律的情况下，可以使用顶帽运算进行背景提取。

dst = tophat(src,element) = src-open(src,element)

### 黑帽

dst = blackhat(src,element) = close(src,element)-src

黑帽运算后的效果图突出了比原图轮廓周围的区域更暗的区域，且这一操作和选择的核的大小相关。所以黑帽运算用来分离一些比临近点暗一点的斑块，效果图有着非常完美的轮廓。

### 相关API

``` C++

void morphologyEx(InputArray src,OutputArray dst,int op,InputArray Kernel,Point anchor,int iteration,int boardtype,constScalar& boardvalue)

```

op参数代表形态学运算的类型。

标识符|含义
-|-
MORPH_OPEN|开运算
MORPH_CLOSE|闭运算
MORPH_GRADIENT|形态学梯度
MORPH_TOPHAT|顶帽
MORPH_BLACKHAT|黑帽
MORPH_ERODE|腐蚀
MORPH_DILATE|膨胀

## 漫水填充

漫水填充简单来说就是自动选中了和种子点相连的区域，接着将该区域替换成指定的颜色，这是个非常有用的功能,经常用来标记或者分离图像的一部分进行处理或分析.漫水填充也可以用来从输入图像获取掩码区域,掩码会加速处理过程,或者只处理掩码指定的像素点。效果类似于ps的魔术棒

在OpenCV中，漫水填充是填充算法中最通用的方法。且在OpenCV 2.X中，使用C++重写过的FloodFill函数有两个版本。一个不带掩膜mask的版本，和一个带mask的版本。这个掩膜mask，就是用于进一步控制哪些区域将被填充颜色（比如说当对同一图像进行多次填充时）。这两个版本的FloodFill，都必须在图像中选择一个种子点，然后把临近区域所有相似点填充上同样的颜色，不同的是，不一定将所有的邻近像素点都染上同一颜色，漫水填充操作的结果总是某个连续的区域。当邻近像素点位于给定的范围（从loDiff到upDiff）内或在原始seedPoint像素值范围内时，FloodFill函数就会为这个点涂上颜色。

在OpenCV中，漫水填充算法由floodFill函数实现，其作用是用我们指定的颜色从种子点开始填充一个连接域。连通性由像素值的接近程度来衡量。OpenCV2.X有两个C++重写版本的floodFill。

``` C++

 int floodFill(InputOutputArray image, Point seedPoint, Scalar newVal, Rect* rect=0, Scalar loDiff=Scalar(), Scalar upDiff=Scalar(), int flags=4 )

int floodFill(InputOutputArray image, InputOutputArray mask, Point seedPoint,Scalar newVal, Rect* rect=0, Scalar loDiff=Scalar(), Scalar upDiff=Scalar(), int flags=4 )

```

- 第一个参数，InputOutputArray类型的image, 输入/输出1通道或3通道，8位或浮点图像，具体参数由之后的参数具体指明。
- 第二个参数， InputOutputArray类型的mask，这是第二个版本的floodFill独享的参数，表示操作掩模,。它应该为单通道、8位、长和宽上都比输入图像 image 大两个像素点的图像。第二个版本的floodFill需要使用以及更新掩膜，所以这个mask参数我们一定要将其准备好并填在此处。需要注意的是，漫水填充不会填充掩膜mask的非零像素区域。例如，一个边缘检测算子的输出可以用来作为掩膜，以防止填充到边缘。同样的，也可以在多次的函数调用中使用同一个掩膜，以保证填充的区域不会重叠。另外需要注意的是，掩膜mask会比需填充的图像大，所以 mask 中与输入图像(x,y)像素点相对应的点的坐标为(x+1,y+1)。
- 第三个参数，Point类型的seedPoint，漫水填充算法的起始点。
- 第四个参数，Scalar类型的newVal，像素点被染色的值，即在重绘区域像素的新值。
- 第五个参数，Rect*类型的rect，有默认值0，一个可选的参数，用于设置floodFill函数将要重绘区域的最小边界矩形区域。
- 第六个参数，Scalar类型的loDiff，有默认值Scalar( )，表示当前观察像素值与其部件邻域像素值或者待加入该部件的种子像素之间的亮度或颜色之负差（lower brightness/color difference）的最大值。 
- 第七个参数，Scalar类型的upDiff，有默认值Scalar( )，表示当前观察像素值与其部件邻域像素值或者待加入该部件的种子像素之间的亮度或颜色之正差（lower brightness/color difference）的最大值。
- 第八个参数，int类型的flags，操作标志符，此参数包含三个部分，比较复杂，我们一起详细看看。

- 低八位（第0~7位）用于控制算法的连通性，可取4 (4为缺省值) 或者 8。如果设为4，表示填充算法只考虑当前像素水平方向和垂直方向的相邻点；如果设为 8，除上述相邻点外，还会包含对角线方向的相邻点。
- 高八位部分（16~23位）可以为0 或者如下两种选项标识符的组合：     

  - FLOODFILL_FIXED_RANGE - 如果设置为这个标识符的话，就会考虑当前像素与种子像素之间的差，否则就考虑当前像素与其相邻像素的差。也就是说，这个范围是浮动的。
  - FLOODFILL_MASK_ONLY - 如果设置为这个标识符的话，函数不会去填充改变原始图像 (也就是忽略第三个参数newVal), 而是去填充掩模图像（mask）。这个标识符只对第二个版本的floodFill有用，因第一个版本里面压根就没有mask参数。

- 中间八位部分，上面关于高八位FLOODFILL_MASK_ONLY标识符中已经说的很明显，需要输入符合要求的掩码。Floodfill的flags参数的中间八位的值就是用于指定填充掩码图像的值的。但如果flags中间八位的值为0，则掩码会用1来填充。

- 而所有flags可以用or操作符连接起来，即“|”。例如，如果想用8邻域填充，并填充固定像素值范围，填充掩码而不是填充源图像，以及设填充值为38，那么输入的参数是这样：
``` C++
flags=8 | FLOODFILL_MASK_ONLY | FLOODFILL_FIXED_RANGE | （38<<8）
```

## 图像金字塔与图片尺寸缩放

当我们想要将某种尺寸的图像转换为其他尺寸的图像，如果想放大或缩小图片尺寸，我们通常有两种方式。

- resize 最直接的方式
- pyrUp、pyrDown对图片进行向上采样或者向下采样

### 关于图像金字塔

图像金字塔是一种以多分辨率来解释图像的有效且简单的结构

金字塔的底部是待处理图像的高分辨率表示，而顶部是低分辨率的近似。

一般情况下有两种图像金字塔经常使用。

- 高斯金字塔：用来向下采样
- 用来从金字塔底层图像重建上层来采样图像，在数字图像处理中也就是预测参差，可以对图像进行最大程度的还原，配合高斯金字塔一起使用。
  
**对图像的向上向下采样是针对图像的尺寸而言的，向上指尺寸放大，向下指尺寸缩小。**

### 高斯金字塔

#### 对图片进行向下采样

为了获取层级为 G_i+1 的金字塔图像，我们采用如下方法:

<1>对图像G_i进行高斯内核卷积

<2>将所有偶数行和列去除

得到的图像即为G_i+1的图像，显而易见，结果图像只有原图的四分之一。通过对输入图像G_i(原始图像)不停迭代以上步骤就会得到整个金字塔。同时我们也可以看到，向下取样会逐渐丢失图像的信息。

以上就是对图像的向下取样操作，即缩小图像。

#### 对图片进行向上采样

如果想放大图像，则需要通过向上取样操作得到，具体做法如下：

<1>将图像在每个方向扩大为原来的两倍，新增的行和列以0填充

<2>使用先前同样的内核(乘以4)与放大后的图像卷积，获得 “新增像素”的近似值

得到的图像即为放大后的图像，但是与原来的图像相比会发觉比较模糊，因为在缩放的过程中已经丢失了一些信息，如果想在缩小和放大整个过程中减少信息的丢失，这些数据形成了拉普拉斯金字塔。

### 拉普拉斯金字塔

$$
L_i = G_i -UP(G_{i+1})\otimes g_{5\times5}
$$

式中的G_i表示第i层的图像。而UP（）操作是将源图像中位置为(x,y)的像素映射到目标图像的(2x+1,2y+1)位置，即在进行向上取样。符号表示卷积，为5x5的高斯内核。

### 相关API函数

``` C++
C++: void resize(InputArray src,OutputArray dst, Size dsize, double fx=0, double fy=0, int interpolation=INTER_LINEAR )
```

第三个参数，Size类型的dsize，输出图像的大小;如果它等于零，由fx，fy来计算

第四个参数，double类型的fx，沿水平轴的缩放系数，有默认值0，且当其等于0时，由dsize自动计算
 

第五个参数，double类型的fy，沿垂直轴的缩放系数，有默认值0，且当其等于0时，由dsize自动计算

第六个参数，int类型的interpolation，用于指定插值方式，默认为INTER_LINEAR（线性插值）。
 可选的插值方式如下：

- INTER_NEAREST - 最近邻插值
- INTER_LINEAR - 线性插值（默认值）
- INTER_AREA - 区域插值（利用像素区域关系的重采样插值）
- INTER_CUBIC –三次样条插值（超过4×4像素邻域内的双三次插值）
- INTER_LANCZOS4 -Lanczos插值（超过8×8像素邻域的Lanczos插值）

``` C++
C++: void pyrUp(InputArray src, OutputArraydst, const Size& dstsize=Size(), int borderType=BORDER_DEFAULT )
```

pyrUp( )函数的作用是向上采样并模糊一张图像，说白了就是放大一张图片。

``` C++
C++: void pyrDown(InputArray src,OutputArray dst, const Size& dstsize=Size(), int borderType=BORDER_DEFAULT)
```

pyrDown( )函数的作用是向下采样并模糊一张图片，说白了就是缩小一张图片。

## 阈值化

阈值化就是根据物体跟背景灰度的差异，通过将每一个像素值与设定阈值相比较，进行像素化的图像分割。

### 相关API

固定阈值操作

``` C++
double threhold(InputArray src,OutputArray dst,double thresh,double maxval,int type)
```

thresh代表阈值具体数值，maxval代表当type为二进制阈值时，表示白色颜色的就是maxval所代表像素值。

type代表阈值类型

参数|类型
-|-
THRESH_BINARY|二进制阈值
THRESH_BINARY_INV|反二进制阈值
THRESH_TRUNC|截断阈值
THRESH_TOZERO|阈值化为0
THRESH_TOZERO_INV|反阈值化为0 