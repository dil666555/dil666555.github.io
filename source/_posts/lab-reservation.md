---
title: 机房预约系统
date: 2022-06-11 00:00:00
updated: 2025-11-13 00:59:33
categories:
  - C++
tags:
  - C++
  - 学习笔记
---

# 机房预约系统

## 1. 机房预约系统需求

### 1.1 系统简介

### 1.2 身份简介

- **学生：**申请使用机房

- **教师：**审核学生的预约申请

- **管理员：**给学生，老师创建账号

### 1.3 机房简介

- **1号机房：**容量20人

- **2号机房：**容量50人

- **3号机房：**容量100人

### 1.4 申请简介

- 申请记录每周由管理员清空

- 学生可以预约未来一周内的机房使用，预约的日期为周一到周五，预约是需要选择预约时段（上午，下午）

- 教师来审核预约

### 1.5 系统需求

**登录界面：**

- 学生，老师，管理员，退出

**身份验证后进入子菜单：**

1. 学生：学号，姓名，密码

2. 老师：教工号，姓名，密码

3. 管理员：姓名，密码

**学生具体功能：**

1. 申请预约

2. 查看自身预约  

3. 查看所有预约  —  查看所有预约信息及预约状态

4. 取消预约  —  成功或审核中都可取消

5. 注销登录

**教师具体功能：**

1. 查看所有预约

2. 审核预约

3. 注销登录

**管理员具体功能：** 

1. 添加账号  —  添加学生或教师账号，需检测是否重复

2. 查看账号  —  可以选择查看学生或教师全部信息

3. 查看机房  —  查看所有机房的信息

4. 清空预约

5. 注销登录

## 2. 创建项目

## 3. 创建主菜单

### 3.1 菜单实现

在main函数中添加菜单提示

```cpp
#include<iostream>
using namespace std;

int main() {

	while (true) {

		//打印主菜单
		cout <<endl<< "================== 欢迎使用机房预约系统 ==================" << endl;
		cout << endl << "请选择您的身份" << endl;
		cout << "\t\t --------------------------" << endl;
		cout << "\t\t|                          |" << endl;
		cout << "\t\t|          1.学  生        |" << endl;
		cout << "\t\t|                          |" << endl;
		cout << "\t\t|          2.老  师        |" << endl;
		cout << "\t\t|                          |" << endl;
		cout << "\t\t|          3.管理员        |" << endl;
		cout << "\t\t|                          |" << endl;
		cout << "\t\t|          0.退  出        |" << endl;
		cout << "\t\t|                          |" << endl;
		cout << "\t\t -------------------------" << endl;
		cout << "输入您的选择：" << endl;
		int select = 6;
	
		cin >> select;
		switch (select)
		{
		case 1:
			break;
		case 2:
			break;
		case 3:
			break;
		case 0:
			break;
		default:
			cout << "输入错误，没有该选项" << endl;
			system("pause");
			system("cls");
			break;
		}
	}
	return 0;
}
```

### 4. 退出功能实现

```cpp
case 0:  //退出
	cout << "欢迎下次使用" << endl;
	system("pause");
	return 0;
```

### 5. 创建身份类

#### 5.1 身份的基类

- 在整个系统中，有三种身份，分别为学生，老师，管理员

- 三种身份有共性也有特性，因此可以抽象出一个身份基类identity

- 在头文件下创建identity.h文件

identity.h

```cpp
#pragma once
#include<iostream>
using namespace std;

//身份抽象基类
class Identity {
public:
	virtual void openMenu() = 0;//纯虚函数，无法实例化对象
	//姓名
	string m_Name;
	//密码
	string m_Pwd;
};
```

#### 5.2 创建学生类

student.h文件

```cpp
#pragma once
#include"identity.h"
#include<iostream>
using namespace std;

class Student :public Identity {
public:
	
	//默认构造
	Student();

	//有参构造
	Student(int id, string name, string pwd);

	//菜单界面
	virtual void openMenu();

	//申请预约
	void applyOrder();

	//查看自身预约
	void showMyorder();

	//查看所有预约
	void showAllorder();

	//取消预约
	void cancelOrder();

	//学号
	int m_Id;

};
```

student.cpp文件

```cpp
#include<iostream>
using namespace std;
#include"student.h"

//默认构造
Student::Student() {

}

//有参构造
Student::Student(int id, string name, string pwd) {

}

//菜单界面
void Student::openMenu() {

}

//申请预约
void Student::applyOrder() {

}

//查看自身预约
void Student::showMyorder() {

}

//查看所有预约
void Student::showAllorder() {

}

//取消预约
void Student::cancelOrder() {

}
```

#### 5.3 教师类

##### 5.3.1 功能分析

教师类主要功能是查看学生预约，并进行审核

教师类中主要功能有：

- 显示教师操作的菜单界面

- 查看所有预约

- 审核预约

##### 5.3.2 类的创建

teacher.h文件

```cpp
#pragma once
#include <iostream>
using namespace std;
#include "identity.h"

class Teacher :public Identity {
public:

	//默认构造
	Teacher();

	//有参构造
	Teacher(int id, string name, string pwd);

	//菜单界面
	virtual void openMenu();

	//查看所有预约
	void showAllorder();

	//审核预约
	void validOrder();

	//职工号
	int m_id;
};
```

teacher.cpp文件

```cpp
#include<iostream>
using namespace std;
#include"teacher.h"

//默认构造
Teacher::Teacher() {

}

//有参构造
Teacher::Teacher(int id, string name, string pwd) {

}

//菜单界面
void Teacher::openMenu() {

}

//查看所有预约
void Teacher::showAllorder() {

}

//审核预约
void Teacher::validOrder() {

}
```

#### 5.4 管理员类

##### 5.4.1 功能分析

管理员类主要功能是对学生和教师账号进行管理，查看机房预约信息以及清空预约记录

主要功能有：

1. 显示管理员菜单界面

2. 添加账号

3. 查看账号

4. 查看机房信息

5. 清空预约记录

##### 5.4.2 类的创建

manager.h文件

```cpp
#pragma once
#include<iostream>
using namespace std;
#include"identity.h"

class Manager :public Identity {
public:

	//默认构造
	Manager();

	//有参构造
	Manager(string name, string pwd);
	
	//选择菜单
	virtual void openMenu();

	//添加账号
	void addPerson();

	//查看账号
	void showPerson();

	//查看机房信息
	void showComputer();

	//清空预约记录
	void cleanFile();
};
```

manager.cpp文件

```cpp
#include <iostream>
using namespace std;
#include "manager.h"

//默认构造
Manager::Manager() {

}

//有参构造
Manager::Manager(string name, string pwd) {

}

//选择菜单
void Manager::openMenu() {

}

//添加账号
void Manager::addPerson() {

}

//查看账号
void Manager::showPerson() {

}

//查看机房信息
void Manager::showComputer() {

}

//清空预约记录
void Manager::cleanFile() {

}
```
