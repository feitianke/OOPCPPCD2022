# 实习<一>程序框架构建
本次实习的目标是掌握Qt的IDE开发环境、Qt开发的基本框架，学习Qt示例工程imageviewer，掌握GUI程序的基本要素：视图、菜单、工具栏等。

## 目录
- [1. VS Qt Plugin]()
- [2. QtCreator](#1_QtCreator)
- [3. 示例代码](#2_示例代码ImageViewer)

## 1. VS Qt Plugin
Qt为VS安装了插件之后，VS菜单会出现Qt菜单项，界面如下：
![VS_Qt_Plugin](../Pics/VS_QtPlugin.png)

## 2. QtCreator
QtCreator是类似VS的IDE开发环境。
- Qt开发界面
![Qt开发界面](../Pics/Qt_face.png)

## 3. 示例代码ImageViewer
### 3.1 imageviewer GUI

![imageviewer](../Pics/imageviewer.png)
```
					示例代码界面图
```

### 3.2 代码分析
1. 工程组织

```c++
// imageviewer.cpp - imageviewer 实现
// imageviewer.h   - imageviewer 头文件
// imageviewer.pro - 工程文件
// main.cpp		   - 主程序main
```

2. 代码分析
- main.cpp

```c++
// main.cpp 是C++程序的唯一入口，定义了一个Qapplication对象，直至程序结束时返回。
#include <QApplication>				//Qt Application
#include <QCommandLineParser>		//Qt CommandLineParser， parse argv[]

#include "imageviewer.h"			//class imageviewer declartion

int main(int argc, char *argv[])	//main function
{
    QApplication app(argc, argv);	//application
    QGuiApplication::setApplicationDisplayName(ImageViewer::tr("Image Viewer"));	//Set Application Title
    
    // 可执行程序是可以带有参数调用的，如：imageviewer.exe d:/test.img 中的
    // d:/test.img 会存储在argv[1]中.
    QCommandLineParser commandLineParser;
    commandLineParser.addHelpOption();
    commandLineParser.addPositionalArgument(ImageViewer::tr("[file]"), ImageViewer::tr("Image file to open."));	//bind the 1st parameters with open
    commandLineParser.process(QCoreApplication::arguments());
    
    ImageViewer imageViewer;	// imageviewer window to display one image
    
    // 如果程序带有参数，则选择调用打开文件
    if (!commandLineParser.positionalArguments().isEmpty()
        && !imageViewer.loadFile(commandLineParser.positionalArguments().front())) {
        return -1;
    }
    imageViewer.show();		//显示窗口
    return app.exec();		//运行程序，直到程序退出
    //思考：程序是如何做到一直运行的呢？？
}
```

- imageviewer
  imageviewer的主要功能是负责图像的显示，以imageviewer.h为例说明。

```c++
#ifndef IMAGEVIEWER_H
#define IMAGEVIEWER_H

#include <QMainWindow>
#include <QImage>
#ifndef QT_NO_PRINTER
#include <QPrinter>
#endif

QT_BEGIN_NAMESPACE
class QAction;
class QLabel;
class QMenu;
class QScrollArea;
class QScrollBar;
QT_END_NAMESPACE

//! [0]
class ImageViewer : public QMainWindow
{
    Q_OBJECT

public:
    ImageViewer();					//构造函数
    bool loadFile(const QString &);	//加载图像

//信号槽
private slots:
    void open();
    void saveAs();
    void print();
    void copy();
    void paste();
    void zoomIn();
    void zoomOut();
    void normalSize();
    void fitToWindow();
    void about();

private:
    void createActions();
    void createMenus();
    void updateActions();
    bool saveFile(const QString &fileName);
    void setImage(const QImage &newImage);
    void scaleImage(double factor);
    void adjustScrollBar(QScrollBar *scrollBar, double factor);

    QImage image;				//用于绘制的图像对象，类似BITMAP
    QLabel *imageLabel;			//绘制图像的控件载体，类似Picture
    QScrollArea *scrollArea;	//滚动条
    double scaleFactor;			//缩放比例

#ifndef QT_NO_PRINTER
    QPrinter printer;
#endif

    QAction *saveAsAct;
    QAction *printAct;
    QAction *copyAct;
    QAction *zoomInAct;
    QAction *zoomOutAct;
    QAction *normalSizeAct;
    QAction *fitToWindowAct;
};
//! [0]
#endif
```

- imageviewer project

```c++
QT += widgets
requires(qtConfig(filedialog))
qtHaveModule(printsupport): QT += printsupport

HEADERS       = imageviewer.h
SOURCES       = imageviewer.cpp \
                main.cpp

# install
target.path = $$[QT_INSTALL_EXAMPLES]/widgets/widgets/imageviewer
INSTALLS += target
```

### 3.3 代码下载
参见Qt [Image Viewer Example code download](../Code/imageviewer.rar)

### 3.4 参考文档
- 学习[Qt5基本教程](https://blog.csdn.net/Louis_815/article/details/54286544)1序----8.添加动作

---
|[Home](https://cugwhp.github.io/OOPCPP/CourseDesign/CourseDesignNew.html#%E8%AF%BE%E8%AE%BE%E5%86%85%E5%AE%B9) | [Return](#目录)| [Prev](./D0_Preparation.md) | [Next](./D2_FileIO.md)|