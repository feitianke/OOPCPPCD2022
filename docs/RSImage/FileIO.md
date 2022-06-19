# Day 2 - Image File IO
> 主要解决从图像文件读取数据到CRSImage对象的过程，遥感图像的文件格式参考[**BSQ/BIL/BIP格式说明**](./RSImageFormat.md)
>

[TOC]

## 目标
> 何为File IO？本质上就是“蚂蚁搬家”，将硬盘（外存）上的数据复制到寄存器（内存）上来。

## 需求分析
>事实上，文件存在硬盘上，也是有地址的，定义CRSImage这样一个类（对象）的Open成员函数，其目的就是要完成蚂蚁搬家的这个事情。
>在遥感图像处理的领域，有个不成文的约定，就是对于每一个数据文件，都有一个对这个遥感图像数据的说明，通常以.hdr为后缀。我们给的数据保护两个文件，一个是数据文件，名字为test.img；另一个是test.hdr，是描述test.img的文件，我们称之为元数据文件。请问，元数据文件test.hdr 和 test.img文件的名称有没有什么关联，知道test.img可否推算出test.hdr？
>我们接下来，回放下“蚂蚁搬家”的过程，搬家之前，是不是要对搬家的对象有所知晓呢？比如你搬家的箱子的大小，多长、多宽，有多重，多多少个箱子等等；搬家之中，用拖车也好，搬家公司也好，你就开始逐个把箱子搬到一个新的地方；这其中，忘了一个细节，就是你在搬家之前，要不要把目的地收拾好，不然，没有位置可以存放这些箱子呢？那么通过这样一个流程，我们就可以类比读文件的一个过程，1）读取元数据文件，知道图像数据的参数；2）分配内存，为复制图像开辟空间；3）执行复制的过程，一段一段的搬运数据。

## 实现步骤

### 1. CRSImage 的设计

我们为了实现CRSImage::Open()函数，我们的工作是读取文件内容，放置到内存中。那么我们就来先设计CRSImage的成员函数和成员变量。

- 成员函数

  函数 CRSImage::Open 需要确定参数和返回值，我们是要把文件-->内存，显然输入是什么，是文件存放的首地址。在C++中，我们是通过文件路径（字符串）来标识的。那么Open函数补充为：

  ```c++
  CRSImage::Open(const char* lpstrPath);	//const char* 字符串
  ```

  ​这个函数是否需要返回值，显然需要，万一文件损坏，传入的文件路径不正确等都会导致这个函数执行失败，因此这个函数就变为了：
  ```c++
  bool CRSImage::Open(const char* lpstrPath);	//const char* 字符串
  ```

- 成员变量
  因为读的数据要存放起来，自然我们需要分配内存空间存放。但试想，我们在读取元数据之前，是否知道图像的行、列、波段等信息。显然是后面才知道的，故图像数据只能定义在堆空间上，通过new的方式，来分配。我们假定图像是多个波段，用3维数组来描述，并假定数据类型是unsigned char。

  ```c++
  unsigned char***		m_pppData;	//三维数组
  int				m_nBands;			//波段数
  int				m_nLines;			//行数
  int				m_nSamples;			//列数
  ```

- class CRSImage Code
  ```c++
  class CRSImage
  {
  public:
  	CRSImage();
  	~CRSImage();

  	// Operations
  	bool	Open(const char* lpstrPath);
   protected:
    	unsigned char***	m_pppData;	//三维数组
  	int				m_nBands;		//波段数
  	int				m_nLines;		//行数
  	int				m_nSamples;		//列数
  };
  ```

### 2. Open函数的实现

在实现Open函数之前，我们先前考虑的读文件的功能、步骤。我们先写一段类似的伪代码。

```c++
// Operations
bool	CRSImage::Open(const char* lpstrPath)
{
  //0. 参数合法性判断，如果调用者给你一个不合法的文件路径，怎么办，需要先行判断
	if (NULL == lpstrPath)	//文件路径不合法，返回false
		return false;

  // Step1. 读元数据文件，要知道图像数据的行、列、波段等信息
  // Q1：元数据文件是什么？还记得前面讲过的元数据文件路径与图像文件路径的关联吗？
  // Q2: 读取元数据文件，想想就觉得这是一个繁琐而复杂的过程，怎么办呢？我们可以封装一个函数
  if (!ReadMetaData(const char*))	//元数据文件路径
  	return false;		//读文件失败
  
  // Step 2. 分配内存，来存文件
  // 我们给的测试数据是一个6波段的图像，是一个三维数组，三维数组怎么分配内存呢？貌似一两行代码搞不定，怎么办呢？封装函数
  if (!InitBuffer())	//new一个三维数组，记住，有new就有释放，所以一定要考虑清楚释
    return false;
  
  // Step 3. 读图像数据文件，真正地“搬家”
  if (!ReadImgData(lpstrPath))	//读文件
    return false;
  
  // 最后返回成功
	return true;
}
```

从上述的分析来看，Open函数还没写完，就多出了3个辅助函数，来帮助我们完成Open的功能。这样，CRSImage的定义就变成了：

```c++
class CRSImage
{
public:
	CRSImage();
	~CRSImage();

	// Operations
	bool	Open(const char* lpstrPath);
  
  protected:
  	bool	ReadMetaData(const char* lpstrMetaFilePath);
	bool	InitBuffer(void);
	bool	ReadImgData(const char* lpstrImgFilePath);

 protected:
  	unsigned char***	m_pppData;	//三维数组
	int				m_nBands;		//波段数
	int				m_nLines;		//行数
	int				m_nSamples;		//列数
};
```

