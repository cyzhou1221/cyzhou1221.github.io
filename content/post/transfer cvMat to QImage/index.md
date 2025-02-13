---
title: cv::Mat 转化为 QImage (Deep copy)
date: 2019-08-06
comments: true
categories:
    - Blog
tags: 
    - OpenCV
    - Qt
---

### 一、QImage的深拷贝和浅拷贝

首先说一下什么是<font color=red> 深拷贝 </font>和<font color=red> 浅拷贝</font>。

<font color=green>When dealing with shared objects, there are two ways of copying an object. We usually speak about deep and shallow copies. A deep copy implies duplicating an object. A shallow copy is a reference copy, i.e. just a pointer to a shared data block. Making a deep copy can be expensive in terms of memory and CPU. In contrast, the benefit of shallow copied is that a program does not need to duplicate data unnecessarily, which results in lower memory use and less copying of data. </font>

<font color=blue>浅拷贝和深拷贝的区别</font>：浅拷贝后，两个不同的变量背后是<font color=Aqua>同一块内存空间</font>，它们只是名字不同而已，常见的浅拷贝有指针传递和引用类型；而深拷贝过后，两个不同的变量分别占据两块<font color=Aqua>不同的内存空间</font>(大小相同)，二者互不干扰，大多数赋值操作采用深拷贝的方式进行。

```cpp
我们再来看下QImage类几个常见的构造函数：

QImage(uchar *data, int width, int height, Format format)
QImage(const uchar *data, int width, int height, Format format)
QImage(uchar *data, int width, int height, int bytesPerLine, Format format)
QImage(const uchar *data, int width, int height, int bytesPerLine, Format format)

** 所需参数：
* 指向图像数据的uchar *型指针；
* 图像宽度；
* 图像高度；
* 图像格式；
* 图像的每行字节数(通道*宽度)；

  通过uchar *data可以看出这几个构造函数都是采用浅拷贝的方式进行的；

** 深拷贝
QImage QImage::copy(const QRect &rectangle = QRect()) const

Returns a sub-area of the image as a new image.

The returned image is copied from the position (rectangle.x(), rectangle.y()) in this image, and will always have the size of the given rectangle.
In areas beyond this image, pixels are set to 0. For 32-bit RGB images, this means black; for 32-bit ARGB images, this means transparent black; for 8-bit images, this means the color with index 0 in the color table which can be anything; for 1-bit images, this means Qt::color0.

If the given rectangle is a null rectangle the entire image is copied.

```

### 二、cv::Mat转化为QImage的背景

在使用 <font color=DeepPink>Qt+OpenCV</font> 写程序时，常常需要使用 OpenCV 对图像进行处理，然后利用 Qt 将图像显示出来，所以需要将 cv::Mat 类型转换为 QImage 类型。

通常是利用 QImage 的构造函数进行转换，但是，从构造函数的参数可以看出，这是通过<font color=SeaGre>浅拷贝</font>实现的，所以在 QImage 对象的生存期内，必须保证 uchar *data 指向的内存空间不会被释放。因此在返回 QImage 对象前，需要进行深拷贝。

### 三、代码

```cpp
QImage Mat2QImage(const cv::Mat &inputMat)
{
    cv::Mat tmpMat;
    QImage qImg;
    if(inputMat.channels() == 1) //灰度图像
    {
        tmpMat = inputMat;
        qImg = QImage((const uchar*)tmpMat.data,   //指向所用图像数据块的指针
                    tmpMat.cols,tmpMat.rows,
                    tmpMat.cols*tmpMat.channels(), //bytesPerLine，每行的字节数
                    QImage::Format_Indexed8);      //利用8bits存储灰度值
    }
    else
    {   //Mat是BGR排列, QImage是RGB排列，利用cvtColor变换颜色空间
        cv::cvtColor(inputMat,tmpMat,CV_BGR2RGB);
        qImg = QImage((const uchar*)tmpMat.data,   //指向所用图像数据块的指针
                    tmpMat.cols, tmpMat.rows,
                    tmpMat.cols*tmpMat.channels(), //bytesPerLine，每行的字节数
                    QImage::Format_RGB888);        //利用24bits真彩色存储RGB值
    }
    return qImg.copy();//deep copy
}
```

Plus: QImage 转 cv::Mat 代码可见博文 [大牛博文传送门](https://blog.csdn.net/liyuanbhu/article/details/86307283)