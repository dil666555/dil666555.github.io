---
title: 职工管理系统
date: 2022-06-12 00:00:00
updated: 2025-11-13 00:59:39
categories:
  - C++
tags:
  - C++
  - 学习笔记
---

# 职工管理系统

## 1. 管理系统需求

职工管理系统可以用来管理公司所有员工的信息

目标：使用c++实现一个基于多态的职工管理系统

公司职工分为三类：普通员工，经理，老板，显示信息时需要显示职工编号，职工姓名，职工岗位，以及职责

- 普通员工职责：完成经理交给的任务

- 经理职责：完成老板交给的任务，并下发任务给员工

- 老板职责：管理公司所有事物

管理系统需要实现的功能如下：

- 退出管理程序：退出当前管理系统

- 增加职工信息：实现批量添加职工功能，将信息录入到文件中，职工信息为：职工编号，姓名，部门编号

- 显示职工信息：显示公司内部所有职工信息

- 删除离职职工：按照编号删除指定的职工

- 修改职工信息：按照编号修改职工个人信息

- 查找职工信息：按照职工编号过着职工的姓名进行查找相关的人员信息

- 按照编号排序：按照职工编号，进行排序，排序规则由用户指定

- 清空所有文档：清空文件中记录的所有职工信息（清空前需要再次确定，防止误删）

## 2. 创建项目

## 3. 创建管理类

管理类负责内容如下：

- 与用户的沟通菜单界面

- 对职工更删改查的操作

- 与文件的读写交互

### 3.1 创建文件

### 3.2 头文件实现

```cpp
//在workManager.h文件中实现
#pragma once
#include<iostream>
using namespace std;

class workManager {
public:
	workManager();

	~workManager();
};
```

### 3.3 源文件实现

```cpp
//在workManager.cpp文件中实现
#include"workManager.h"

workManager::workManager() {

}

workManager::~workManager() {

}
```

## 4. 菜单功能

功能描述：与用户的沟通界面

### 4.1 添加成员函数

在管理类workManager.h中添加函数 `void showMenu();`

### 4.2 菜单功能实现

在管理类workManager.cpp,中实现该函数

```cpp
void workManager::showMenu() {
	cout << "******************************************************" << endl;
	cout << "*****************欢迎使用职工管理系统*****************" << endl;
	cout << "*****************   0.退出管理系统   *****************" << endl;
	cout << "*****************   1.添加职工信息   *****************" << endl;
	cout << "*****************   2.显示职工信息   *****************" << endl;
	cout << "*****************   3.删除职工信息   *****************" << endl;
	cout << "*****************   4.修改职工信息   *****************" << endl;
	cout << "*****************   5.查找职工信息   *****************" << endl;
	cout << "*****************   6.排序职工信息   *****************" << endl;
	cout << "*****************   7.清空所有文档   *****************" << endl;
	cout << endl;
}
```

### 4.3 测试菜单功能

在职工管理系统.cpp文件中创建main函数测试展示菜单函数

```cpp
#include"workManager.h"

int main() {
	workManager manager01;
	manager01.showMenu();
	return  0;
}
```

## 5. 退出功能

### 5.1 提供功能接口

在main函数中提供分支选择，提供每个分支接口

```cpp
int main() {
	workManager manager01;
	int choice = 0;
	while (1) {
		switch (choice)
		{
		manager01.showMenu();
		cout << "请选择" << endl;
		cin >> choice;
		case0:  //退出功能
			break;
		case1:  //添加功能
			break;
		case2:  //显示功能
			break;
		case3:  //删除功能
			break;
		case4:  //修改功能
			break;
		case5:  //查找功能
			break;
		case6:  //排序功能
			break;
		case7:  //清除功能
			break;
		default:
			system("cls");
			break;
		}
	}

	return  0;
}
```

### 5.2 实现退出功能

在workManager.h中添加成员函数`void exitSystem();`

在workManager.cpp中添加实现

```cpp
//.h文件操作
void exitSystem();
//.cpp文件操作
void workManager::exitSystem() {
	cout << "欢迎下次使用" << endl;
	exit(0);//要在非main函数内实现结束函数进程使用exit函数即可
}
```

## 6. 创建职工类

### 6.1 创建职工抽象类

职工分类：普通职工，经理，老板

将三种职工抽象到一个类worker中，利用多态来管理不同职工种类

