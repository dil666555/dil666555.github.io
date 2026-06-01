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

**文章作者: [dre](https://dil666555.github.io)**文章链接: [https://dil666555.github.io/2022/06/11/lab-reservation/](https://dil666555.github.io/2022/06/11/lab-reservation/)**版权声明: 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) 许可协议。转载请注明来源 [dre](https://dil666555.github.io)！[C++](/tags/C/)[学习笔记](/tags/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/)[上一篇职工管理系统职工管理系统1. 管理系统需求职工管理系统可以用来管理公司所有员工的信息 目标：使用c++实现一个基于多态的职工管理系统 公司职工分为三类：普通员工，经理，老板，显示信息时需要显示职工编号，职工姓名，职工岗位，以及职责  普通员工职责：完成经理交给的任务  经理职责：完成老板交给的任务，并下发任务给员工  老板职责：管理公司所有事物   管理系统需要实现的功能如下：  退出管理程序：退出当前管理系统  增加职工信息：实现批量添加职工功能，将信息录入到文件中，职工信息为：职工编号，姓名，部门编号  显示职工信息：显示公司内部所有职工信息  删除离职职工：按照编号删除指定的职工  修改职工信息：按照编号修改职工个人信息  查找职工信息：按照职工编号过着职工的姓名进行查找相关的人员信息  按照编号排序：按照职工编号，进行排序，排序规则由用户指定  清空所有文档：清空文件中记录的所有职工信息（清空前需要再次确定，防止误删）   2. 创建项目3. 创建管理类管理类负责内容如下：  与用户的沟通菜单界面 对职工更删改查的操作 与文件的读写交互  3.1 创建文件3.2 头文件实现12345...](/2022/06/12/staff-management/)[下一篇c++提高编程c++提高编程主要针对c++范式编程以及STL技术 1.模板1.1 模板的概念模板就是建立通用的模具看，大大提高复用性 例如生活中的一寸照片和PPT模板 模板的特点：  模板不可以直接使用，它只是一个框架 模板的通用并不是万能的  1.2 函数模板 c++另一种编程思想称为泛型编程，主要利用的技术就是模板 c++提供两种模板机制：函数模板和类模板  1.2.1 函数模板语法函数模板作用： 建立一个通用函数，其函数返回值和形参类型可以不指定，用一个虚拟的类型来代替即可 语法： 12template<typename T>//其中T就代表了一个虚拟的类型，名称可以更换//函数定义或语法  解释：template—表示要创建一个模板了 typename–表示其后面的符号是一种数据类型，可以用class来代替 T        –        表示一种通用的数据类型，名称可以更改，通常为大写字母 实例： 12345678910111213141516171819202122232425262728293031323334353637383940414243444546474...](/2022/06/10/cpp-improved/)**相关推荐[** 2022-06-10c++提高编程c++提高编程主要针对c++范式编程以及STL技术 1.模板1.1 模板的概念模板就是建立通用的模具看，大大提高复用性 例如生活中的一寸照片和PPT模板 模板的特点：  模板不可以直接使用，它只是一个框架 模板的通用并不是万能的  1.2 函数模板 c++另一种编程思想称为泛型编程，主要利用的技术就是模板 c++提供两种模板机制：函数模板和类模板  1.2.1 函数模板语法函数模板作用： 建立一个通用函数，其函数返回值和形参类型可以不指定，用一个虚拟的类型来代替即可 语法： 12template<typename T>//其中T就代表了一个虚拟的类型，名称可以更换//函数定义或语法  解释：template—表示要创建一个模板了 typename–表示其后面的符号是一种数据类型，可以用class来代替 T        –        表示一种通用的数据类型，名称可以更改，通常为大写字母 实例： 12345678910111213141516171819202122232425262728293031323334353637383940414243444546474...](/2022/06/10/cpp-improved/)[** 2022-06-14C++面向对象编程c++面向对象编程1. 内存分区模型C＋＋程序在执行时，将内存大方向划分为4个区域  代码区：存放函数体的二进制代码，由操作系统进行管理的  全局区：存放全局变量和静态变量以及常量  ·栈区：由编译器自动分配释放，存放函数的参数值，局部变量等  ·堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收   1.1 程序运行前在程序编译后，生成了exe可执行程序，未执行该程序前分为两个区域 代码区： ​	存放CPU 执行的机器指令 ​	代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可，代码区是只读的，使其只读的原因是防止程序意外地修改了它的指令 全局区： ​	全局变量和静态变量存放在此. ​	全局区还包含了常量区，字符串常量和其他常量（const修饰的变量）也存放在此. ​	该区域的数据在程序结束后由操作系统释放. 1.2 程序运行后栈区： 由编译器自动释分配释放，存放函数的参数值，局部变量等； 注意：不要返回局部变量的地址，栈区开辟的地址由编译器自动释放; 123456789int* fuct(){	int a=10;    r...](/2022/06/14/cpp-oop/)[** 2022-06-12职工管理系统职工管理系统1. 管理系统需求职工管理系统可以用来管理公司所有员工的信息 目标：使用c++实现一个基于多态的职工管理系统 公司职工分为三类：普通员工，经理，老板，显示信息时需要显示职工编号，职工姓名，职工岗位，以及职责  普通员工职责：完成经理交给的任务  经理职责：完成老板交给的任务，并下发任务给员工  老板职责：管理公司所有事物   管理系统需要实现的功能如下：  退出管理程序：退出当前管理系统  增加职工信息：实现批量添加职工功能，将信息录入到文件中，职工信息为：职工编号，姓名，部门编号  显示职工信息：显示公司内部所有职工信息  删除离职职工：按照编号删除指定的职工  修改职工信息：按照编号修改职工个人信息  查找职工信息：按照职工编号过着职工的姓名进行查找相关的人员信息  按照编号排序：按照职工编号，进行排序，排序规则由用户指定  清空所有文档：清空文件中记录的所有职工信息（清空前需要再次确定，防止误删）   2. 创建项目3. 创建管理类管理类负责内容如下：  与用户的沟通菜单界面 对职工更删改查的操作 与文件的读写交互  3.1 创建文件3.2 头文件实现12345...](/2022/06/12/staff-management/)![avatar](/images/profile.jpg)dredre 的博客[文章4](/archives/)[标签2](/tags/)[分类1](/categories/)[**Follow Me](https://github.com/dil666555)[**](https://github.com/dil666555)**公告This is my Blog**目录

1. [1. 机房预约系统](#%E6%9C%BA%E6%88%BF%E9%A2%84%E7%BA%A6%E7%B3%BB%E7%BB%9F)

  1. [1.1. 1. 机房预约系统需求](#1-%E6%9C%BA%E6%88%BF%E9%A2%84%E7%BA%A6%E7%B3%BB%E7%BB%9F%E9%9C%80%E6%B1%82)

    1. [1.1.1. 1.1 系统简介](#1-1-%E7%B3%BB%E7%BB%9F%E7%AE%80%E4%BB%8B)
    2. [1.1.2. 1.2 身份简介](#1-2-%E8%BA%AB%E4%BB%BD%E7%AE%80%E4%BB%8B)
    3. [1.1.3. 1.3 机房简介](#1-3-%E6%9C%BA%E6%88%BF%E7%AE%80%E4%BB%8B)
    4. [1.1.4. 1.4 申请简介](#1-4-%E7%94%B3%E8%AF%B7%E7%AE%80%E4%BB%8B)
    5. [1.1.5. 1.5 系统需求](#1-5-%E7%B3%BB%E7%BB%9F%E9%9C%80%E6%B1%82)
  2. [1.2. 2. 创建项目](#2-%E5%88%9B%E5%BB%BA%E9%A1%B9%E7%9B%AE)
  3. [1.3. 3. 创建主菜单](#3-%E5%88%9B%E5%BB%BA%E4%B8%BB%E8%8F%9C%E5%8D%95)

    1. [1.3.1. 3.1 菜单实现](#3-1-%E8%8F%9C%E5%8D%95%E5%AE%9E%E7%8E%B0)
    2. [1.3.2. 4. 退出功能实现](#4-%E9%80%80%E5%87%BA%E5%8A%9F%E8%83%BD%E5%AE%9E%E7%8E%B0)
    3. [1.3.3. 5. 创建身份类](#5-%E5%88%9B%E5%BB%BA%E8%BA%AB%E4%BB%BD%E7%B1%BB)

      1. [1.3.3.1. 5.1 身份的基类](#5-1-%E8%BA%AB%E4%BB%BD%E7%9A%84%E5%9F%BA%E7%B1%BB)
      2. [1.3.3.2. 5.2 创建学生类](#5-2-%E5%88%9B%E5%BB%BA%E5%AD%A6%E7%94%9F%E7%B1%BB)
      3. [1.3.3.3. 5.3 教师类](#5-3-%E6%95%99%E5%B8%88%E7%B1%BB)

        1. [1.3.3.3.1. 5.3.1 功能分析](#5-3-1-%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90)
        2. [1.3.3.3.2. 5.3.2 类的创建](#5-3-2-%E7%B1%BB%E7%9A%84%E5%88%9B%E5%BB%BA)
      4. [1.3.3.4. 5.4 管理员类](#5-4-%E7%AE%A1%E7%90%86%E5%91%98%E7%B1%BB)

        1. [1.3.3.4.1. 5.4.1 功能分析](#5-4-1-%E5%8A%9F%E8%83%BD%E5%88%86%E6%9E%90)
        2. [1.3.3.4.2. 5.4.2 类的创建](#5-4-2-%E7%B1%BB%E7%9A%84%E5%88%9B%E5%BB%BA)**最新文章[C++面向对象编程](/2022/06/14/cpp-oop/)2022-06-14[职工管理系统](/2022/06/12/staff-management/)2022-06-12[机房预约系统](/2022/06/11/lab-reservation/)2022-06-11[c++提高编程](/2022/06/10/cpp-improved/)2022-06-10© 2025 By dre框架 [Hexo 8.0.0](https://hexo.io)|主题 [Butterfly 5.5.2](https://github.com/jerryc127/hexo-theme-butterfly)************