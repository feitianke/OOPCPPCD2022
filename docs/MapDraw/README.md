# 题目--图形绘制程序
## 目录
- [课设内容](#课设内容)
- [课设要点](#课设要点)
- [报告模板](#报告模板)
- [附录](#附录)

## 课设内容
本次课设的内容是运用C++语言，编写一个简易的栅格图像矢量化程序。以栅格图像为底图，实现点、线、多边形的绘制功能，并能够存储矢量为文件。


其功能项如下：
|  序号  | 功能菜单             |              功能              | 备注     |
| :--: | :--------------- | :--------------------------: | ------ |
|  00  | X – Exit         |             退出程序             | **必做** |
|  01  | I – Load Image   |      打开栅格图像文件，加载到窗口      | **必做** |
|  02  | O – Open Vector  | 打开矢量文件(.vec)，加载到视图窗口，与图像叠加显示 | **必做** |
|  03  | N – New Vector   |            新建矢量文件(.vec)，用于存放矢量文件    | **必做** |
|  04  | S – Save         |     保存绘制结果为矢量文件     | **必做** |
|  05  | P – Point        |     绘制点，可设置点的大小与颜色                 | **必做** |
|  06  | L – Line         |     绘制线，可设置线的粗细、颜色、样式                    | **必做** |
|  07  | Y – Polygon      |  绘制多边形(封闭)，可设置多边形的边界的粗细、颜色和填充色| **必做**   |
|  08  | M – Move         |     移动选中矢量的坐标      | *选做*   |
|  09  | D – Delete       |   删除选中的矢量对象 | *选做*   |
|  10  | U - Modify       |      修改选中矢量的颜色、线宽、样式等      | *选做*   |
|  11  | T - Statistics  |      统计图形的数量、长度、面积等  | *选做*   |

- [详细功能描述](./Subject.md)

- [MapDraw 演示视频](https://space.bilibili.com/383106091/channel/detail?cid=191672&ctype=0)

- 优秀作品展示 New！！！
1. [From Qinwanrou](https://www.bilibili.com/video/BV1Yf4y157GN?share_source=copy_web)
1. [From ChenJing](https://www.bilibili.com/video/BV1Ro4y1D7pW?share_source=copy_web)
2. [From PanJiale](https://www.bilibili.com/video/BV1vw41197t7?share_source=copy_web)
3. [From YueShengjie](https://www.bilibili.com/video/BV1A64y167Wp?share_source=copy_web)


## 课设要点

### [Day0：EasyX的安装与使用](./D0_EasyX.md)

### [Day1：程序主框架搭建](./D1_MainFrame.md)

### [Day2：鼠标消息](./D2_MouseMsg.md)

### [Day3：图形绘制消息](./D3_DrawMsg.md)

### [Day4：图形类的设计](./D4_ShapeClass.md)

### [Day5：形状数据集的构建](./D5_ShapeSets.md)

### [Day6：矢量图形的IO](./D6_ShapeIO.md)

### [Day7：矢量图形的编辑](./D7_ShapeEdit.md)

### [Day8：调试与报告撰写](./D8_DebugReport.md)


## 报告模板
- [课设报告模板](../../refs/Report_Template.doc)

## 附录
- [EasyX 官网](https://easyx.cn/)
- [EasyX 手册 1中文版](https://docs.easyx.cn/zh-cn/intro)  [2英文版](https://docs.easyx.cn/en-us/tutorials)
- 视频-[C++EasyX图形编程教程](https://www.51zxw.net/List.aspx?cid=802)
- 矢量化

---
[Home](../../README.md) | [Head](#题目--图形绘制程序) | [图像处理程序<-Prev](../RSImage/README.md)