职工的属性为：职工编号，职工姓名，职工所在部门编号

职工的行为为：岗位职责信息描述，获取岗位名称

```cpp
//woeker.h文件
#pragma once
#include<iostream>
using namespace std;
#include<string>

class worker {
public:
	virtual void showInfo() = 0;

	virtual string getDeptName() = 0;

	int m_id;
	string m_name;
	int m_dId;
};
```

### 6.2 创建普通员工类

```cpp
//employee.h文件
#pragma once
#include"worker.h"

class employee :public worker{
public:
	employee(int id, string name, int dId);

	virtual void showInfo();

	virtual string getDeptName();

};

//employee.cpp文件
#include"employee.h"

employee::employee(int id,string name,int dId) {
	m_id = id;
	m_name = name;
	m_dId = dId;
}

void employee::showInfo() {
	cout << "编号：" << m_id
		<< "\t姓名：" << m_name
		<< "\t部门：" << this->getDeptName() << endl;
}

string employee::getDeptName() {
	return string("员工");
}
```

### 6.3 创建经理类

```cpp
//Manager.h文件
#pragma once
#include<iostream>
using namespace std;
#include"worker.h"

class Manager :public worker {
public:
	Manager(int id,string name,int dId);

	virtual void showInfo();

	virtual string getDeptName();

};

//manager.cpp文件
#include"manager.h"

Manager::Manager(int id, string name, int dId) {
	m_id = id;
	m_name = name;
	m_dId = dId;
}

void Manager::showInfo() {
	cout << "编号：" << m_id
		<< "\t姓名：" << m_name
		<< "\t职位：" << this->getDeptName() << endl;
}

string Manager::getDeptName() {
	return string("经理");
}
```

### 6.4 创建老板类

```cpp
//boss.h文件
#pragma once
#include<iostream>
using namespace std;
#include"worker.h"

class boss :public worker{
public:
	boss(int id, string name, int dId);

	virtual void showInfo();

	virtual string getDeptName();

};
//boss.cpp文件
#include"boss.h"

boss::boss(int id, string name, int dId) {
	m_id = id;
	m_name = name;
	m_dId = dId;
}

void boss::showInfo() {
	cout << "编号：" << m_id
		<< "\t姓名：" << m_name
		<< "\t职位：" << this->getDeptName() << endl;
}

string boss::getDeptName() {
	return string("总裁");
}
```

### 6.5 测试多态

```cpp
worker* wo;
wo = new employee(1, "张三", 1);
wo->showInfo();
delete wo;

wo = new Manager(2, "李四", 2);
wo->showInfo();
delete wo;

wo = new boss(3, "王五", 3);
wo->showInfo();
delete wo;
```

运行结果：

编号：1 姓名：张三      职位：员工编号：2 姓名：李四      职位：经理编号：3 姓名：王五      职位：总裁

## 7. 添加职工

功能描述：批量添加职工并且保存到文件中

### 7.1 功能分析

分析：

用户在批量创建时可能会添加不同种类的职工

如果想将不同种类的员工添加到同一个数组中，可以将所有员工的指针维护到同一个数组中

如果想在程序中维护这个不定长度的指针，可以创建到堆区，并利用worker**维护

## 8. 文件交互–写文件

功能描述：对文件进行读写

### 8.1 设定文件路径

`#include<fstream> #define FILENAME "empFile.txt"`

### 8.2 成员函数声明

在workerManager.h文件中添加函数声明

`void save`

### 8.3 保存文件功能实现

```cpp
//在workerManager.cpp文件中实现
void workManager::save() {
	ofstream ofs;
	ofs.open(FILENAME, ios::out);

	for (int i = 0; i < this->m_EmpNum; i++) {
		ofs << this->m_EmpArry[i]->m_id << " "
			<< this->m_EmpArry[i]->m_name << " "
			<< this->m_EmpArry[i]->m_dId << endl;
	}

	ofs.close();
}
```

## 9. 文件交互–读文件

功能描述：将文件中的内容读取到程序中

虽然我们实现了添加职工后保存到文件的操作，但是每次开始运行程序，并没有将文件中的数据读取到文件中

而我们的程序功能中还有清空文件的需求

因此构造函数初始化数据的情况分为三种

1.第一次使用，文件未创建

2.文件存在，但数据被用户清空

3.文件存在，且保存有数据

### 9.1 文件未创建

