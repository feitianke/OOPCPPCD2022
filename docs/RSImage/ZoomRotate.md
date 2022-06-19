# Day 6. 图像的缩放与旋转

## 图像缩放
在对图像的数据有了基本了解之后，我们可以将单波段的图像视作二维的矩阵。例如我们访问二维矩阵a[row][col]的某个像素值，可以采用行数和列数来寻址，如第i行第j列的像素值，可记为:a[i-1][j-1](i,j从1开始）。

图像缩放的意思就是对图像给予水平(x)和垂直(y)方向的比例因子，以此来改变图像的大小，图像的内容仍用原始图像的像素值来填充缩放后的图像（矩阵）内容。

在这个过程中，有一个非常重要的操作，我们称之为[图像重采样](https://blog.csdn.net/LanerGaming/article/details/49207435?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522159188202719725247625540%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=159188202719725247625540&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-4-49207435.ecpm_v1_rank_ctr_v3&utm_term=%E5%9B%BE%E5%83%8F%E9%87%8D%E9%87%87%E6%A0%B7)。因为我们知道缩放比例，就可以计算得b[i][j]对应于a[i0][j0]的位置，经过坐标转换后，你会发现i0,j0极有可能是非整数。此时，就需要利用i0,j0周围若干个整数位置的像素值加权求得第i0行,j0列的像素值，此过程我们称之为重采样。

- [图像缩放的参考例子](https://blog.csdn.net/jizhidexiaoming/article/details/80305451)

- 缩放的效果图
![image](https://img-blog.csdn.net/20180514101349889?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2ppemhpZGV4aWFvbWluZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

## 图像旋转
如果你对图像缩放的功能理解清楚了，图像旋转的实现也就非常容易实现了。图像的缩放，可以细分为2步：
- 1. 计算缩放后的图像坐标与原始图像坐标（行列数）的对应关系
- 2. 图像重采样，完成像素的赋值

此处，坐标对应关系，只需换成旋转（仿射变换）的公式即可，重采样的步骤完全一致。

---
|[Home](Subject.md)| [<-]() | [->]() |
