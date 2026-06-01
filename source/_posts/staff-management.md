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