在workManager.h中添加新的成员属性用来标志是否为空

`//标志文件是否为空`

`bool m_FileIsEmpty;`

修改workManager.cpp文件中的构造函数

```cpp
workManager::workManager() {
	//文件未创建
	ifstream ifs;
	ifs.open(FILENAME, ios::in);
	if (!ifs.is_open()) {
		cout << "文件不存在"<<endl;
		//初始化人数为零
		m_EmpNum = 0;
		//初始化指针未空
		m_EmpArry = NULL;
		//初始化标志为真
		m_FileIsEmpty = true;
		ifs.close();
		return;
	}
}
```

### 9.2 文件存在且数据为空

在workerManager.cpp中的构造函数追加代码

```cpp
//文件存在但数据为空
	char ch;
	ifs >> ch;
	if (ifs.eof()) {
		cout << "文件存在但为空" << endl;
		//初始化人数为零
		m_EmpNum = 0;
		//初始化指针未空
		m_EmpArry = NULL;
		//初始化标志为真
		m_FileIsEmpty = true;
		ifs.close();
		return;
	}
```

### 9.3 文件存在且保存有职工数据

#### 9.3.1 获取记录的职工人数

在`workerManager.h`文件中添加成员函数`int get_empNum`

```cpp
int get_empNum();
```

在workManager.cpp文件中实现函数`int get_empNum();`

```cpp
int workManager::get_empNum() {
	ifstream ifs;
	int id;
	string name;
	int dId;
	int num = 0;
	ifs.open(FILENAME,ios::in);
	while (ifs >> id && ifs >> name && ifs >> dId) {
		num++;
	}
	return num;
}
```

在构造函数中引用`get_empNum()`函数

```cpp
//文件存在并且不为空
	int num = this->get_empNum();
	cout << "职工人数为：" << num << endl;
	this->m_EmpNum=num;
```

#### 9.3.2 初始化数据

根据职工的数据，初始化workerManager中的work** m_EmpArry指针

在workManager.h中添加成员函数`void init_Emp();`

```cpp
void init_Emp();//对数据进行初始化
```

在workManager.cpp中实现函数`void init_Emp();`

```cpp
//对已有数据进行初始化
void workManager::init_Emp() {
	ifstream ifs;
	ifs.open(FILENAME, ios::in);
	int id;
	string name;
	int dId;
	int index = 0;
	while (ifs >> id && ifs >> name && ifs >> dId) {
		worker* worker=NULL;
		switch (dId)
		{
		case 1:
			worker = new employee(id, name, dId);
			break;
		case 2:
			worker = new Manager(id, name, dId);
			break;
		case 3:
			worker = new boss(id, name, dId);
			break;
		default:
			break;
		}
		this->m_EmpArry[index] = worker;
		index++;
	}
}
```

在构造函数中调用：

```cpp
//开辟空间
	this->m_EmpArry = new worker * [this->m_EmpNum];
	//将数据存储
	this->init_Emp();
	for (int i = 0; i < this->m_EmpNum; i++) {
		cout << "编号：" << this->m_EmpArry[i]->m_id
			<< "姓名：" << this->m_EmpArry[i]->m_name
			<< "岗位：" << this->m_EmpArry[i]->getDeptName() << endl;
	}
```

## 10. 显示职工

功能描述：显示当前所有职工的信息

### 10.1 显示职工函数声明

在workManager.h文件中添加`void show_Emp();`

```cpp
//显示所有职工信息
void workManager::show_Emp() {
	if (this->m_FileIsEmpty) {
		cout << "文件不存在或记录为空" << endl;
	}
	else {
		for (int i = 0; i < this->m_EmpNum; i++) {
			//利用多态调用接口
			this->m_EmpArry[i]->showInfo();
		}
	}
	system("pause");
	system("cls");
}
```

## 11. 删除职工

功能描述：按照职工的编号进行删除操作

### 11.1 删除职工函数声明

在workManager.h文件中太添加成员函数`void Del_Emp()`

```cpp
void Del_Emp();//对职工进行删除操作
```

### 11.2 职工是否存在函数声明

由于功能删除职工，修改职工，查找职工都要判断职工是否存在

则在workManager.h函数中申明`int IsExist(int id);`

```cpp
int isExist(int id);//按照编号判断这个人是否存在
```

### 11.3 职工是否存在函数实现

在workManager.cpp函数中实现

