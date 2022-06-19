# 图形文件IO

当鼠标左键单击【加载矢量】按钮时，将触发onLoad()函数。
当鼠标左键单击【保存矢量】按钮时，将触发onSave()函数。
   
## 1. Load
- onLoad()的执行过程为：
1. 弹出输入文件路径的对话框，用于输入文件路径
2. 打开ifstream对象
3. 调用>>函数，读取Shapeset对象
4. 显示ShapeSet对象

- shape文件格式为：
```
0	10	100
1	4	636	311	688	228	918	307	1071	253	
2	4	856	461	1006	339	1021	370	872	490	
```
- onLoad()函数
```c++
void Commander::onLoad()
{
	char		sShapeFilePath[260] = "";
	if (!InputBox(sShapeFilePath, 260, "Vector File: ", "Input Vector File Path."))
		return;

	ifstream	ifs(sShapeFilePath);
	if (!ifs.is_open())
	{
		MessageBox(NULL, "Open File Failed.", "ERROR...", MB_ICONERROR);
		return;
	}

	m_shapeSet.Clear();	//清空已有的图形数据

	ifs >> m_shapeSet;	//读取图形
	ifs.close();

	m_shapeSet.Draw();	//绘制图形
}

```
  
## 2. Draw
图形的绘制函数，均调用每个图形各自的Draw()函数，其整体绘制见ShapeSet::Draw()
- ShapeSet::Draw
```c++
	void ShapeSet::Draw() const
	{
		for (auto& pshp : m_lstShape)
		{
			if (pshp)	pshp->Draw();
		}
	}
```

显示一条线段和一个圆的示意图如下所示：
![Display](https://i.loli.net/2021/06/08/kDGx3uJA7eFqXo2.png)

## 3. Save
与onLoad()函数类似，onSave()函数实现图形数据的写文件。
```c++
void Commander::onSave()
{
	char		sShapeFilePath[260] = "";
	if (!InputBox(sShapeFilePath, 260, "Vector File: ", "Save Vector File Path."))
		return;

	ofstream	ofs(sShapeFilePath);
	if (!ofs.is_open())
	{
		MessageBox(NULL, "Open File Failed.", "ERROR...", MB_ICONERROR);
		return;
	}

	ofs << m_shapeSet;

	ofs.close();
}
```
---

Go to | [Home](../README.md) | [Head](#图形文件IO) | [ShapeEdit->Next](./D7_ShapeEdit.md) | [ShapeSets<-Prev](./D5_ShapeSets.md) |

