# RS Image Format

## 1. RS Image

下图是一幅遥感图像显示的示例。图像窗口中，显示的图像是矩形区域，对应的数据为R-Band3， G-Band4， B-Band5，所以图像可以描述为三维矩阵（长\*宽\*高）。当图像逐步放大时，从右下方的Zoom窗口中可见，图像开始出现“马赛克”效应，即每一个像素点显示的够大，以至于像素间呈现了差异，这形象地揭示了图像和矩阵的联系。

![image-20210704093148250](https://i.loli.net/2021/07/04/jwPr5FKSGAegkOf.png)

## 2. RS Image  File

既然多波段的遥感图像是三维矩阵，那么我们又是如何存储的呢？数据存储在外存（硬盘）上时，只有一种存储方式——顺序存储。下图是上图显示图像的数据存储文件。从左侧数据地址可知，数据是按字节依次存放的，从地址0x00000000开始，每一行存16个字节，第2行首地址即为：0x00000010，依此类推。

![image-20210704101855309](https://i.loli.net/2021/07/04/8dVyBcmqbJEUuLR.png)

如果我们拿到上述的图像存储文件，我们能否将图像转变为三维矩阵呢？显然还不能，因为我们并不知道图像的长、宽、高以及每个像素存储的字节数（数据类型）。若要正确转换为矩阵，此时我们就需要借助一个对图像数据进行描述的文件，这种描述数据的数据，我们称之为**元数据**。对应其上的元数据文件如下所示：

```
ENVI
description = {
  Canon City, Colorado, Landsat TM, Calibrated to Reflectance }
samples = 640
lines   = 400
bands   = 6
header offset = 0
file type = ENVI Standard
data type = 1
interleave = bsq
sensor type = Landsat TM
wavelength units = Micrometers
z plot range = {0.00, 100.00}
z plot titles = {Wavelength, Reflectance}
default stretch = 2.0% linear
band names = {
 TM Band 1, TM Band 2, TM Band 3, TM Band 4, TM Band 5, TM Band 7}
wavelength = {
 0.48500, 0.56000, 0.66000, 0.83000, 1.65000, 2.21500}
```

从元数据中，我们可以读取到samples - 列，lines - 行, bands = 波段, data type - 数据类型，interleave-间隔方式等元数据。

## 3. RS Image Format

在三维矩阵写入到图像文件中时，根据数据间隔(interleave)方式不同，我们将数据分为按波段间隔-BSQ， 按行间隔-BIL和按点间隔-BIP，可以简单记为：点-BIP/线-BIL/面-BSQ。何为间隔方式呢，就是三维矩阵中的元素排列的方式，以三维数组A\[i\]\[j\]\[k\]为例，i-波段数bands，j-行数lines，k-列数samples，则：

- BSQ 排列顺序为：

  ```c++
  第1个波段：A[0][0][0],..., A[0][lines-1][samples-1],
  第2个波段：A[1][0][0],..., A[1][lines-1][samples-1],
  第.个波段：A[...][0][0],..., A[...][lines-1][samples-1],
  第b个波段：A[b-1][0][0],..., A[b-1][lines-1][samples-1]
  ```

- BIL 排列顺序为：

  ```c++
  第1个波段第1行：A[0][0][0],..., A[0][0][samples-1],
  ...,
  第1个波段第l行：A[0][l-1][0],..., A[0][l-1][samples-1],
  ...,
  
  第2个波段第1行：A[1][0][0],..., A[1][0][samples-1],
  ...,
  第2个波段第l行：A[1][l-1][0],..., A[1][l-1][samples-1],
  ...,
  
  第b个波段第1行：A[b-1][0][0],..., A[1][0][samples-1],
  ...,
  第b个波段第l行：A[b-1][l-1][0],..., A[b-1][l-1][samples-1]
  ```

- BIP 排列顺序为：

  ```C++
  第1个波段第1行第1列：A[0][0][0],
  ...,
  第b个波段第1行第1列：A[b-1][0][0],
  ...,
  
  第1个波段第1行第2列：A[0][0][1],
  ...,
  第b个波段第1行第2列：A[b-1][0][1],
  ...,
  
  第1个波段第1行第s列：A[0][0][s-1],
  ...,
  第b个波段第1行第s列：A[b-1][0][s-1],
  
  ...,
  第1个波段第2行第1列：A[0][1][0],
  ...,
  第b个波段第l行第s列：A[b-1][l-1][s-1]
  ```

## 4. Read RS Image File to Image Matrix

在已知了samples, lines, bands, datatype, interleave之后，用程序来实现读取就变的比较容易了。

```c++
//以数据类型为unsigned char为例
// 分配3维数组，存放图像文件
unsigned char*** pImg = new unsigned char**[nbands];
for (int i=0; i<nbands; ++i)
    pImg[i] = new unsigned char* [nlines];

for (int i=0; i<nbands; ++i)
    for (int j=0; j<nlines; ++j)
        pImg[i][j] = new unsigned char[nsamples];

ifstream	ifs("test.img", ios::binary);
// BSQ读取
for (int i=0; i<nbands; ++i)
    for (int j=0; j<nlines; ++j)
        ifs.read(reinterpret_cast<char*>pImg[i][j], sizeof(unsigned char)*nsamples);

//BIL读取
for (int j=0; j<nlines; ++j)
    for (int i=0; i<nbands; ++i)
        ifs.read(reinterpret_cast<char*>pImg[i][j], sizeof(unsigned char)*nsamples);

//BIP读取
for (int j=0; j<nlines; ++j)
    for (int k=0; k<nsamples; ++k)
        for (int i=0; i<nbands; ++i)
            ifs.read(reinterpret_cast<char*>&pImg[i][j][k], sizeof(unsigned char));

ifs.close();
```

## 5. Write Image Matrix to RS Image File

写文件，与读文件类似。把```ifs.read() --> ifs.write```即可。

## References





