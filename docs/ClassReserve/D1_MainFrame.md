
# 程序框架

**功能描述：**

* 设计主菜单，与用户进行交互

## 3.1 菜单实现

* 在主函数main中添加菜单提示，代码如下：

```C++
int main() {

	cout << "======================  欢迎来到传智播客机房预约系统  =====================" 
         << endl;
	cout << endl << "请输入您的身份" << endl;
	cout << "\t\t -------------------------------\n";
	cout << "\t\t|                               |\n";
	cout << "\t\t|          1.学生代表           |\n";
	cout << "\t\t|                               |\n";
	cout << "\t\t|          2.老    师           |\n";
	cout << "\t\t|                               |\n";
	cout << "\t\t|          3.管 理 员           |\n";
	cout << "\t\t|                               |\n";
	cout << "\t\t|          0.退    出           |\n";
	cout << "\t\t|                               |\n";
	cout << "\t\t -------------------------------\n";
	cout << "输入您的选择: ";

	system("pause");

	return 0;
}
```



运行效果如图：

![1548557945611](assets/1548557945611.png)



## 3.2 搭建接口

* 接受用户的选择，搭建接口
* 在main中添加代码

```C++
int main() {

	int select = 0;

	while (true)
	{

		cout << "======================  欢迎来到传智播客机房预约系统  =====================" << endl;
		cout << endl << "请输入您的身份" << endl;
		cout << "\t\t -------------------------------\n";
		cout << "\t\t|                               |\n";
		cout << "\t\t|          1.学生代表           |\n";
		cout << "\t\t|                               |\n";
		cout << "\t\t|          2.老    师           |\n";
		cout << "\t\t|                               |\n";
		cout << "\t\t|          3.管 理 员           |\n";
		cout << "\t\t|                               |\n";
		cout << "\t\t|          0.退    出           |\n";
		cout << "\t\t|                               |\n";
		cout << "\t\t -------------------------------\n";
		cout << "输入您的选择: ";

		cin >> select; //接受用户选择

		switch (select)
		{
		case 1:  //学生身份
			break;
		case 2:  //老师身份
			break;
		case 3:  //管理员身份
			break;
		case 0:  //退出系统
			break;
		default:
             cout << "输入有误，请重新选择！" << endl;
		    system("pause");
			system("cls");
			break;
		}

	}
	system("pause");
	return 0;
}
```

测试，输入0、1、2、3会重新回到界面，输入其他提示输入有误，清屏后重新选择

效果如图：

![1548558694230](assets/1548558694230.png)

## 3.3 退出功能实现

在main函数分支 0 选项中，添加退出程序的代码：

```C++
	cout << "欢迎下一次使用"<<endl;
	system("pause");
	return 0;
```



![1548558992754](assets/1548558992754.png)



## 3.4 测试退出功能

运行程序，效果如图：

![1548559026436](assets/1548559026436.png)

至此，界面搭建完毕

## 3.5 探索
1. 用Qt搭建GUI界面
2. 设计GUI功能菜单

---
Go to | [Home](./README.md) | [Head](#程序框架) | [创建项目<-Prev](./D0_CreatePrj.md) |[Next->设计类](./D2_DesignObjects.md)| 