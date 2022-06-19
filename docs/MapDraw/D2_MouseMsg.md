# 消息响应
[TOC]

GUI程序是以消息驱动来执行程序的。以下内容包括：消息是什么？程序包含哪些消息？消息如何驱动？消息如何处理？以及消息处理示例。希望大家能够举一反三，对基本的消息处理流程加深了解，可以在此基础上新增消息处理功能。

## 1. 消息及消息驱动
消息的英文是Message，故名思意，就是通过通信、反馈来实现程序的运转。消息如何产生的呢？本质上讲，消息是由硬件中断产生。
### 1.1. 什么是消息？
常见的软件消息包括：鼠标消息和键盘消息，当然还可以包括各种硬件设备，如打印机等等。当用户敲击键盘上的任意键，就会触发键盘消息，键盘消息会包含按键的信息。鼠标在移动、按钮被按下、滚轮滚动等操作都会触发鼠标消息。当输入设备触发了消息，系统就需要对消息捕获和处理，GUI程序的执行流程是建立在消息传递机制上的。

### 1.2. 程序包括哪些消息？
1. 键盘消息
如上所述，键盘按键会触发按键消息，通过函数_kbhit()可以监听按键是否被按下，通过_getch()可以获取按键被按下的ASCII。
2. 鼠标消息
鼠标消息根据按键的不同，通常分为：
- WM_LButtonDown
- WM_LButtonUp
- WM_MouseMove
- WM_RButtonDown
- WM_RButtonUp
- WM_Wheel

### 1.3. 消息驱动
主流的操作系统(Windows/Linux/MacOS)会建立一个消息队列，系统中所有触发的消息都会被推送到消息队列中，程序会从消息队列中依次取出自己的消息，并加以处理和执行。如果你想让程序执行某个指令，只需要向该程序发送响应的消息即可，相应程序会自行处理该消息。