```cpp
//判断是否有该id员工存在
int workManager::isExist(int id) {
	int index = -1;
	for (int i = 0; i < this->m_EmpNum; i++) {
		if (this->m_EmpArry[i]->m_id = id) {
			index = i;
			return index;
		}
	}
	return index;
}
```

### 11.4 删除职工函数实现

在workManager.cpp文件中实现函数`void Del_Emp();`

```cpp
void workManager::Del_Emp() {//对职工进行删除操作
	if (this->m_FileIsEmpty) {
		cout << "文件不存在" << endl;
	}
	else {
		cout << "请输入要删除的职员编号" << endl;
		int id = 0;
		cin >> id;
		int index = this->isExist(id);
		if (index != -1) {
			for (int i = index; i < this->m_EmpNum - 1; i++) {
				this->m_EmpArry[i] = this->m_EmpArry[i + 1];
			}
			this->m_EmpNum--;
			this->save();
			cout << "删除成功" << endl;
		}
		else {
			cout << "删除失败，未找到该员工" << endl;
		}
	}
	system("pause");
	system("cls");
}
```

## 12. 修改职工

功能描述：能够按照职工编号对职工信息进行修改

### 12.1 修改职工函数声明

在workManager.h文件中声明`void Mod_Emp();`

```cpp
//修改职工
void Mod_Emp();
```

### 12.3 修改函数实现、

在workManager.cpp文件中实现函数`void Mod_Emp();`

```cpp
void workManager::Del_Emp() {//对职工进行删除操作
	if (this->m_FileIsEmpty) {
		cout << "文件不存在" << endl;
	}
	else {
		cout << "请输入要删除的职员编号" << endl;
		int id = 0;
		cin >> id;
		int index = this->isExist(id);
		if (index != -1) {
			for (int i = index; i < this->m_EmpNum - 1; i++) {
				this->m_EmpArry[i] = this->m_EmpArry[i + 1];
			}
			this->m_EmpNum--;
			this->save();
			cout << "删除成功" << endl;
		}
		else {
			cout << "删除失败，未找到该员工" << endl;
		}
	}
	system("pause");
	system("cls");
}
```

## 13. 查找职工

功能描述：提供两种查找职工的方式，一种按照职工编号，一种按照职工姓名

### 13.1 查找职工函数声明

在workManager.h文件中进行`void Find_Emp();`

```cpp
//对职工进行查找
void workManager::Find_Emp() {
	if (this->m_FileIsEmpty) {
		cout << "文件不存在" << endl;
	}
	else {
		cout << "请输入查找方式\n" << "1.通过id查找\n" << "2.通过姓名查找" << endl;
		int select = 0;
		cin >> select;
		if (select == 1) {//按照id查找
			cout << "请输入要查找的id" << endl;
			int id = 0;
			cin >> id;
			int indx = this->isExist(id);
			if (indx != -1) {
				cout << "已经查找到了这名成员,该成员信息为：" << endl;
				this->m_EmpArry[indx]->showInfo();
			}
			else {
				cout << "没有该编号的员工" << endl;
			}
		}
		else if(select==2) {//按照姓名查找
			cout << "请输入该员工的姓名" << endl;
			string name="";
			cin >> name;
			bool flag = false;
			for (int i = 0; i < m_EmpNum; i++) {
				if (this->m_EmpArry[i]->m_name == name) {
					cout << "找到了改名员工,该名员工的信息为" << endl;
					this->m_EmpArry[i]->showInfo();
					flag = true;
				}
			}
			if (!flag) {
				cout << "未找到姓名为" << name << "的员工" << endl;
			}
		}
		system("pause");
		system("cls");
	}
}
```

## 14. 排序

功能描述：按照职工编号进行排序，排序的顺序由用户指定

### 14.1 排序函数声明

在workManager.h文件中实现成员函数`void Sort_Emp();`

