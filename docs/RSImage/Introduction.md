# Day 0 - 准备工作
> 此处将会说明完成此题所需要的工具、开发环境以及遥感图像数据。**工欲善其事，必先利其器！！！**

## 1. IDE Tools
### 1.1 IDE环境
C++的IDE环境众多，在不同操作系统下，均有不同的开发环境。为了方便程序的开发和调试，我们建议大家使用**Windows**操作系统来完成课程设计。尽管C++是跨操作系统的程序语言，但受界面库的影响，使用Windows会更方便。Windows下我们推荐如下几种IDE环境：
1. IDE的首选 - [Visual Studio >2017](https://visualstudio.microsoft.com/zh-hans/vs/community/)
> Visual Studio 是 Microsoft 出品的强大IDE，安装VS全家桶，几乎不用担心缺少库的情况。最佳版本是当前日期前2年的产品，会比较问题，奇葩问题会比较少，推荐VS2017 和 VS2019版本。

3. IDE的次选 - [Visual Code](https://code.visualstudio.com/)
> Visual Studio Code 也是 microsoft出品的开源编辑器，相比VS，VSCode具有小巧轻便、配置灵活的特点，只适合有**一定计算机基础的同学**，否则容易崩溃。VSCode的配置参考[VSCode C++配置](https://blog.csdn.net/YY_hide_on_bush/article/details/116752635?utm_source=app&app_version=4.9.3&code=app_1562916241&uLinkId=usr1mkqgl919blen)。

4. IDE的其他选择 - [Qt Creator](http://download.qt.io/archive/qt/5.10/5.10.1/qt-opensource-windows-x86-5.10.1.exe)
> Qt 是 Nokia 提供的开源IDE环境，Qt是一套跨平台的C++界面库，借助于Qt可以开发出精美的GUI程序。Qt也只适合有**一定计算机基础**的同学使用，Qt配置参考[Qt的安装](./QtSetup.md)。

### 1.2 [EasyX图形库](http://easyx.cn)
> EasyX是一个简单易用的图形库，登录[EasyX](http://easyx.cn)官网首页，可以找到EasyX的[安装程序](https://easyx.cn/download/EasyX_20210719.exe)和[帮助文档](https://docs.easyx.cn/zh-cn/intro)。若选择Visual Studio为IDE，整个安装和配置过程不到一分钟即可完成，若选择Visual Code为IDE，则需要一定的[配置过程](https://blog.csdn.net/YY_hide_on_bush/article/details/116752635?utm_source=app&app_version=4.9.3&code=app_1562916241&uLinkId=usr1mkqgl919blen)。

### 1.3 下载示例数据
> 为了测试程序功能，特准备了一组遥感图像数据，可通过此链接下载[遥感测试数据](../../data/rsimage.zip)。


### 1.4 [FWTools](http://home.gdal.org/fwtools/FWTools247.exe)
> 遥感图像数据不能用常用的图像查看工具打开，为了方便查看遥感数据的信息、显示，可下载开源的[遥感图像显示工具-FWTools](http://home.gdal.org/fwtools/FWTools247.exe)，有关FWTools的使用可查看[FWTools说明](../../refs/OpenEV_Manual.pptx)。


---
准备工作包括：**安装IDE、下载安装EasyX库、下载遥感测试数据、下载FWTools查看遥感图像**。你准备好了吗？

---