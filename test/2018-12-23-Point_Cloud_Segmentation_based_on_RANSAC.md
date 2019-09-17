---
layout: post
title:  "基于PCL的三维激光点云平面分割"
date:   2018-12-23 12:33:09
categories: [Technical Sharon]
tags: [PCL,点云分割]
comments: true
# photos: 
#     - "/image/posts/Next.jpg"
---


三维激光扫描技术是上世纪九十年代中期开始出现的一项高新技术，是继GPS空间定位系统之后又一项测绘技术新突破，可以快速、大量的采集空间点位信息，为快速建立物体的三维影像模型提供了一种全新的技术手段。<!--more-->它通过高速激光扫描测量的方法，大面积高分辨率地快速获取被测对象表面的三维坐标数据，具有采样密度高、点云分布密集等特点，成为三维空间信息快速获取的主要手段之一，被广泛应用于文物保护、三维重建、数字地面模型生产、城市规划等领域。

## 点云数据介绍

三维激光扫描通过测量实物表面的三维坐标点集，得到的大量坐标点的集合称为点云(PointCloud)或点云数据(PointCloudData)。点云数据是三维数据的一种表示形式，是三维扫描仪对物体进行采样得到的物体表面的直接采样点，是对物体表面的几何属性最真实地记录。

## 点云分割
点云分割是将具有相同或者相似性质的数据点进行聚类，这需要从散乱的点云数据中获取原始的三维数据信息。近年来，国内外许多学者在点云分割方面做了大量的研究，也取得了较多成果，并设计了许多算法来进行物体表面特征的提取，主要有：
*   &nbsp;区域生长法
*   &nbsp;随机一致性法（RANSAC）
*   &nbsp;特征空间聚类法
*   &nbsp;能量优化法等。

一般来说，基于区域生长的分割方法对于二次曲面的分割比较有效，因为二次曲面可由多项式表达；存在的问题是难以选择合适种子点以及区分光滑边界，而且其区域生长受设定阈值的影响较大，选择合适的生长准则也比较困难。

基于特征聚类的分割方法对于曲面类型较为明显的曲面分块存在一定的优势，但是对于复杂的曲面而言，要直接确定曲面的分类个数和曲面类型十分困难，对容易出现的细碎面片进行二次处理也增加了算法的难度。

本次实验采用RANSAC算法进行点云分割来提取平面， RANSAC是Random Sample Consensus（随机抽样一致性）的缩写，是从一个观察数据集合中，估计模型参数（模型拟合）的迭代方法。RANSAC是随机的不确定的，每次运算求出的结果可能不相同，但总能给出一个合理的结果，为了提高概率必须提高迭代次数。RANSAC 的主要特点有：在每次迭代过程中，最好的策略是选择最小的数据集来做模型估计；最大迭代次数并非是固定不变的，一般是用上次迭代得到的内点比例，来更新最大迭代次数；当迭代结束后，需要用所有的内点来重新估计模型。RANSAC 是一个鲁棒性非常好的模型估计算法，主要用来从一系列包含离群点的数据中估计出一个预先定义好的模型出来。

本次实验选择RANSAC算法进行点云分割提取平面,下面对RANSAC算法及实验过程及结果进行讲述。