```cpp
void workManager::Sort_Emp() {
	if (this->m_FileIsEmpty) {
		cout << "文件不存在" << endl;
		system("pause");
		system("cls");
	}
	else {
		cout << "请选择排序方式\n" << "1.升序\n" << "2.降序" << endl;
		int select = 0;
		cin >> select;
		for (int i = 0; i < m_EmpNum; i++) {
			int minOrmax = i;
			for (int j = i + 1; j < m_EmpNum; j++) {
				if (select == 1) {//升序
					if (m_EmpArry[minOrmax]->m_id > m_EmpArry[j]->m_id) {
						minOrmax = j;
					}
				}
				else {//降序
					if (m_EmpArry[minOrmax]->m_id < m_EmpArry[j]->m_id) {
						minOrmax = j;
					}
				}
			}
			if (i != minOrmax) {
				worker* temp = m_EmpArry[i];
				m_EmpArry[i] = m_EmpArry[minOrmax];
				m_EmpArry[minOrmax] = temp;
			}
		}
		cout << "排序成功，排序结果为" << endl;
		this->save();
		this->show_Emp();
	}
}
```

## 15. 清空文件

功能描述：将文件中所有的数据清空

### 15.1 清空函数声明

在workerManager.h文件中声明`void clear_File();`

```cpp
void clear_File();//将文件中所有的数据清空
```

### 15.3 清空函数实现

```cpp
//将文件中所有的数据清空
void workManager::clear_File() {
	if (this->m_FileIsEmpty) {
		cout << "文件不存在" << endl;
		system("pause");
		system("cls");
		return;
	}
	cout << "是否确定清空\n" << "1.确定\n" << "2.取消" << endl;
	int select = 0;
	cin >> select;
	if (select == 2) {
		system("pause");
		system("cls");
		return;
	}
	else if (select == 1) {
		ofstream osf(FILENAME, ios::trunc);//将文件删除再创建
		if (this->m_EmpArry != NULL) {
			for (int i = 0; i < this->m_EmpNum; i++) {
				if (this->m_EmpArry[i] != NULL) {
					delete this->m_EmpArry[i];
				}
			}
			this->m_EmpNum = 0;
			delete[] this->m_EmpArry;
			this->m_EmpArry = NULL;
			this->m_FileIsEmpty = true;
		}
		cout << "清空成功" << endl;
	}
	system("pause");
	system("cls");
}
```

