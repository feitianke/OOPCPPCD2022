# 矢量图形的编辑
上节实现了图形的IO，此处简要介绍图形的编辑功能，仅提供实现思路，实现与否不做特殊要求。
此处的图形移动、搜索和参数修改均少不了“选中”操作，何为选中，即根据鼠标的位置，搜索在鼠标附近的图形对象。如：
- 点的搜索：在鼠标的一定范围内(如：2个像素内)
- 线的搜索：线穿过鼠标所在位置的一定范围内(可以鼠标位置外扩一定范围的矩形区域)
- 多边形的搜索：多边形与鼠标所在位置的矩形范围相交，即为选中
选中图形对象后，完成图形对象的移动、删除等操作，只需调用响应接口即可。

## 1. 搜索
### 1.1. 搜索以当前点位置外扩形成一个矩形，判断有哪些图形与该搜索矩形相交即可
- ShapeSet.h
```c++
class ShapeSet{
	protected:
		list<Shape*>::iterator Search(const POINT& pt);
		list<Shape*>::iterator Search(const RECT& rect);
}
```
- ShapeSet.cpp
```c++
	list<Shape*>::iterator ShapeSet::Search(const POINT& pt)
	{
		list<Shape*>::iterator iter;
		for (iter = m_lstShape.begin(); iter != m_lstShape.end(); ++iter)
		{
			if ((*iter)->isIntersect(pt))
				return iter;
		}
		return m_lstShape.end();
	}

	list<Shape*>::iterator ShapeSet::Search(const RECT& rect)
	{
		list<Shape*>::iterator iter;
		for (iter = m_lstShape.begin(); iter != m_lstShape.end(); ++iter)
		{
			if ((*iter)->isIntersect(rect))
				return iter;
		}
		return m_lstShape.end();
	}
```
### 1.2. 判断点或矩形与图形对象相交
- Shape.h
```c++
		virtual bool isIntersect(const POINT& pt) const = 0;
		virtual bool isIntersect(const RECT& rect) const = 0;
```
- Line.h
```c++
		bool isIntersect(const POINT& pt) const override;
		bool isIntersect(const RECT& rect) const override;
```
- Line.cpp
```c++
	bool Line::isIntersect(const POINT& pt) const
	{
        //省略...
        return false;
	}
```

## 2. 移动
1. 搜索当前点附近的图形
2. 鼠标左键按下，移动图形到目的位置，记录dx,dy
3. 移动选中图形的x,y，加上(dx,dy)增量
省略。。。

## 3. 删除
1. 鼠标左键单击，根据搜索范围，搜索最邻近的图形，直接删除。
并未实现图形的实时绘制。。。
```c++
	void ShapeSet::Del(int x, int y, int w, int h)
	{
		RECT	rect;
		rect.left = x;
		rect.top = y;
		rect.right = x + w;
		rect.bottom = y + h;

		list<Shape*>::iterator iter = Search(rect);
		if (iter != m_lstShape.end())
		{
			m_lstShape.erase(iter);
		}
	}
```

## 4. 修改参数
1. 鼠标左键单击图形，搜索邻近的图形，修改参数。
省略。。。

## 5. 小结
至此，本节简述了选中图形对象，并完成相应操作的基本流程。此处有关图形按位置搜索的函数未实现，需要完成者自行搜索相应的算法实现。同时，删除图形后绘制的更新也有待改进。

---

Go to | [Home](../README.md) | [Head](#矢量图形的编辑) | [DebugReport->Next](./D8_DebugReport.md) | [ShapeIO<-Prev](./D6_ShapeIO.md) |