## 2. 如何处理消息
- 视频演示链接[MouseMsg](https://www.bilibili.com/video/BV1yL411W7kR?share_source=copy_web)
<iframe src="//player.bilibili.com/player.html?aid=461617608&bvid=BV1yL411W7kR&cid=367800691&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
---
回到之前的程序，来观察程序如何处理消息的。消息处理的部分变的越来越复杂，因此，我们将消息处理的过程封装为类对象Commander，提供Run()函数来完成消息的处理。
```c++
int main()
{
	initgraph(1280, 720);

	//
	LoadImageUI();
	
	// 消息处理
	Commander	cmd;
	cmd.Run();

	closegraph();
	return 0;
}
```

为了区分左侧和右侧的消息，我们将消息处理函数分为onMenuMsg() 和 onDrawMsg() 分别来处理左侧按钮消息和右侧绘制的消息。

### 2.1 Commander类的设计
-  Commander.h
```c++
#pragma once
#include <graphics.h>
class Commander
{
public:
	Commander();
	void Run();

protected:
	inline bool isExit() const { return m_bExit; }
	
	void onMenuMsg(const MOUSEMSG& msg);
	void onDrawMsg(const MOUSEMSG& msg);
private:
	bool	m_bExit;
};
```

- Commander.cpp
```c++
#include "Commander.h"
Commander::Commander()
{
	m_bExit = false;
}

// 运行，实质上是一个循环，只有点击退出消息才退出循环，即退出程序
void Commander::Run()
{
	MOUSEMSG msg;		//定义变量，保存鼠标消息
	FlushMouseMsgBuffer();// 清空鼠标消息缓冲区，避免无效鼠标信息带入到正式判断中
	
	while (!m_bExit)	//一直循环
	{
		while (MouseHit())	//检查鼠标消息
		{
			msg = GetMouseMsg();	//鼠标消息状态

			if (msg.x >= 0 && msg.x <= 200)
				onMenuMsg(msg);
			else
				onDrawMsg(msg);

			if (m_bExit) break;	//退出循环
		}
	}
}

void Commander::onMenuMsg(const MOUSEMSG& msg)
{
	switch (msg.uMsg)
	{
	case WM_LBUTTONDOWN:	//左键按下
		if (x <= 160 && x >= 40)	//左侧按钮
		{
			if (y >= 35 && y <= 65)	MessageBox(GetHWnd(), "Load Image", "Info", MB_ICONINFORMATION);
			else if (y >= 80 && y <= 110) MessageBox(GetHWnd(), "Load Vector", "Info", MB_ICONINFORMATION);
			else if (y >= 125 && y <= 155) MessageBox(GetHWnd(), "New Vector", "Info", MB_ICONINFORMATION);
			// 根据鼠标位置，执行相应的函数...
		}
		break;
			
		// 省略...
	}
}

void Commander::onDrawMsg(const MOUSEMSG& msg)
{
	//暂时不处理
}
```

### 2.2 细分onMenuMsg
在处理onMenuMsg()时，我们需要有2个判别：**①鼠标消息的类型(LBUTTONDOWN, MOUSEMOVE, RBUTTONUP等)；②哪个按钮被点击。**因此，我们需要在Commander类中增加一个函数，来根据鼠标消息的坐标判别是哪个按钮被点击。

如何来记录被点击的控件呢，需要对控件编号，比如：加载底图是0，加载矢量是1，新建矢量是2，依次编号。程序中为了方便记忆和理解，我们就需要将控件编号声明为枚举，这就有了Commander.h中定义枚举。
- Commander.h
```c++
//定义控件编号枚举，只定义了部分
enum eCtrlID {
	IDC_NULL,		//null command
	IDC_LOADIMAGE,	//Load Image
	IDC_LOADVECTOR,	//Load Vector
	IDC_NEWVECTOR,	//New Vector
	IDC_SAVE,		//Save Vector
	IDC_DRAWPOINT,	//Draw Point
	IDC_DRAWLINE,	//Draw Line
	IDC_DRAWPOLYGON,	//Draw Polygon
	IDC_MOVE,		//Move
	IDC_DEL,			//Del
	IDC_MODIFY,		//Modify
	IDC_EXIT = 999	//Exit
};
protected:
	eCtrlID CheckCtrlID(int x, int y);	//根据鼠标位置，返回单击的按钮ID
```

- Commander.cpp
```c++
eCtrlID MenuCommander::CheckCtrlID(int x, int y)// 根据坐标返回CommandID
{
	eCtrlID	curCmdID = IDC_NULL;

	if (x <= 160 && x >= 40)	//左侧按钮
	{
		if (y >= 35 && y <= 65)	curCmdID = IDC_LOADIMAGE;
		else if (y >= 80 && y <= 110) curCmdID = IDC_LOADVECTOR;
		else if (y >= 125 && y <= 155) curCmdID = IDC_NEWVECTOR;
		else if (y >= 170 && y <= 200) curCmdID = IDC_SAVE;

		else if (y >= 250 && y <= 280) curCmdID = IDC_DRAWPOINT;
		else if (y >= 295 && y <= 325) curCmdID = IDC_DRAWPOLYGON;
		else if (y >= 340 && y <= 370) curCmdID = IDC_DRAWLINE;

		else if (y >= 415 && y <= 445) curCmdID = IDC_MOVE;
		else if (y >= 460 && y <= 490) curCmdID = IDC_DEL;
		else if (y >= 505 && y < 535) curCmdID = IDC_MODIFY;
		else if (y >= 585 && y <= 615) curCmdID = IDC_EXIT;

		else curCmdID = IDC_NULL;
	}

	return curCmdID;
}
```
- onMenuMouseMessage处理函数
```c++
void Commander::onMenuMouseMessage(const MOUSEMSG& msg)
{
	switch (msg.uMsg)
	{
	case WM_LBUTTONDOWN:	//左键按下
		onMenuLButtondown(msg);
		break;
	default:
		Sleep(10);	//休眠10ms
		break;
	}
}

//-----------------------------------------------------------------------//
// 执行对应的Command函数
// 参数：eWndCmd cmd - CommandID
// 返回值： bool - 若退出返回false，其他返回true
//-----------------------------------------------------------------------//
void Commander::onMenuLButtondown(const MOUSEMSG& msg)
{
	clearrectangle(20, 640, 100, 660);

	// Check Command ID
	eCtrlID	curCtrlID = CheckCtrlID(msg.x, msg.y);
	switch (curCtrlID)
	{
	case IDC_EXIT:
		outtextxy(20, 640, "Exit");
		m_bExit = true;
		break;
	case IDC_LOADIMAGE:
		outtextxy(20, 640, "Load Image");
		break;
	case IDC_LOADVECTOR:
		outtextxy(20, 640, "Load Vector");
		onLoad();
		break;
	case IDC_NEWVECTOR:
		outtextxy(20, 640, "New Vector.");
		break;
	case IDC_SAVE:
		outtextxy(20, 640, "Save");
		break;
	case IDC_DRAWPOINT:	//Draw Point
		outtextxy(20, 640, "Draw Point");
		break;
	case IDC_DRAWLINE:	//Draw Line
		outtextxy(20, 640, "Draw Line");
		break;
	case IDC_DRAWPOLYGON:	//Draw Polygon
		setEditType(ET_PLY);
		outtextxy(20, 640, "Draw Polygon");
		break;
	case IDC_MOVE:	//Move
		outtextxy(20, 640, "Move Shape");
		break;
	case IDC_DEL:			//Del
		outtextxy(20, 640, "Delete Shape");
		break;
	case IDC_MODIFY:		//Modify
		outtextxy(20, 640, "Modify Setting");
		break;
	default:
		m_bExit = false;
		break;
	}
}
```

## 3. 小结
至此，上述代码实现了鼠标消息的监听和处理。其流程为：
1. main()函数通过MouseHit()监听鼠标消息；
2. 使用GetMouseMsg()来获取消息参数；
3. 根据鼠标消息的位置判断点击位置的控件编号
4. 根据消息的类型和控件编号，进入相应的处理函数。
此处为了简单起见，在界面的左下角输出了菜单的名称，以此测试鼠标消息的响应是否正常。

---

Go to | [Home](../README.md) | [Head](#消息响应) | [DrawMsg->Next](./D3_DrawMsg.md) | [MainFrame<-Prev](./D1_MainFrame.md) |

