# 调试与报告撰写

## 1 视频演示

- [01 机房预约系统-需求分析介绍](https://www.bilibili.com/video/BV1et411b73Z?p=282)
- [02 机房预约系统-成品展示](https://www.bilibili.com/video/BV1et411b73Z?p=283)
- [03 机房预约系统-创建项目](https://www.bilibili.com/video/BV1et411b73Z?p=284)
- [04 机房预约系统-主菜单界面搭建以及提供登录](https://www.bilibili.com/video/BV1et411b73Z?p=285)
- [05 机房预约系统-退出功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=286)
- [06 机房预约系统-身份的抽象基类创建](https://www.bilibili.com/video/BV1et411b73Z?p=287)
- [07 机房预约系统-学生类的创建](https://www.bilibili.com/video/BV1et411b73Z?p=288)
- [08 机房预约系统-教师类创建](https://www.bilibili.com/video/BV1et411b73Z?p=289)
- [09 机房预约系统-管理员类创建](https://www.bilibili.com/video/BV1et411b73Z?p=290)
- [10 机房预约系统-全局文件添加](https://www.bilibili.com/video/BV1et411b73Z?p=291)
- [11 机房预约系统-登录函数接口封装](https://www.bilibili.com/video/BV1et411b73Z?[p=292)
- [12 机房预约系统-学生身份登录实现](https://www.bilibili.com/video/BV1et411b73Z?[p=293)
- [13 机房预约系统-教师身份登录实现](https://www.bilibili.com/video/BV1et411b73Z?[p=294)
- [14 机房预约系统-管理员身份登录实现](https://www.bilibili.com/video/BV1et411b73Z?p=295)
- [15 机房预约系统-管理员子菜单搭建以及注销](https://www.bilibili.com/video/BV1et411b73Z?p=296)
- [16 机房预约系统-管理员添加账号实现](https://www.bilibili.com/video/BV1et411b73Z?p=297)
- [17 机房预约系统-获取文件中学生和老师信息](https://www.bilibili.com/video/BV1et411b73Z?p=298)
- [18 机房预约系统-检则账号重复的功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=299)
- [19 机房预约系统-解决检测重复账号添加](https://www.bilibili.com/video/BV1et411b73Z?p=300)
- [20 机房预约系统-查看账号功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=301)
- [21 机房预约系统-查看机房信息功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=302)
- [22 机房预约系统-清空预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=303)
- [23 机房预约系统-学生子菜单搭建以及注销实现](https://www.bilibili.com/video/BV1et411b73Z?p=304)
- [24 机房预约系统-申请预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=305)
- [25 机房预约系统-预约类的容器属性分析](https://www.bilibili.com/video/BV1et411b73Z?p=306)
- [26 机房预约系统-预约类中获取所有预约信息](https://www.bilibili.com/video/BV1et411b73Z?p=307)
- [27 机房预约系统-更新预约记录功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=308)
- [28 机房预约系统-学生显示自身预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=309)
- [29 机房预约系统-学生显示所有预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=310)
- [30 机房预约系统-学生取消预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=311)
- [31 机房预约系统-教师子菜单搭建以及注销实现](https://www.bilibili.com/video/BV1et411b73Z?p=312)
- [32 机房预约系统-教师显示所有预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=313)
- [33 机房预约系统-教师审核预约功能实现](https://www.bilibili.com/video/BV1et411b73Z?p=314)
---

## 2 代码调式
代码调试是编写程序的基本功，通过调试代码既可以快速查找错误，也可以理清程序逻辑结构。下面对调试基本技巧简要说明：
1. 编译 - F7
	- 代码完成后，先编译，检查语法错误
2. 调试模式 - F5
	- 进入调试模式，可以查看变量值和程序运行逻辑、调用堆栈等信息
3. 断点 - F9
	- 加入断点的行，程序会暂停，此时可以查看变量
4. 逐过程跟踪 - F10
	- 碰到函数，不进入函数内部。用于查看程序执行流程
5. 逐语句跟踪 - F11
	- 碰到函数，会进入函数内部，逐行执行，用于查看变量和跟踪错误信息。
6. 查帮助
	- [cppreference](https://en.cppreference.com/w/)是查找C++帮助的首选
7. 遇到错误解决办法
	- [google](google.com) 或 [stackoverflow](https://stackoverflow.com/) 或 [bing](bing.com)


## 3 报告撰写

报告的撰写至关重要，几点要求：
1. 格式
	- 严格按照固定的格式撰写，不可随意为之
2. 内容
	- 包括**完整的课设思路，不可对思考过程忽略，只写最后结果，除了关键代码，大部分代码尽量不要**出现在报告中。
	- 绘制图表，对需要用图、表来表达的地方，图要美观、表要规范，不可随意画，甚至手画拍照（除非手绘很厉害）。
3. 提交内容
	- 报告文件(学号_姓名.pdf)
	- 源代码压缩包(学号_姓名.zip)
	- 演示视频3min以内(学号_姓名.mp4)
	- 纸质报告（带封面打印）
	

---

Go to | [Home](../README.md) | [Head](#调试与报告撰写) | [ShapeEdit<-Prev](./D7_ShapeEdit.md) | 