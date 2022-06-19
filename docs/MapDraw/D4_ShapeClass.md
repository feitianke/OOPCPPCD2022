# 形状类的设计
在上一节中，需要创建Line对象存放线，程序还需要存放Point, Polygon对象，因为Point, Polygon和Line继承于Shape类，便于类的接口统一。

- 视频链接[ShapeClass](https://www.bilibili.com/video/BV1Sy4y1K7k7?share_source=copy_web)
<iframe src="//player.bilibili.com/player.html?aid=804069056&bvid=BV1Sy4y1K7k7&cid=367800938&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
---

## 1. 类的继承关系
点、线、面是同一类，具有相似的行为，如：Draw, Perimeter, Area等，其类的继承关系为图为：
![Shape_class](https://i.loli.net/2021/07/11/KWPdqH1VabMj59n.png)
<!--![Shape_class](../../png/Shape_class.png)-->

```
类的关系图描述了Point, Line, Polygon 均从Shape基类继承而成。同时Line和Polygon对象中还包含Point对象，描述的是组合关系。
考虑到Draw函数的差异性，将Draw函数抽象为Shape的虚函数，事实上，可以抽象为纯虚函数，这样Shape就变成了抽象类。
```

## 2. 类的接口设计

分析程序的需求，需要Shape类提供什么接口呢？有无统一的接口呢？如有统一的接口，则需要添加为基类的虚函数。

```
记住：程序的设计阶段无需考虑编码的实现，此处专注的是功能接口的参数定义、返回值等。
```

可能的接口包括：

- Draw - 绘制图形
- Area - 计算面积
- Perimeter - 周长
- read - 从文件读
- write - 写入文件
- Move - 移动坐标
- ...

## 2.1 Shape类
Shape类设计为抽象类，具有纯虚函数，这样的目的是为了统一接口，统一接口的好处在随后的实现中会体现出来，大家也可以在代码实现后好好阅读和体会。
- Shape.h
```c++
#include <iostream>
#include <graphics.h>
using namespace std;

namespace MapDraw {	//名字空间，解决类的同名冲突
	class Shape
	{
		friend ostream& operator<<(ostream& os, const Shape& shp);
		friend istream& operator>>(istream& is, Shape& shp);
	public:
		Shape();
		virtual ~Shape() = default;

		virtual void Draw() = 0;				
		virtual void Move(int dx, int dy) = 0;

	protected:
		virtual bool write(ostream& os) const = 0;	//纯虚函数
		virtual bool read(istream& is) = 0;			//纯虚函数

		COLORREF	m_penClr;	//画笔颜色
		COLORREF	m_fillClr;	//填充颜色
		int			m_nSize;	//大小
	};

	ostream& operator<<(ostream& os, const Shape& shp);
	istream& operator>>(istream& is, Shape& shp);
}
```
- Shape.cpp

```c++
namespace MapDraw {
	Shape::Shape()
	{
		m_penClr = RED;	//画笔颜色
		m_fillClr = GREEN;	//填充颜色
		m_nSize = 1;	//大小
	}
	
	// 注意此处的operator<< 和operator>>重载函数，调用的是write和read虚函数，是为了实现多态。
	ostream& operator<<(ostream& os, const Shape& shp)
	{
		shp.write(os);
		return os;
	}

	istream& operator>>(istream& is, Shape& shp)
	{
		shp.read(is);
		return is;
	}
}
```
### 2.2 Point类
Point类实现了write 和 read 函数，其目的是从输入流读入x，y坐标，将x,y写出到输出流。点文件的格式为：
```
0 10 10
点类型 x y
```
- Point.h
```c++
namespace MapDraw {
	class Point : public Shape
	{
	public:
		Point(int x = 0, int y = 0);
		~Point() = default;

		void Draw() override;
		void Move(int dx, int dy) override;

	protected:
		bool write(ostream& os) const override;
		bool read(istream& is) override;

	protected:
		int			m_x, m_y;
	};
}
```
- Point.cpp
```c++
#include "Point.h"

namespace MapDraw {
	Point::Point(int x, int y) : Shape(), m_x(x), m_y(y)
	{
	}

	void Point::Draw()
	{
		putpixel(m_x, m_y, RED);
	}

	void Point::Move(int dx, int dy)
	{
		m_x += dx;
		m_y += dy;
	}

	bool Point::write(ostream& os) const
	{
		os << 0 << "\t" << m_x << "\t" << m_y << endl;
		return true;
	}

	//此处没有读入点类型0，为何？？
	bool Point::read(istream& is)
	{
		is >> m_x >> m_y;
		return true;
	}
}
```
### 2.3 Line类
与点的类类似，Line类存储线的坐标，其数据存储样式为：
```
1	3	10	0	20	20	50	5
点类型 节点数 x1 y1 x2 y2 x3 y3
```
此处重点关注，Line类的write()和read()。
- Line.h
```c++
#include "Shape.h"
#include "Line.h"
#include <vector>
#include <initializer_list>
using namespace std;

namespace MapDraw {
	class Line : public Shape
	{
	public:
		Line(POINT* const pts = nullptr, int n = 0);
		Line(const vector<POINT>& pts);
		Line(const initializer_list<POINT>& pts);
		~Line();

		void Draw() override;
		void Move(int dx, int dy) override;

	protected:
		bool write(ostream& os) const override;
		bool read(istream& is) override;

	protected:
		vector<POINT>	m_nodes;
	};
}
```

- Line.cpp
```c++
#include "Line.h"

namespace MapDraw {
	Line::Line(POINT* const pts, int n)
	{
		for (int i = 0; i < n; ++i)
		{
			m_nodes.push_back(pts[i]);
		}
	}

	Line::Line(const vector<POINT>& pts) : m_nodes(pts)
	{
	}

	Line::Line(const initializer_list<POINT>& pts) : m_nodes(pts)
	{
	}

	Line::~Line()
	{
	}

	void Line::Draw()
	{
		polyline(m_nodes.data(), m_nodes.size());
	}

	void Line::Move(int dx, int dy)
	{
		return;
	}

	bool Line::write(ostream& os) const
	{
		os << 1 << "\t" << m_nodes.size() << "\t";
		for (auto& pt : m_nodes)
			os << pt.x << "\t" << pt.y << "\t";
		os << endl;
		return true;
	}

	bool Line::read(istream& is)
	{
		size_t	ncount;
		POINT	pt;
		is >> ncount;
		for (int i = 0; i < ncount; ++i)
		{
			is >> pt.x >> pt.y;
			m_nodes.push_back(pt);
		}
		return true;
	}
}
```

### 2.4 Polygon类
Polygon类与Line类差别不大，区别是要首尾封闭。为了简化代码，Polygon类从Line类继承，在Draw()和write()函数略有不同，注意观察二者之间的细微差别。
- Polygon.h
```c++
#include "Line.h"
#include <vector>
using namespace std;

namespace MapDraw {
	class Polygon : public Line
	{
	public:
		Polygon(POINT* const pts = nullptr, int n = 0);
		Polygon(const vector<POINT>& pts);
		Polygon(const initializer_list<POINT>& pts);
		~Polygon() = default;

		void Draw() override;
	protected:
		bool write(ostream& os) const override;
	};
}
```
- Polygon.cpp
```c++
#include "Polygon.h"

namespace MapDraw {
	Polygon::Polygon(POINT* const pts, int n) : Line(pts, n)	{

	}

	Polygon::Polygon(const vector<POINT>& pts) : Line(pts)
	{
	}

	Polygon::Polygon(const initializer_list<POINT>& pts) : Line(pts)
	{
	}

	void Polygon::Draw()
	{
		polygon(m_nodes.data(), m_nodes.size());
	}

	bool Polygon::write(ostream& os) const
	{
		os << 2 << "\t" << m_nodes.size() << "\t";
		for (auto& pt : m_nodes)
			os << pt.x << "\t" << pt.y << "\t";
		os << endl;
		return true;
	}
}
```
## 3. 小结
通过此节的内容，可实现点、线、多边形的存取。体会设计虚函数的目。只有单个的shape对象，还不足以存储程序绘制的多个形状，如何存储多个形状呢，下一节介绍ShapeSet类，用于组织多个形状序列。

Go to | [Home](../README.md) | [Head](#形状类的设计) | [ShapeSets->Next](./D5_ShapeSets.md) | [DrawMsg<-Prev](./D3_DrawMsg.md) |

