# Day 5. EasyX显示图像的基本步骤

> 全部代码在main.cpp中完成
## 0. 安装EasyX库
从[EasyX](http://easyx.cn)下载EasyX安装程序，安装即可。

## 1. 增加头文件
```c++
include <graphics.h>	//使用EasyX图形绘制和IMAGE对象
```

## 2. 增加前向声明
// 前向声明，显示图像
void DisplayImage(CRSImage* pRSImg);
```

## 3. 实现 DisplayImage函数
```c++
//////////////////////////////////////////////////////////////////////////
// DisplayImage - 显示图像到控制台窗口									//
// CRSImage* pRSImg - 图像数据指针										//
// 返回值 - void														//
//////////////////////////////////////////////////////////////////////////
void DisplayImage(CRSImage* pRSImg)
{
	// 参数判断
	if (pRSImg==NULL)
		return;
	
        initgraph(m_nSamples, m_nLines);

	int		i, j;	
	int		nBands = pRSImg->GetBands();		//波段
	int		nSamples = pRSImg->GetSamples();	//列
	int		nLines = pRSImg->GetLines();		//行
	unsigned char***	pppData = pRSImg->GetDataBuffer();	//三维数组头指针
	unsigned char**		ppRed = pppData[0];		//红色通道的波段
	unsigned char**		ppGrn = nBands>2 ? pppData[1] : pppData[0];	//绿色通道的波段
	unsigned char**		ppBlu = nBands>2 ? pppData[2] : pppData[0];	//蓝色通道的波段

        // 创建EasyX IMAGE对象
	IMAGE	img(m_nSamples, m_nLines);
	DWORD*	pImgBuffer = GetImageBuffer(&img);

	// 遍历像素，显示图像
	for (i = 0; i < m_nLines; i++)
	{
		for (j = 0; j < m_nSamples; j++)
		{
			pImgBuffer[i*m_nSamples + j] = RGB(m_pppData[nRedBand][i][j], m_pppData[nGrnBand][i][j],
				m_pppData[nBluBand][i][j]);
		}
	}

	// 绘制影像
	putimage(0, 0, &img);

	// 等待用户按 ESC，退出显示窗口
	while (!_kbhit())
	{
		if (_getch() == 27)
			break;
	}

	// 关闭绘制窗体
	closegraph();
}
```

## 4. 调用Display函数

```c++
// 在图像文件打开之后，就可以调用图像显示
...
  case 'O':
	...
	rsImg.Open(...);
	if (rsImg.IsOpen())
		DisplayImage(&rsImg);
	break;
```
你若有兴趣，可以修改DisplayImage的接口，实现图像的定制化显示，如：波段组合等。