> Question: 为何要将新增的3个函数设置为protected权限，而不是public权限呢？

### 3. 实现各个子函数

#### 3.1 ReadMetaData 函数

元数据文件是一个明码的文件，即ASCII文件，也称为文本文件。用notepad打开，是可以看到里面的内容是由一个一个字符组成。接下来，我们以ReadMetaData的源码为例，解释下文件流的使用。

- 首先是元数据文件路径的由来

```c++
// Meta Data File Path
// d:\\test.img -> d:\\test.hdr
// d:\\test -> d:\\test.hdr
string		strMetaFilePath = lpstrPath;
//元数据文件路径的推导是：1）检查数据文件中有无字符'.'，我们用的是rfind，意思是从后往前找，如果找到了，就把字符'.'在字符串中所做的index返回出来，没有找到就返回-1；2）index从0开始，取pos个字符，正好取到d:\\test；3）在后面加上.hdr,变成了d:\\test.hdr
int			pos = strMetaFilePath.rfind('.');	//反向查找
if (pos>=0)
{
  strMetaFilePath = strMetaFilePath.substr(0, pos);	//取子串，把'.'之后的字符都删除掉
}
strMetaFilePath.append(".hdr");	//添加".hdr"
```

- 读文本文件

> 在此之前我们先交代几个概念：
>
> 1）流-stream
>
> 事实上，大家对stream并不陌生，用的最多的是iostream，即cout、cin。在此处我们用到另外2类stream，一个是file stream，一个是string stream。
>
> 首先我们理解下cin 作为一种标准的input stream 的过程。比如：int a; cin>>a; 这么简单的代码是如何实现的呢？cin>>a;会等待用户输入一串键盘，回车的时候，传入到变量a中。比如我们在键盘上输入“1”、“2”、“3”然后回车，系统读入的是什么，其实是ASCII码，即“123”字符串，cin充当了一个翻译器，将其转换成了123。既然如此，我们能否将一个字符串“123”转换为123呢？当然可以，因为其原理一样，只是字符串的来源不同罢了，一个来源于键盘的输入，一个来源于字符串。另外一个是file stream，就是我们打开元数据文件test.hdr中看到的内容，也可以直接读取到变量中来。
>
> stream 有一个特点，就是具有缓冲池。每个程序都想往屏幕输出内容，同一时刻都输出来，会互相“打架”，争夺屏幕绘制的权利，这个时候就采用了一种“缓冲池”的机制，就行如大家在12306上买票，你提交了买票的订单，12306后台并没有理解出票，而是有一个排队的过程，然后后台一个一个的处理。这样就避免了同时买票，导致12306服务器崩溃。
>
> 2）file -stream

```c++
/////////////////////////////////////////////////////////////////////
// 读取元数据文件
/////////////////////////////////////////////////////////////////////
bool	CRSImage::ReadMetaData(const char* lpstrMetaFilePath)
{
    ifstream    ifs;		//input file stream
    string      strLine;
    string      strSubTxt;
    stringstream    ss;		//string stream

    char        sBuff[260];

	ifs.open(lpstrMetaFilePath, ios_base::in);
    if (!ifs.is_open())
        return false;

    while(!ifs.eof())   //end of file
    {
        ifs.getline(sBuff, 260);
        strLine = sBuff;

        ss.clear();
        ss.str(strLine);    //"samples = 640"
        ss >> strSubTxt;

        if (strSubTxt == "samples")
        {
            ss >> strSubTxt >> m_nSamples;
        }
        else if (strSubTxt == "lines")
        {
            ss >> strSubTxt >> m_nLines;
        }
        else if (strSubTxt == "bands")
        {
            ss >> strSubTxt >> m_nBands;
        }
        else if (strSubTxt == "interleave")
        {
            ss >> strSubTxt >> strSubTxt;
            if (strSubTxt == "bsq")
            {
                m_eInterleave = BSQ;
            }
            else if (strSubTxt == "bip")
            {
                m_eInterleave = BIP;
            }
            else if (strSubTxt == "bil")
            {
                m_eInterleave = BIL;
            }
            else
            {
                // blank
            }
        }
        else if (strSubTxt == "data")
        {
            ss >> strSubTxt;
            if(strSubTxt == "type")
            {
                ss >> strSubTxt >> m_nDataType;
            }
        }
        else
        {
            // blank
        }
    }

	return true;
}
```





```c++
// Operations
bool	CRSImage::Open(const char* lpstrPath)
{
	if (NULL == lpstrPath)
		return false;

	// 1. Read Meta Data
	string		strMetaFilePath = lpstrPath;
	int				pos = strMetaFilePath.rfind('.');
	if (pos>=0)
    {
        strMetaFilePath = strMetaFilePath.substr(0, pos);
    }
	strMetaFilePath.append(".hdr");
	if (!ReadMetaData(strMetaFilePath.c_str()))
	{
		cerr << "Read Meta Data Failed." << endl;
		return false;
	}

	// 2. Initialize Buffer
	if (!InitBuffer())
	{
		cerr << "Initialize Buffer Failed." << endl;
		return false;
	}

	// 3. Read File
	if (!ReadImgData(lpstrPath))
	{
		cerr << "Read Image Data Failed." << endl;
		return false;
	}

	return true;
}
```
---
| [Home](./README.md) | [课设题目](./Subject.md) | [<-Prev](./Frame.md) | [->Next](./Information.md)