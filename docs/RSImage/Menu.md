# 实习<二>显示遥感图像
通过实习<一>的程序，我们已经能够掌握常用图像的IO、显示。本次实习，我们的重点是读取遥感图像，并将其显示在imageviewer中。

## 目录
- [1. CRSImage回顾](#1_CRSImage回顾)
- [2. 扩展imageviewer对RSImage的支持](#2_扩展ImageViewer对RSImage的支持)

## 1. CRSImage回顾
以CRSImage的代码为例，之前我们的CRSImage类中，已经完成了图像文件的读取，读取的图像数据放置在一个三维的素组m_pppData中。

```c++
class CRSImage
{
public:
	typedef unsigned char	DataType;	//typedef 定义数据类型别名

	//------------------- 构造/析构函数 -----------------------//
	CRSImage();
	CRSImage(const CRSImage& img);
	~CRSImage();

	//-------------------------- Operations ------------------------------//
	bool	Open(const char* lpstrPath);	//打开文件，传入文件路径，读取数据到内存变量
	
	//--------------- 内联函数，获取图像属性值 --------------------//
	inline DataType*** GetDataBuffer() const {return m_pppData;}

protected:
	// 读文件
	bool	ReadMetaData(const char* lpstrMetaFilePath);	//元数据文件
	bool	InitBuffer(void);								//初始化内存
	bool	ReadImgData(const char* lpstrImgFilePath);		//读图像文件

protected:
	//--------------------- 成员变量 --------------------------//
	DataType***		m_pppData;		//指针记录三维数组首地址
	int				m_nBands;		//波段数
	int				m_nLines;		//行数
	int				m_nSamples;		//列数
	INTERLEAVE_TYPE m_eInterleave;	//数据存储类型BSQ/BIL/BIP
	short       m_nDataType;		//数据类型
};
```

## 2. 扩展ImageViewer对CRSImage的支持
### 2.1 图像显示代码分析
分析iamgeviewer工程不难发现，图像显示的代码集中在imageviewer类中，所涉及的函数主要有：loadFile和setImage。

- loadFile()
loadFile函数的意思，显而易见是读取图像文件，我们分析代码发现，Qt提供了一个类QImageReader类，调用read()函数，可以返回一个QImage对象。然后将调用setImage()即完成了图像的显示。源代码如下：

```c++
bool ImageViewer::loadFile(const QString &fileName)
{
	// QImageReader负责传入文件名，调用read，即返回QImage对象
	// 试想，CRSImage可否担当此功能呢？？
    QImageReader reader(fileName);
    reader.setAutoTransform(true);
    const QImage newImage = reader.read();	//返回QImage对象
    if (newImage.isNull()) {
        QMessageBox::information(this, QGuiApplication::applicationDisplayName(),
                                 tr("Cannot load %1: %2")
                                 .arg(QDir::toNativeSeparators(fileName), reader.errorString()));
        return false;
    }

    setImage(newImage);	//绘制QImage

	// 更新有关图像的状态信息
    setWindowFilePath(fileName);	//更改标题

    const QString message = tr("Opened \"%1\", %2x%3, Depth: %4")
        .arg(QDir::toNativeSeparators(fileName)).arg(image.width()).arg(image.height()).arg(image.depth());
    statusBar()->showMessage(message);	//更改状态栏
    return true;
}
```

- setImage()
setImage负责将QImage对象绘制在imageLabel上，代码如下：

```c++
void ImageViewer::setImage(const QImage &newImage)
{
    image = newImage;	//QImage
    imageLabel->setPixmap(QPixmap::fromImage(image));	//QImage -> QPixmap
//! [4]
    scaleFactor = 1.0;	//显示比例

	// 更新相关的UI状态
    scrollArea->setVisible(true);
    printAct->setEnabled(true);
    fitToWindowAct->setEnabled(true);
    updateActions();

    if (!fitToWindowAct->isChecked())
        imageLabel->adjustSize();
}
```

### 2.2 如何嵌入CRSImage
从上面的分析可知，嵌入CRSImage的关键是，将CRSImage读入的图像数据（三维数组）转换为QImage。

```c++
void CImageDisplayDlg::ShowRaster()  
{  
     int band_list[3] = {1,2,3};  
	int		iImageWidth;		//图像的宽度
	
    iImageWidth = (iImageWidth*8+31)/32*4;  //按4字节对齐...
  
	// 图像数据
    unsigned char* pBuffer = new unsigned char[iScaleWidth*iScaleHeight*dataBands];  
   
	// 波段组合。。。
	unsigned char* pDataBuffer = NULL;  
    if (dataBands >=3 )  
    {  
        pDataBuffer = pBuffer;  
    }  
    else  
    {  
        pDataBuffer = new unsigned char[iImageWidth*iImageHeight*3];  
        for (int i=0; i<iImageWidth*iImageHeight*3; i++)  
            pDataBuffer[i] = pBuffer[i/3];  
  
        delete []pBuffer;  
    }  
  
	// 构建QImage对象
    QImage QImg(pDataBuffer, iImageWidth, iImageHeight, QImage::Format_RGB888);  
}  
```

---

|[Home](https://cugwhp.github.io/OOPCPP/CourseDesign/CourseDesignNew.html#%E8%AF%BE%E8%AE%BE%E5%86%85%E5%AE%B9) | [Return](#目录)| [Prev](./D1_Frame.md) | [Next](./D3_Information.md)|