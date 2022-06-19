# EasyX的安装与使用

## 1. What's EasyX?

EasyX Graphics Library 是针对 Visual C++ 的免费绘图库，支持 VC6.0 ~ VC2019，简单易用，学习成本极低，应用领域广泛。

## 2. How to setup EasyX?

非常简单的安装步骤：“下载 -> 安装 -> 使用”，全过程不超过一分钟。具体安装步骤请参考 <https://easyx.cn/setup>

## 3. How to use EasyX?

创建一个空的控制台项目（Win32 Console  Application），然后添加一个cpp代码文件(main.cpp)，并引用 graphics.h 头文件就可以了。

```cpp
#include <graphics.h>		// 引用图形库头文件
#include <conio.h>
int main()
{
	initgraph(640, 480);	// 创建绘图窗口，大小为 640x480 像素
	circle(200, 200, 100);	// 画圆，圆心(200, 200)，半径 100
	_getch();				// 按任意键继续
	closegraph();			// 关闭绘图窗口
	return 0;
}
```

## 4. How to learn EasyX?

- 查看EasyX[帮助手册](https://docs.easyx.cn/zh-cn/intro)
- 查看[示例代码库](https://codebus.cn/)

---
Go to | [Home](../README.md) | [Head](#EasyX的安装与使用) | [MainFrame->Next](./D1_MainFrame.md) |

