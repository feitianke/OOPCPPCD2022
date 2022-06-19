# RSCmd Manual

## 0. 操作视频
- [01.MainFrame](../../videos/01.RSCmd_MainFrame.avi)
- [02.OpenData](../../videos/02.RSCmd_OpenData.avi)
- [02-1.Open](02.RSCmd_2_Open.avi)
- [03.Display](03.RSCmd_Display.avi)

## 1. Introduction RSCmd and Data

- RSCmd.rar

> 压缩包，包含着RSCmd.exe和数据test.hdr, test.img。

- RSCmd.exe

> 可执行程序，即源代码编译链接后的输出结果。

- test.hdr

> 图像元数据文件，描述图像数据的数据，是ASCII文件，用记事本(notepad)或Notepad++，EditPlus等工具打开即可看到相关内容。打开示意图如下：

![test_hdr](../../png/hdr.png)

- test.img

> 图像数据文件，图像二进制流，按照排列顺序，一个一个像素顺序排列的文件。



## 2. How to Execute RSCmd

- Win+R - Windows图标+R

弹出运行框，输入cmd，执行，弹出控制台窗口

![WinR](../../png/WinR.png)

![Console](../../png/Console.png)

- Execute RSCmd

> 输入如下的命令行，即可启动程序

![RSCmd](../../png/RSCmd.png)

- RSCmd.exe

> 程序界面如下：

![RSCmdFace](../../png/RSCmdFace.png)



## 3. Command Line

### 3.1 Open

- Open Command

![Open](../../png/Open.png)

### 3.2 Statistics

- Command Line

![StatisticsCmd](../../png/StatisticsCmd.png)

- Output

![Statistics](../../png/Statistics.png)

### 3.3 Histogram

- Command Line

![HistCmd](../../png/HistogramCmd.png)

- Output Histogram

![Histogram](../../png/Histogram.png)

### 3.4 Display

- Command Line

![NormalDisp](../../png/NormalCmd.png)

- Output Image:

![Normal](../../png/Normal.png)

- Linear Stretch

![LinearDispCmd](../../png/LinearDispCmd.png)

- Linear stretch Display

![LinearDisp](../../png/LinearDisp.png)

- Gray Display Command Line

![GrayCmd](../../png/GrayCmd.png)

- Output: Gray Display

![Gray](../../png/Gray.png)



- **试着改变波段组合，显示不同的伪彩色图像**

### 3.5 Filter

- Filter Command Line

![MeanFilterCmd](../../png/MeanFilterCmd.png)

- Image Filtered

![MeanFilter](../../png/MeanFilter3.png)

- **试着改变滤波核的大小，看看图像会有哪些变化。**

- 自定义滤波

![FilterCmd](../../png/FilterCmd.png)

- Image Filtered

![FilterRes](../../png/FilterRes.png)

- **自定义输入滤波核，观察图像的变化！**

