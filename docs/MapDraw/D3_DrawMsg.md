# 图形绘制消息

[TOC]

- 视频演示[DrawMsg](https://www.bilibili.com/video/BV1zy4y1K7NJ?share_source=copy_web)
<iframe src="//player.bilibili.com/player.html?aid=804031062&bvid=BV1zy4y1K7NJ&cid=367800860&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
---

## 1. 消息响应
上一节重点介绍了onMenuMouseMsg()的处理过程，此节来详细介绍图形绘制的消息处理。在Commander::Run()中，根据鼠标的位置，区分了onMenuMouseMsg()和onDrawMouseMsg()。简而言之，就是当鼠标在左侧活动时，由onMenuMouseMsg()来处理鼠标消息，鼠标在右侧活动时，由onDrawMouseMsg()处理鼠标消息。处理onDrawMouseMsg()需要的前提条件是：
1. 当前编辑的状态：输入点、输入线、输入多边形、删除、移动、修改参数等。
2. 当前鼠标消息的类型：MOUSEMOVE, LBUTTONDOWN, LBUTTONUP, RBUTTONUP等

### 1.1 编辑状态
编辑状态是当鼠标点击了【输入点】按钮后改变的，因此，需要在Commander中增加成员变量，记录编辑状态，由上的叙述可知，编辑状态可以定义为枚举变量。Commander.h中新增的代码为：
```c++
// 编辑状态
enum eEditType {	//
	ET_NULL,	//无编辑状态
	ET_PNT,		//输入点
	ET_LIN,		//输入线
	ET_PLY,		//输入多边形
	ET_MOVE,	//移动
	ET_DEL,		//删除
	ET_MODIFY,	//修改
	ET_LAST		//结束标志
};

class Commander {
private:
    eEditType   m_nEditType;    // 编辑类型
};
```
m_nEditType的修改在onMenuLButtondown()函数中，代码片段如下：
```c++
	case IDC_DRAWPOINT:	//Draw Point
		m_nEditType = ET_PNT;
		outtextxy(20, 640, "Draw Point");
		break;
	case IDC_DRAWLINE:	//Draw Line
		m_nEditType = ET_LIN;
		outtextxy(20, 640, "Draw Line");
		break;
	case IDC_DRAWPOLYGON:	//Draw Polygon
		m_nEditType = ET_PLY;
		outtextxy(20, 640, "Draw Polygon");
		break;
	case IDC_MOVE:	//Move
		m_nEditType = ET_MOVE;
		outtextxy(20, 640, "Move Shape");
		break;
	case IDC_DEL:			//Del
		m_nEditType = ET_DEL;
		outtextxy(20, 640, "Delete Shape");
		break;
	case IDC_MODIFY:		//Modify
		m_nEditType = ET_MODIFY;
		outtextxy(20, 640, "Modify Shape Settings");
		break;
```

### 1.2 onDrawMouseMsg
- Commander.cpp

新增onDrawLButtondown(), onDrawLButtonup(), onDrawMousemove(), onDrawRButtonup()消息处理函数
```c++
void Commander::onDrawMouseMessage(const MOUSEMSG& msg)
{
	switch (msg.uMsg)
	{
	case WM_LBUTTONDOWN:	//左键按下
		onDrawLButtondown(msg);
		break;
	case WM_LBUTTONUP:
		onDrawLButtonup(msg);
		break;
	case WM_MOUSEMOVE:
		onDrawMousemove(msg);
		break;
	case WM_RBUTTONUP:
		onDrawRButtonup(msg);
		break;
	default:
		Sleep(10);	//休眠10ms
		break;
	}
}
```

## 2. 如何绘制线
基于消息驱动的程序，在完成一个功能时，于控制台程序的执行方式有着绝然不同的方式。以输入线流程为例：
- 1. 鼠标左键按下，记录第一个节点的坐标
- 2. 鼠标移动，连接上一个鼠标按下节点的坐标
- 3. 鼠标再次按下左键，记录第二个节点的坐标
- 4. 重复第2-3步，添加节点
- 5. 鼠标右键抬起，结束线的绘制
从绘线的流程可以看出，涉及到WM_LBUTTONDOWN, WM_MOUSEMOVE, WM_RBUTTONUP这三个消息。简而言之，就是绘线的过程，被上述3个消息打断了，由他们组合、配合而成。

### 2.1 onDrawLButtondown
当鼠标按下的时候，程序需要如何响应呢？从上述的绘制线的流程可知，需要完成：
- 1. 记录当前点的坐标
- 2. 如何当前点是第2,...,n个节点，需要将当前点和前面点连接成线
基于上述的描述，需要在Commander类中增加成员变量记录鼠标单击的点序列。
- Commander.h
```c++
class Commander{
private:
    vector<POINT>   m_ptList;   //点序列，每次单击就追加一个
    POINT           m_ptPrev;   //鼠标移动前鼠标的位置
};
```
- Commander.cpp
```c++
// 鼠标左键按下
void Commander::onDrawLButtondown(const MOUSEMSG& msg)
{
	m_bPressed = true;

	switch (m_nEditType)
	{
	case ET_PNT:
		//...
		break;
	case ET_LIN:
	case ET_PLY:
	{
		if (m_ptList.size() > 0)
		{
			line(m_ptList.rbegin()->x, m_ptList.rbegin()->y, msg.x, msg.y);
		}
		m_ptPrev.x = msg.x;	//记录鼠标位置
		m_ptPrev.y = msg.y;
		m_ptList.push_back(m_ptPrev);
		break;
	}
	case ET_NULL:
		break;
	default:
		break;
	}
}
```

### 2.2 onDrawMouseMove
为了绘制鼠标跟踪线，需实时连接鼠标左键按下时点的坐标和当前鼠标位置。因为在鼠标移动过程中，绘制的线是临时的，需要将上次绘制的临时线删除，此时用NOTXOR的方式来绘制。绘制过程为：
- 第一次是把上次绘制的线重绘一遍，把上次绘制的线擦除；
- 第二次才是当前鼠标位置和上一个节点的连线。

示例代码片段为：
```c++
void Commander::onDrawMousemove(const MOUSEMSG& msg)
{
	switch (m_nEditType)
	{
	case ET_PNT:
		break;
	case ET_LIN:
	case ET_PLY:
		if (m_ptList.size()>0)
		{
			int	oldmode = getrop2();
			setrop2(R2_NOTXORPEN);	//异或线

            // 擦除之前的线，再绘制一条线
			line(m_ptList.rbegin()->x, m_ptList.rbegin()->y, m_ptPrev.x, m_ptPrev.y);
			line(m_ptList.rbegin()->x, m_ptList.rbegin()->y, msg.x, msg.y);
			m_ptPrev.x = msg.x;
			m_ptPrev.y = msg.y;

			setrop2(oldmode);		//绘制模式还原
		}
		break;
	default:
		break;
	}
}
```
### 2.3 onDrawRBttonup
右键抬起，结束线的输入。此处要完成如下工作：
1. 将输入的点序列m_ptList（坐标数组）创建一个线对象，存储起来；
2. 擦除onDrawMousemove最后绘制的一条异或线
3. 清空m_ptList的点序列，输入状态恢复到点击【输入线】的状态
```c++
// 鼠标右键抬起
void Commander::onDrawRButtonup(const MOUSEMSG& msg)
{
	int			oldmode = getrop2();
	switch (m_nEditType)
	{
	case ET_LIN:
		if (m_ptList.size() > 0)
		{   //擦除onDrawMousemove绘制的异或线
			setrop2(R2_NOTXORPEN);	//异或线
			line(m_ptList.rbegin()->x, m_ptList.rbegin()->y, msg.x, msg.y);
			setrop2(oldmode);		//绘制模式还原
		}
		if (m_ptList.size() > 1) //至少2个节点
			m_shapeSet.Append(new Line(m_ptList));  //创建一个Line对象存储起来
		break;
	case ET_PLY:
		//...
		break;
	default:
		break;
	}

	m_ptList.clear();	//清空缓存节点
}
```

## 3.小结
至此，上述代码实现了在右侧图像窗口，通过鼠标单击、移动和右键抬起等操作，完成了一条线的输入。有关Line对象的描述，见下一节。此节达到的效果为：
1. 鼠标点击左侧【输入线】按钮，onMenuMouseMsg()能记录编辑状态
2. 鼠标在右侧图像部分，程序能处理onDrawLButtondown(), onDrawMousemove(), onRButtonup()消息，共同协作完成线的绘制。

---

Goto | [Home](../README.md) | [Head](#图形绘制消息) | [ShapeClass->Next](./D4_ShapeClass.md) | [MouseMsg<-Prev](./D2_MouseMsg.md) |