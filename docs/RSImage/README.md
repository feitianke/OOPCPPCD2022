# 遥感图像处理程序
## 目录
- [课设内容](#课设内容)
- [课设要点](#课设要点)
- [报告模板](#报告模板)
- [附录](#附录)

## 课设内容
本次课设的内容是运用C++语言，构建一个简易的图像处理系统，实现初步的图像处理功能。
1. 编写一个遥感图像处理控制台程序，要求在控制台上输入字母菜单，执行相应的功能。
2. 构建GUI界面程序完成相应的功能移植。
3. **要求用类对象，实现主体功能。功能介绍见[RSManual](./RSCmdManual.md)**

其功能项如下：
|  序号  | 功能菜单             |              功能              | 备注     |
| :--: | :--------------- | :--------------------------: | ------ |
|  00  | X – Exit         |             退出程序             | **必做** |
|  01  | O – Open Image   |      输入图像文件路径，即可读入文件数据       | **必做** |
|  02  | I – Information  | 输出当前图像的路径，行列值、波段数、数据类型、排列方式等 | **必做** |
|  03  | C –Close Image   |            关闭当前图像            | **必做** |
|  04  | S – Statistics   |     输出图像数据统计量，文件未打开，输出提示     | **必做** |
|  05  | H – Histogram    |           输出图像的直方图           | **必做** |
|  06  | ? – Help         |            输出本信息             | **必做** |
|  07  | A – Save as File |     输入保存的文件路径，输出图像为二进制文件     | **必做**   |
|  08  | Z – Zoom         |     图像缩放，输入缩放比例尺，输出缩放图像      | **必做**   |
|  09  | R – Rotate Image |     图像旋转，输入旋转角度，逆时针旋转图像      | *选做*   |
|  10  | F - Filter       |      输入滤波核，执行滤波，输出滤波后图像      | *选做*   |
|  11  | GUI              |      使用界面库，如Qt等构建GUI程序，移植已有功能| *选做*   |

- [详细功能描述](./Subject.md)

## 课设要点
从实现的角度来看，可分为3个步骤：
1. 搭建程序框架和设计数据结构
搭建主函数流程和菜单项、设计图像数据的矩阵存储
2. 实现各项功能
实现所要求的各子功能:Open,Display,Rotate...
3. 构建GUI程序
构建GUI菜单和功能，完成功能的移植

### [Day 1. 构建程序框架](./Frame.md)
构建控制台界面，理解题目需求，设计相关的类（数据组织）

### [Day 2. 图像文件I/O](./FileIO.md)
读写图像文件和元数据文件，掌握图像格式的存储

### [Day 3. 图像信息输出](./Information.md)
输出图像的数据信息，如：行、列、波段数、数据类型、投影、控制点信息等。

### [Day 4. 图像统计量计算](./Statistics.md)
将每个波段的最大值、最小值、均值、方差、直方图输出

### [Day 5. New!!! 图像显示 EasyX](./Display_EasyX.md) | [图像显示Win32](./Display.md)
将图像文件输出到屏幕[EasyX](http://easyx.cn)

### [Day 6. 图像缩放与旋转](./ZoomRotate.md)
实现图像的缩放和旋转

### [Day 7. 图像滤波](./Filter.md)
图像滤波

### [Day 8. 构建GUI框架](./Gui.md)
Qt的下载、安装、配置及使用

### [Day 9. 功能移植](./Menu.md)
完成功能菜单的挂接和消息槽函数的映射

### [Day 10. 界面完善](./ImageProcess.md)
完善已有功能的界面优化，如：直方图的图形化显示等

## 报告模板
- [课设报告模板](../../refs/Report_Template.doc)

## 附录
- [The Basic of Remote Sensing Image](../../refs/Basic_RS_Image.docx)
- [遥感测试数据](../../data/rsimage.zip)
- [遥感图像显示工具-FWTools](../../refs/OpenEV_Manual.pptx) [下载FWTools](http://home.gdal.org/fwtools/FWTools247.exe)
- [Qt配置](./QtSetup.md)
- [The Basic of Remote Sensing Image](../../refs/Basic_RS_Image.docx)

---
[Home](../../README.md) | [Head](#遥感图像处理程序) | [->Next](../MapDraw/README.md)