**文章作者: [dre](https://dil666555.github.io)**文章链接: [https://dil666555.github.io/2022/06/12/staff-management/](https://dil666555.github.io/2022/06/12/staff-management/)**版权声明: 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明来源 [dre](https://dil666555.github.io)！[C++](/tags/C/)[学习笔记](/tags/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/)[上一篇C++面向对象编程c++面向对象编程1. 内存分区模型C＋＋程序在执行时，将内存大方向划分为4个区域  代码区：存放函数体的二进制代码，由操作系统进行管理的  全局区：存放全局变量和静态变量以及常量  ·栈区：由编译器自动分配释放，存放函数的参数值，局部变量等  ·堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收   1.1 程序运行前在程序编译后，生成了exe可执行程序，未执行该程序前分为两个区域 代码区： ​	存放CPU 执行的机器指令 ​	代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可，代码区是只读的，使其只读的原因是防止程序意外地修改了它的指令 全局区： ​	全局变量和静态变量存放在此. ​	全局区还包含了常量区，字符串常量和其他常量（const修饰的变量）也存放在此. ​	该区域的数据在程序结束后由操作系统释放. 1.2 程序运行后栈区： 由编译器自动释分配释放，存放函数的参数值，局部变量等； 注意：不要返回局部变量的地址，栈区开辟的地址由编译器自动释放; 123456789int* fuct(){	int a=10;    r...](/2022/06/14/cpp-oop/)[下一篇机房预约系统机房预约系统1. 机房预约系统需求1.1 系统简介1.2 身份简介 **学生：**申请使用机房 **教师：**审核学生的预约申请 **管理员：**给学生，老师创建账号  1.3 机房简介 **1号机房：**容量20人 **2号机房：**容量50人 **3号机房：**容量100人  1.4 申请简介 申请记录每周由管理员清空 学生可以预约未来一周内的机房使用，预约的日期为周一到周五，预约是需要选择预约时段（上午，下午） 教师来审核预约  1.5 系统需求登录界面：  学生，老师，管理员，退出  身份验证后进入子菜单：  学生：学号，姓名，密码 老师：教工号，姓名，密码 管理员：姓名，密码  学生具体功能：  申请预约 查看自身预约   查看所有预约  —  查看所有预约信息及预约状态 取消预约  —  成功或审核中都可取消 注销登录  教师具体功能：  查看所有预约 审核预约 注销登录  管理员具体功能：   添加账号  —  添加学生或教师账号，需检测是否重复 查看账号  —  可以选择查看学生或教师全部信息 查看机房  —  查看所有机房的信息 清空预约 注销登录  2. 创建...](/2022/06/11/lab-reservation/)**相关推荐[** 2022-06-10c++提高编程c++提高编程主要针对c++范式编程以及STL技术 1.模板1.1 模板的概念模板就是建立通用的模具看，大大提高复用性 例如生活中的一寸照片和PPT模板 模板的特点：  模板不可以直接使用，它只是一个框架 模板的通用并不是万能的  1.2 函数模板 c++另一种编程思想称为泛型编程，主要利用的技术就是模板 c++提供两种模板机制：函数模板和类模板  1.2.1 函数模板语法函数模板作用： 建立一个通用函数，其函数返回值和形参类型可以不指定，用一个虚拟的类型来代替即可 语法： 12template<typename T>//其中T就代表了一个虚拟的类型，名称可以更换//函数定义或语法  解释：template—表示要创建一个模板了 typename–表示其后面的符号是一种数据类型，可以用class来代替 T        –        表示一种通用的数据类型，名称可以更改，通常为大写字母 实例： 12345678910111213141516171819202122232425262728293031323334353637383940414243444546474...](/2022/06/10/cpp-improved/)[** 2022-06-14C++面向对象编程c++面向对象编程1. 内存分区模型C＋＋程序在执行时，将内存大方向划分为4个区域  代码区：存放函数体的二进制代码，由操作系统进行管理的  全局区：存放全局变量和静态变量以及常量  ·栈区：由编译器自动分配释放，存放函数的参数值，局部变量等  ·堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收   1.1 程序运行前在程序编译后，生成了exe可执行程序，未执行该程序前分为两个区域 代码区： ​	存放CPU 执行的机器指令 ​	代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可，代码区是只读的，使其只读的原因是防止程序意外地修改了它的指令 全局区： ​	全局变量和静态变量存放在此. ​	全局区还包含了常量区，字符串常量和其他常量（const修饰的变量）也存放在此. ​	该区域的数据在程序结束后由操作系统释放. 1.2 程序运行后栈区： 由编译器自动释分配释放，存放函数的参数值，局部变量等； 注意：不要返回局部变量的地址，栈区开辟的地址由编译器自动释放; 123456789int* fuct(){	int a=10;    r...](/2022/06/14/cpp-oop/)[** 2022-06-11机房预约系统机房预约系统1. 机房预约系统需求1.1 系统简介1.2 身份简介 **学生：**申请使用机房 **教师：**审核学生的预约申请 **管理员：**给学生，老师创建账号  1.3 机房简介 **1号机房：**容量20人 **2号机房：**容量50人 **3号机房：**容量100人  1.4 申请简介 申请记录每周由管理员清空 学生可以预约未来一周内的机房使用，预约的日期为周一到周五，预约是需要选择预约时段（上午，下午） 教师来审核预约  1.5 系统需求登录界面：  学生，老师，管理员，退出  身份验证后进入子菜单：  学生：学号，姓名，密码 老师：教工号，姓名，密码 管理员：姓名，密码  学生具体功能：  申请预约 查看自身预约   查看所有预约  —  查看所有预约信息及预约状态 取消预约  —  成功或审核中都可取消 注销登录  教师具体功能：  查看所有预约 审核预约 注销登录  管理员具体功能：   添加账号  —  添加学生或教师账号，需检测是否重复 查看账号  —  可以选择查看学生或教师全部信息 查看机房  —  查看所有机房的信息 清空预约 注销登录  2. 创建...](/2022/06/11/lab-reservation/)![avatar](/images/profile.jpg)dredre 的博客[文章4](/archives/)[标签2](/tags/)[分类1](/categories/)[**Follow Me](https://github.com/dil666555)[**](https://github.com/dil666555)**公告This is my Blog**目录

1. [1. 职工管理系统](#%E8%81%8C%E5%B7%A5%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F)

  1. [1.1. 1. 管理系统需求](#1-%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F%E9%9C%80%E6%B1%82)
  2. [1.2. 2. 创建项目](#2-%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE)
  3. [1.3. 3. 创建管理类](#3-%E5%88%9B%E5%BB%BA%E7%AE%A1%E7%90%86%E7%B1%BB)

    1. [1.3.1. 3.1 创建文件](#3-1-%E5%88%9B%E5%BB%BA%E6%96%87%E4%BB%B6)
    2. [1.3.2. 3.2 头文件实现](#3-2-%E5%A4%B4%E6%96%87%E4%BB%B6%E5%AE%9E%E7%8E%B0)
    3. [1.3.3. 3.3 源文件实现](#3-3-%E6%BA%90%E6%96%87%E4%BB%B6%E5%AE%9E%E7%8E%B0)
  4. [1.4. 4. 菜单功能](#4-%E8%8F%9C%E5%8D%95%E5%8A%9F%E8%83%BD)

    1. [1.4.1. 4.1 添加成员函数](#4-1-%E6%B7%BB%E5%8A%A0%E6%88%90%E5%91%98%E5%87%BD%E6%95%B0)
    2. [1.4.2. 4.2 菜单功能实现](#4-2-%E8%8F%9C%E5%8D%95%E5%8A%9F%E8%83%BD%E5%AE%9E%E7%8E%B0)
    3. [1.4.3. 4.3 测试菜单功能](#4-3-%E6%B5%8B%E8%AF%95%E8%8F%9C%E5%8D%95%E5%8A%9F%E8%83%BD)
  5. [1.5. 5. 退出功能](#5-%E9%80%80%E5%87%BA%E5%8A%9F%E8%83%BD)

    1. [1.5.1. 5.1 提供功能接口](#5-1-%E6%8F%90%E4%BE%9B%E5%8A%9F%E8%83%BD%E6%8E%A5%E5%8F%A3)
    2. [1.5.2. 5.2 实现退出功能](#5-2-%E5%AE%9E%E7%8E%B0%E9%80%80%E5%87%BA%E5%8A%9F%E8%83%BD)
  6. [1.6. 6. 创建职工类](#6-%E5%88%9B%E5%BB%BA%E8%81%8C%E5%B7%A5%E7%B1%BB)

    1. [1.6.1. 6.1 创建职工抽象类](#6-1-%E5%88%9B%E5%BB%BA%E8%81%8C%E5%B7%A5%E6%8A%BD%E8%B1%A1%E7%B1%BB)
    2. [1.6.2. 6.2 创建普通员工类](#6-2-%E5%88%9B%E5%BB%BA%E6%99%AE%E9%80%9A%E5%91%98%E5%B7%A5%E7%B1%BB)
    3. [1.6.3. 6.3 创建经理类](#6-3-%E5%88%9B%E5%BB%BA%E7%BB%8F%E7%90%86%E7%B1%BB)
    4. [1.6.4. 6.4 创建老板类](#6-4-%E5%88%9B%E5%BB%BA%E8%80%81%E6%9D%BF%E7%B1%BB)
    5. [1.6.5. 6.5 测试多态](#6-5-%E6%B5%8B%E8%AF%95%E5%A4%9A%E6%80%81)
  7. [1.7. 7. 添加职工](#7-%E6%B7%BB%E5%8A%A0%E8%81%8C%E5%B7%A5)

    1. [1.7.1. 7.1 功能分析](#7-1-%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90)
  8. [1.8. 8. 文件交互–写文件](#8-%E6%96%87%E4%BB%B6%E4%BA%A4%E4%BA%92%E2%80%93%E5%86%99%E6%96%87%E4%BB%B6)

    1. [1.8.1. 8.1 设定文件路径](#8-1-%E8%AE%BE%E5%AE%9A%E6%96%87%E4%BB%B6%E8%B7%AF%E5%BE%84)
    2. [1.8.2. 8.2 成员函数声明](#8-2-%E6%88%90%E5%91%98%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
    3. [1.8.3. 8.3 保存文件功能实现](#8-3-%E4%BF%9D%E5%AD%98%E6%96%87%E4%BB%B6%E5%8A%9F%E8%83%BD%E5%AE%9E%E7%8E%B0)
  9. [1.9. 9. 文件交互–读文件](#9-%E6%96%87%E4%BB%B6%E4%BA%A4%E4%BA%92%E2%80%93%E8%AF%BB%E6%96%87%E4%BB%B6)

    1. [1.9.1. 9.1 文件未创建](#9-1-%E6%96%87%E4%BB%B6%E6%9C%AA%E5%88%9B%E5%BB%BA)
    2. [1.9.2. 9.2 文件存在且数据为空](#9-2-%E6%96%87%E4%BB%B6%E5%AD%98%E5%9C%A8%E4%B8%94%E6%95%B0%E6%8D%AE%E4%B8%BA%E7%A9%BA)
    3. [1.9.3. 9.3 文件存在且保存有职工数据](#9-3-%E6%96%87%E4%BB%B6%E5%AD%98%E5%9C%A8%E4%B8%94%E4%BF%9D%E5%AD%98%E6%9C%89%E8%81%8C%E5%B7%A5%E6%95%B0%E6%8D%AE)

      1. [1.9.3.1. 9.3.1 获取记录的职工人数](#9-3-1-%E8%8E%B7%E5%8F%96%E8%AE%B0%E5%BD%95%E7%9A%84%E8%81%8C%E5%B7%A5%E4%BA%BA%E6%95%B0)
      2. [1.9.3.2. 9.3.2 初始化数据](#9-3-2-%E5%88%9D%E5%A7%8B%E5%8C%96%E6%95%B0%E6%8D%AE)
  10. [1.10. 10. 显示职工](#10-%E6%98%BE%E7%A4%BA%E8%81%8C%E5%B7%A5)

    1. [1.10.1. 10.1 显示职工函数声明](#10-1-%E6%98%BE%E7%A4%BA%E8%81%8C%E5%B7%A5%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
  11. [1.11. 11. 删除职工](#11-%E5%88%A0%E9%99%A4%E8%81%8C%E5%B7%A5)

    1. [1.11.1. 11.1 删除职工函数声明](#11-1-%E5%88%A0%E9%99%A4%E8%81%8C%E5%B7%A5%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
    2. [1.11.2. 11.2 职工是否存在函数声明](#11-2-%E8%81%8C%E5%B7%A5%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
    3. [1.11.3. 11.3 职工是否存在函数实现](#11-3-%E8%81%8C%E5%B7%A5%E6%98%AF%E5%90%A6%E5%AD%98%E5%9C%A8%E5%87%BD%E6%95%B0%E5%AE%9E%E7%8E%B0)
    4. [1.11.4. 11.4 删除职工函数实现](#11-4-%E5%88%A0%E9%99%A4%E8%81%8C%E5%B7%A5%E5%87%BD%E6%95%B0%E5%AE%9E%E7%8E%B0)
  12. [1.12. 12. 修改职工](#12-%E4%BF%AE%E6%94%B9%E8%81%8C%E5%B7%A5)

    1. [1.12.1. 12.1 修改职工函数声明](#12-1-%E4%BF%AE%E6%94%B9%E8%81%8C%E5%B7%A5%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
    2. [1.12.2. 12.3 修改函数实现、](#12-3-%E4%BF%AE%E6%94%B9%E5%87%BD%E6%95%B0%E5%AE%9E%E7%8E%B0%E3%80%81)
  13. [1.13. 13. 查找职工](#13-%E6%9F%A5%E6%89%BE%E8%81%8C%E5%B7%A5)

    1. [1.13.1. 13.1 查找职工函数声明](#13-1-%E6%9F%A5%E6%89%BE%E8%81%8C%E5%B7%A5%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
  14. [1.14. 14. 排序](#14-%E6%8E%92%E5%BA%8F)

    1. [1.14.1. 14.1 排序函数声明](#14-1-%E6%8E%92%E5%BA%8F%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
  15. [1.15. 15. 清空文件](#15-%E6%B8%85%E7%A9%BA%E6%96%87%E4%BB%B6)

    1. [1.15.1. 15.1 清空函数声明](#15-1-%E6%B8%85%E7%A9%BA%E5%87%BD%E6%95%B0%E5%A3%B0%E6%98%8E)
    2. [1.15.2. 15.3 清空函数实现](#15-3-%E6%B8%85%E7%A9%BA%E5%87%BD%E6%95%B0%E5%AE%9E%E7%8E%B0)**最新文章[C++面向对象编程](/2022/06/14/cpp-oop/)2022-06-14[职工管理系统](/2022/06/12/staff-management/)2022-06-12[机房预约系统](/2022/06/11/lab-reservation/)2022-06-11[c++提高编程](/2022/06/10/cpp-improved/)2022-06-10© 2025 By dre框架 [Hexo 8.0.0](https://hexo.io)|主题 [Butterfly 5.5.2](https://github.com/jerryc127/hexo-theme-butterfly)************