## PCL点云库环境配置
[PCL（Point Cloud Library）](http://pointclouds.org/)是在吸收了前人点云相关研究基础上建立起来的大型跨平台开源C + +编程库，它实现了大量点云相关的通用算法和高效数据结构，涉及到点云获取、滤波、分割、配准、检索、特征提取、识别、追踪、曲面重建、可视化等。
首先完成PCL点云库的环境安装，为后续平面分割实验做准备。

## 基于RANSAC算法的平面分割实验

#### 查看数据，点云数据可视化：

CloudCompare是一款三维点云处理软件(例如那些用激光扫描仪获得的),它还可以处理三角网格和校准图像。CloudCompare提供了一组基本工具，用于手工编辑和渲染3D点云和三角形网格。它还提供了各种先进的处理算法，其中包括投影、配准、距离计算、统计计算、分割、几何特征计算等。所以，在实验之前利用CloudCompare加载原始数据进行观察，indoor点云数据的可视化结果:

<img src="/image/posts/blog2indoor.png" style="display:block;margin:0 auto;">

#### 数据转换：

在使用PCL点云库进行程序编写的时候，其内定的点云数据格式类型为.pcd.所给的原始数据有两种格式：.txt和.las，所以将数据转换成.pcd格式对于后续的点云处理是非常有意义的。数据转换的方法不是唯一的，进行了两种转换方式的探索。

（1） .las 转.pcd：libLAS 是一个用来读写三维激光雷达数据（LiDAR）的C++库，调用liblas以及pcl点云库便可实现.las与.pcd格式转换；

（2） .txt 转.pcd：利用CloudCompare软件，首先导入文本文件然后导出.pcd数据便可完成.txt与.pcd格式转换。

#### 从.pcd文件中读取点云数据：

关键代码如下：

```c++
#include <iostream>           // 标准C++库中的输入输出类相关头文件。
#include <pcl/io/pcd_io.h>      // pcd 读写类相关的头文件。
#include <pcl/point_types.h>    // PCL中支持的点类型头文件。

pcl::PointCloud<pcl::PointXYZ>::Ptr cloud(newpcl::PointCloud<pcl::PointXYZ>);  //创建一个PointCloud<PointXYZ> boost共享指针并进行实例化。 
pcl::io::loadPCDFile("indoor.pcd", *cloud);   //填入点云数据
```

#### 使用VoxelGrid滤波器对点云进行滤波（下采样）：

在点云数据采集过程时，由于设备精度、操作者经验环境因素、电磁波的衍射特性、被测物体表面性质变化和数据拼接配准操作过程的影响，点云数据中不可避免的出现一些噪声。在点云处理流程中，滤波处理作为预处理的第一步，对后续的影响比较大。只有在滤波预处理中将噪声点、离群点、孔洞、数据压缩等按照后续处理定制，才能够更好的进行后续应用处理。

PCL中点云滤波模块提供了很多灵活实用的滤波处理算法，例如：双边滤波、高斯滤波、条件滤波、直通滤波、基于随机采样一致性滤波等。本实验采用VoxelGrid滤波器对点云进行下采样，该方法使用体素化网格方法实现下采样，即减少点的数量，并同时保存点云的形状特征，在分割，曲面重建，形状识别等算法速度中非常实用。VoxelGrid类通过输入的点云数据创建一个三维体素栅格，容纳后每个体素内用体素中所有点的重心来近似显示体素中其他点，这样该体素内所有点都用一个重心点最终表示，对于所有体素处理后得到的过滤后的点云，这种方法比用体素中心逼近的方法更慢，但是对于采样点对应曲面的表示更为准确。

```c++
pcl::VoxelGrid<pcl::PCLPointCloud2> sor;  //创建滤波对象
sor.setInputCloud (cloud);                //设置需要过滤的点云给滤波对象
sor.setLeafSize (0.06f, 0.06f, 0.06f);    //设置滤波时创建的体素体积为1cm的立方体
sor.filter (*cloud_filtered);             //执行滤波处理，存储输出
```
<img src="/image/posts/blog2afterfilter.png" style="display:block;margin:0 auto;">

<img src="/image/posts/blog2compare.png" style="display:block;margin:0 auto;">

对比可以看出：滤波之后，点的密度大小和整齐程度与之前有所差异，虽然处理后数据量大大减少，但其所含有的形状特征与空间结构信息与原始点云差不多，这为后续的点云分割提供了便捷。

#### 创建分割对象，选用分割模型：

实验中选用的是RANSAC算法来进行分割，该算法的主要解决步骤是：

**1) 在点云数据中随机选择3个点并且计算构成的平面；**

**2) 计算其他所有点到该平面的距离；**

**3) 根据距离阈值T，将所有距离大于T的点标记为外点；**

**4) 计算外点的数量；**

**5) 重复1-4步骤N次；**

**6) 将含有最大内点数量的平面选为最佳拟合平面，从点云数据中去除平面的内点；**

**7) 重复以上步骤，直到点云数据为空。**

关键代码如下：

```c++
pcl::ModelCoefficients::Ptr coefficients(new pcl::ModelCoefficients()); //存储输出的模型系数
pcl::PointIndices::Ptr inliers(new pcl::PointIndices());//存储内点
pcl::SACSegmentation<pcl::PointXYZ>  seg; //创建分割对象
seg.setOptimizeCoefficients(true);
seg.setModelType(pcl::SACMODEL_PLANE);   //设置模型类型，这里是提取平面
seg.setMethodType(pcl::SAC_RANSAC);      //设置分割方法为RACSAC
seg.setMaxIterations(1200);              //设置最大迭代次数
seg.setDistanceThreshold(0.2);           //设置距离阈值
```

## 实验结果

在本实验的参数设置下，利用RANSAC方法分割得到总共7个平面：

<img src="/image/posts/blog2filterresult.png" style="display:block;margin:0 auto;">

将分割得到的各平面.pcd文件在CloudCompare软件中进行可视化，不同的面设置不同的颜色来进行区分，不同视角的观察结果如图:

<img src="/image/posts/blog2result.png" style="display:block;margin:0 auto;">

从效果来看，采用RANSAC算法提取的平面整体效果还可以。RANSAC的优点是它能鲁棒的估计模型参数，例如，它能从包含大量局外点的数据集中估计出高精度的参数。RANSAC的缺点是它计算参数的迭代次数没有上限；如果设置迭代次数的上限，得到的结果可能不是最优的结果，甚至可能得到错误的结果。RANSAC只有一定的概率得到可信的模型，概率与迭代次数成正比。RANSAC的另一个缺点是它要求设置跟问题相关的阀值，设置较合适的阈值这往往并不是非常简单的。

## 总结

点云分割是是三维激光扫描的点云数据处理和分析的重要研究内容。本文实现的点云分割提取平面实验，首先转换数据，获取到.pcd格式的点云数据；考虑到在点云数据采集过程时，由于设备精度、操作者经验环境因素等影响，实验中采用VoxelGrid滤波器对点云进行下采样，该方法使用体素化网格方法实现下采样，即减少点的数量，并同时保存点云的形状特征；利用RANSAC算法来进行分割，提取到7个平面。实验结果表明，RANSAC算法算法在平面分割方面具有比较大的优点，是一个鲁棒性非常好的模型估计算法，但是其在参数设定等方面还存在不足。