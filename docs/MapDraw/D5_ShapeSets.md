# 形状数据集的构建
ShapeSet是将输入的图形以list容器组织起来，提供图形添加、删除、修改、绘制等操作。

- 视频链接[ShapeSets](https://www.bilibili.com/video/BV1eo4y1X7u6?share_source=copy_web)
<iframe src="//player.bilibili.com/player.html?aid=376624966&bvid=BV1eo4y1X7u6&cid=367801011&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
---

## 1. ShapeSet类
## 1.1. ShapeSet类的定义
ShapeSet以list<Shape*>来存放图形的原因是，Shape*实现了不同形状的统一管理，保证了接口的统一。此处实现的主要函数为：
- Append() - 添加一个shape*到链表
- Clear() - 清空链表中的Shape*对象，注意此处的Shape*需要delete
- Draw() - 绘制每个图形
- operator<< - 输出操作符
- operator>> - 输入操作符
```c++
#pragma once
#include <iostream>
#include <list>
using namespace std;

namespace MapDraw {
	class Shape;

	class ShapeSet
	{
		friend ostream& operator<<(ostream& os, const ShapeSet& shpset);
		friend istream& operator>>(istream& is, ShapeSet& shpset);
	public:
		ShapeSet();
		~ShapeSet();

		void Append(Shape* pshp);
		void Clear();
		void Draw() const;
	private:
		list<Shape*>	m_lstShape;
	};

	ostream& operator<<(ostream& os, const ShapeSet& shpset);
	istream& operator>>(istream& is, ShapeSet& shpset);
}
```

## 1.2. Shapeset类的实现
```c++
#include "ShapeSet.h"
#include "Shape.h"

namespace MapDraw {
	ShapeSet::ShapeSet()
	{

	}

	ShapeSet::~ShapeSet()
	{
		Clear();
	}

	void ShapeSet::Append(Shape* pshp)
	{
		if (pshp)
			m_lstShape.push_back(pshp);
	}

	void ShapeSet::Clear()
	{
		while (!m_lstShape.empty())
		{
			delete (*m_lstShape.rbegin());	//delete
			m_lstShape.pop_back();
		}
	}

	void ShapeSet::Draw() const
	{
		for (auto& pshp : m_lstShape)
		{
			if (pshp)	pshp->Draw();
		}
	}

	ostream& operator<<(ostream& os, const ShapeSet& shpset)
	{
		for (auto& pshp : shpset.m_lstShape)
			os << (*pshp);
		return os;
	}

	istream& operator>>(istream& is, ShapeSet& shpset)
	{
		while (!is.eof())
		{
			Shape*	pshp = Shape::findClone(is);	//findClone的实现见下
			if (pshp)	shpset.Append(pshp);
		}
		return is;
	}
}
```
## 2. Shape::findClone
finClone是一个静态函数，其目的是从输入流中创建一个Shape实体，并通过输入流设置其参数。其具体流程为：
1. 从输入流读入某行的第1个参数
2. 根据参数（0-PNT, 1-LINE, 2-PLY, 其他-错误）new 对应实体
3. 调用对象<<函数，初始化值

```c++
class Shape
		static Shape* findClone(istream& is);
}；
```

```c++

	Shape* Shape::findClone(istream& is)
	{
		int	type;
		Shape*		pshp = nullptr;
		is >> type;
		switch (type)
		{
		case PNT:
			pshp = new Point();
			break;
		case LIN:
			pshp = new Line();
			break;
		case PLY:
			pshp = new Polygon();
			break;
		default:
			pshp = nullptr;
			break;
		}

		if (pshp)
			is >> (*pshp);
		return pshp;
	}
```

- ShapeSet对象作为Commander的成员变量，用于形状的存储。

## 3. 小结
至此，可以打开一个文件，创建多个图形对象，并存放于Shapeset中。同时，也可以将Shapeset中的图形存入文件中。下一节，将介绍如何响应MenuMouseMsg()来读入和存图形文件。

---

Go to | [Home](../README.md) | [Head](#形状数据集的构建) | [ShapeIO->Next](./D6_ShapeIO.md) | [<-Prev](./D4_ShapeClass.md) |

