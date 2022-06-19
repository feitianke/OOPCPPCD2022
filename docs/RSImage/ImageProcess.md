# 遥感图像处理功能
本次实习完成几个图像处理算法，主要有：图像旋转、图像缩放、图像滤波等。要求参数使用对话框输入、输出结果能在视图上有所反映。

## 功能列表

### 统计信息显示
统计每个波段的最大值、最小值、均值、方差、直方图以图形化界面展示出来。

#### 统计值
- 界面参考工程[frozencolumn Project](../Code/frozencolumn)

#### 直方图
将直方图以折线图的方式予以展现。有关界面参考如下：
- lineChart

![lineChart](../Pics/Qt_linechart.png)

[lineChart Project](../Code/lineChart)

- splineChart

![splineChart](../Pics/Qt_splinechart.png)

[splineChart Project](../Code/splineChart)

### 显示图像基本信息
将图像的基本信息显示在对话框中，基本信息包括：图像行列值、投影参数及元数据信息等。
此部分的实习内容相对容易，即将CRSImage中读取的图像的基本信息显示在对话框中，具体的内容参考Qt控件的使用。
具体可参考：
- [Qt Widget Demo]
![Qt_Widget](../Pics/Qt_widget.png)
```
					Qt Widget Demo
```

![Qt_Widget_Video](../Pics/Qt_widget_video.png)
```
					Qt Widget Video
```

- Qt入门-[对话框简介](https://www.devbean.net/2012/09/qt-study-road-2-dialogs-intro/)
- Qt入门-[对话框数据传递](https://www.devbean.net/2012/09/qt-study-road-2-data-between-dialogs/)
- Qt入门-[标准对话框 QMessageBox](https://www.devbean.net/2012/09/qt-study-road-2-standard-dialogs-qmessagebox/)


### **R – Rotate Image**

**功能描述：**点击菜单**图像旋转**，弹出对话框，输入旋转角度（0-360°），实现图像逆时针旋转指定角度，并将结果显示在窗口中。

**程序流程：**1）弹出对话框，输入旋转角度（0—360°）；2）旋转图像；3）显示旋转后图像。

### **F – Filter**

**功能描述：**点击菜单**滤波(Filter...)**，提示输入滤波核，执行图像卷积。

**程序流程：**1）弹出对话框，输入滤波核；2）执行图像卷积；3）显示滤波后结果图像。


### **C –Close Image**

**功能描述：**点击菜单**关闭**，关闭当前打开的文件，图像窗口关闭。

**程序流程：**释放图像数据存储空间，擦除图像窗口。


---
|[Home](https://cugwhp.github.io/OOPCPP/CourseDesign/CourseDesignNew.html#%E8%AF%BE%E8%AE%BE%E5%86%85%E5%AE%B9) | [Return](#功能列表)| [Prev](./D4_Statistics.md) |
