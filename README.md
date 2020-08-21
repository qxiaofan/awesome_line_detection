## 目录
<!-- MarkdownTOC depth=4 -->
- [一 前言](#前言)
- [二 Hough_Line](#Hough)
- [三 LSD](#LSD)
- [四 FLD](#FLD)
- [五 LSWMS](#LSWMS)
- 六 [LSM](LSM)
- [七 EDline](#EDline)
- [八 CannyLines](#CannyLines)
- [九 MCMLSD](#MCMLSD)

<a name="前言"></a>

## 一 前言

> 公众号：[3D视觉工坊](https://mp.weixin.qq.com/s?__biz=MzU1MjY4MTA1MQ==&mid=2247484684&idx=1&sn=e812540aee03a4fc54e44d5555ccb843&chksm=fbff2e38cc88a72e180f0f6b0f7b906dd616e7d71fffb9205d529f1238e8ef0f0c5554c27dd7&token=691734513&lang=zh_CN#rd)
>
> 主要关注：3D视觉算法、SLAM、vSLAM、计算机视觉、深度学习、自动驾驶、图像处理以及技术干货分享
>
> 运营者和嘉宾介绍：运营者来自国内一线大厂的算法工程师，深研3D视觉、vSLAM、计算机视觉、点云处理、深度学习、自动驾驶、图像处理、三维重建等领域，特邀嘉宾包括国内外知名高校的博士硕士，旷视、商汤、百度、阿里等就职的算法大佬，欢迎一起交流学习
>
> 小助理微信：CV3Der
>
> 联系邮箱：vision3d@yeah.net



本仓库主要借鉴参考[LineSegmentsDetection](https://github.com/Vincentqyw/LineSegmentsDetection)，以及博客[直线检测算法汇总](https://blog.csdn.net/WZZ18191171661/article/details/101116949#t9)，在此基础上，增加自己的一些理解，主要介绍关于线段检测的一些算法汇总，请大家以论文与代码为主，如有不到之处，欢迎提交issue。

<a name="Hough"></a>

## 二 Hough_Line

Hough变换大家应该比较熟悉，主要用来进行直线、圆、椭圆检测等。

实现步骤主要包括，[参考](https://blog.csdn.net/WZZ18191171661/article/details/101116949#t2)：

1、首先，它创建一个二维数组或累加器（用于保存两个参数的值，并将其初始设置为零）；

2、用r来表示行，用theta来表示列

3、数组的大小取决于你所需要的精度。假设您希望角度的精度为1度，则需要180列（直线的最大度数为180）；

4、对于r,可能的最大距离是图像的对角线长度。因此，取一个像素精度，行数可以是图像的对角线长度。

当然，除了HoughLines，HoughLinesP是Hough_line算法的改进版，具有更快的速度和更好的效果。

HoughLinesP不仅使用起来比较方便，基本上不需要进行调节参数，除此之外，该算法获得更好的直线检测效果。

<a name="LSD"></a>

## [三 LSD](http://www.ipol.im/pub/art/2012/gjmr-lsd/)

- 论文标题："LSD: a Line Segment Detector"
- 项目主页：http://www.ipol.im/pub/art/2012/gjmr-lsd/
- 论文地址：http://www.ipol.im/pub/art/2012/gjmr-lsd/article.pdf
- 代码地址：http://www.ipol.im/pub/art/2012/gjmr-lsd/lsd_1.5.zip

该方法是目前性价比（速度精度）最好的算法，现已经集成到`opencv`中[`LSDDetector`](https://docs.opencv.org/master/d1/dbd/classcv_1_1line__descriptor_1_1LSDDetector.html)。LSD能够在线性时间内检测到亚像素精度的线段。无需调整参数，适用于各种场景。因为每张图有误检，LSD能够控制误检率。PS：论文此处不介绍了，可以参考[这里](https://blog.csdn.net/chishuideyu/article/details/78081643?locationNum=9&fps=1)。

缺点：它在背景含有一些白噪声的图像中容易失败，使得它不适合实时应用，参考[[线特征---EDLines原理](https://www.cnblogs.com/Jessica-jie/p/7655466.html)]



<a name="FLD"></a>

## 四 FLD 

- 论文标题：“Outdoor Place Recognition in Urban Environments using Straight Lines”
- 论文地址：http://cvlab.hanyang.ac.kr/~jwlim/files/icra14linerec.pdf

该论文尝试着使用直线特征来代替原始的SURF点特征点进行建筑物识别。与点特征进行相比，线特征具有更容易发现和更好的鲁棒性，线特征基本上不会受到光照、遮挡、视角变化的影响。下面展示了该算法的直线检测效果，从图中我们可以看出，线特征比点特征更好一些。

<a name="LSWMS"></a>

## 五 LSWMS

- 论文标题："Line segment detection using weighted mean shift procedures on a 2D slice sampling strategy"
- 论文地址：[https://www.researchgate.net/LSWMS.pdf](https://www.researchgate.net/profile/Marcos_Nieto3/publication/220654859_Line_segment_detection_using_weighted_mean_shift_procedures_on_a_2D_slice_sampling_strategy/links/56a5d56a08aef91c8c16b1ac.pdf?inViewer=0&origin=publication_detail&pdfJsDownload=0)
- 代码地址：https://sourceforge.net/projects/lswms/



##  六 LSM

- 项目主页：http://faculty.pucit.edu.pk/nailah/research/LSM/
- 论文地址：http://faculty.pucit.edu.pk/nailah/research/LSM/Line%20Segment%20Merging%20(LSM).pdf
- [代码链接](http://faculty.pucit.edu.pk/nailah/research/LSM/LSM Demo.zip)

 LSM算法不仅仅是一个直线检测算法，同时也是一个直线合并算法。论文中提出了一种合并这些断开的线段的算法，以恢复原始的感知准确的线段。该算法根据角度和空间接近度对线段进行分组。然后将每组中满足新的自适应合并准则的线段对依次合并成一条线段。重复此过程，直到不再合并行段。我们还提出了一种定量比较线段检测算法的方法。在york-urban数据集上的结果表明，与最新的线段检测算法相比，我们的合并线段更接近人类标记的地面真线段。

**LSM可以将一些间断的直线合并成一条更长的直线，这在现实场景中具有很大的用处，但是我们也会发现LSM算法会错误的将一些直线进行合并，会造成一些误差。**

<a name="EDline"></a>

## 七 EDline（ED: Edge Drawing）

- 论文标题："Edge Drawing: A Combined Real-Time Edge and Segment Detector"
- 论文地址：https://sci-hub.tw/10.1016/j.jvcir.2012.05.004
- 代码地址：https://github.com/mtamburrano/LBD_Descriptor

<a name="CannyLines"></a>

## [八 CannyLines](http://cvrs.whu.edu.cn/projects/cannyLines/)

- 论文标题："CannyLines: A Parameter-Free Line Segment Detector"
- 项目主页：http://cvrs.whu.edu.cn/projects/cannyLines/
- 论文地址：http://cvrs.whu.edu.cn/projects/cannyLines/papers/CannyLines-ICIP2015.pdf
- 代码地址：http://cvrs.whu.edu.cn/projects/cannyLines/codes/CannyLines-v3.rar

本文提出了一种鲁棒的线段检测算法来有效地检测来自输入图像的线段。首先，文章提出了一种无参数的Canny算子，称为Canny（PANYPF），通过自适应地设置传统Canny算子的低阈值和高阈值来鲁棒地从输入图像中提取边缘。然后，提出了有效的像素连接和分割技术，直接从边缘图中收集共线点群，用于基于最小二乘拟合方法拟合初始线段。第三，通过有效的扩展和合并，产生更长、更完整的线段。最后，利用亥姆霍兹原理（Helmholtz Principle）对所有的检测线段检测，主要考虑梯度方向和幅度信息。该算法能够在人工场景中能够获得比LSD以及EDline精度更高以及平均长度更高的线段。

<a name="MCMLSD"></a>

## 九 MCMLSD

- 论文标题："MCMLSD: A Dynamic Programming Approach to Line Segment Detection"
- 论文地址：http://openaccess.thecvf.com/content_cvpr_2017/papers/Almazan_MCMLSD_A_Dynamic_CVPR_2017_paper.pdf
- 代码地址：http://www.elderlab.yorku.ca/?smd_process_download=1&download_id=8423
