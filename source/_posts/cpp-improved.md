---
title: c++提高编程
date: 2022-06-10 00:00:00
updated: 2025-11-13 00:59:28
categories:
  - C++
tags:
  - C++
  - 学习笔记
---

# c++提高编程

主要针对c++范式编程以及STL技术

## 1.模板

### 1.1 模板的概念

模板就是建立**通用的模具**看，大大提高**复用性**

例如生活中的一寸照片和PPT模板

模板的特点：

- 模板不可以直接使用，它只是一个框架

- 模板的通用并不是万能的

### 1.2 函数模板

- c++另一种编程思想称为泛型编程，主要利用的技术就是模板

- c++提供两种模板机制：函数模板和类模板

#### 1.2.1 函数模板语法

函数模板作用：

建立一个通用函数，其函数返回值和形参类型可以不指定，用一个虚拟的类型来代替即可

语法：

```cpp
template<typename T>//其中T就代表了一个虚拟的类型，名称可以更换
//函数定义或语法
```

解释：template—表示要创建一个模板了

typename–表示其后面的符号是一种数据类型，可以用class来代替

T        –        表示一种通用的数据类型，名称可以更改，通常为大写字母

实例：

```cpp
//定义一个两个数据进行交换数据的函数

//传统交换函数
void swapInt(int& a, int& b) {//01
	int temp = a;
	a = b;
	b = temp;
}

void swapDouble(double& a, double& b) {//02，可以看出，只要交换的两个数数据类型发生改变，那么就要重写函数
	double temp = a;
	a = b;
	b = temp;
}

//模板交换函数
template<typename T>
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

void test01() {//用来测试传统交换函数
	int a = 10;
	int b = 20;

	swapInt(a, b);
	cout << "传统：a原值是10，交换后为：" << a << endl;
	cout << "传统：b原值是20，交换后为：" << b << endl;

	double c = 1.1;
	double d = 2.2;

	swapDouble(c, d);
	cout << "传统：c原值是1.1，交换后为：" << c << endl;
	cout << "传统：d原值是2.2，交换后为：" << d << endl;

}

void test02() {//用来测试模板交换函数
	int a = 10;
	int b = 20;

	mySwap(a, b);//使用模板的第一种方法：自动类型推导
	cout << "模板：a原值是10，交换后为：" << a << endl;
	cout << "模板：b原值是20，交换后为：" << b << endl;

	double c = 1.1;
	double d = 2.2;

	mySwap<double>(c, d);//使用模板第二中方法：显示指定类型
	cout << "模板：c原值是1.1，交换后为：" << c << endl;
	cout << "模板：d原值是2.2，交换后为：" << d << endl;
}

int main() {
	test01();
	cout << endl;
	test02();
	return 0;
}
```

总结：

- 函数模板关键字：template

- 使用函数模板的两种方法：自动类型推导，显示指定类型            

- 模板的目的是为了提高复用性，将类型参数化

#### 1.2.2 函数模板注意事项

- 自动类型推导，必须要推导出一致的数据类型

- 必须要推导出T的数据类型才可以使用模板

示例：

```cpp
//模板交换函数
template<typename T>
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

template<typename T>
void func() {
	cout << "func被调用了。" << endl;
}

void test01() {//用来测试模板交换函数
	int a = 10;
	int b = 20;
	char c = 'a';

	//注意事项01，自动类型推导必须要推导出一致的数据类型T，才可以使用
	mySwap(a, b);//正确
	//mySwap(a, c);错误，数据类型不统一

	//注意事项02，T必须要推导出数据类型才可以使用
	//func();错误，不清楚T是什么数据类型
	func<int>();//正确，T为int

}

int main() {
	test01();
	return 0;
}
```

总结：

使用模板是必须要能推导出一直的数据类型T

#### 1.2.3 函数模板案例

案例描述：

- 通过模板封装出一个排序函数

- 排序方法从大到小，排序算法为选择排序

- 分别用char和int进行验证

示例：

```cpp
#include<iostream>
using namespace std;

//模板交换函数
template<typename T>
void mySwap(T& a, T& b) {
	T temp = a;
	a = b;
	b = temp;
}

//模板排序函数
template<typename T>
void mySort(T arr[],int len) {
	for (int i = 0; i < len; i++) {
		int max = i;
		for (int j = i + 1; j < len; j++) {
			if (arr[max] < arr[j]) {
				max = j;
			}
		}
		if (max != i) {
			mySwap(arr[i], arr[max]);
		}
	}
}

//模板打印函数
template<typename T>
void myPrintf(T arr[], int len) {
	for (int i = 0; i < len; i++) {
		cout << arr[i];
	}
	cout << endl;
}

void test01() {
	char s[] = "badcfe";
	int num1 = sizeof(s) / sizeof(char);
	mySort(s, num1);
	myPrintf(s, num1);
	int a[] = { 3,2,5,1,6,7,8,6 };
	int num2 = sizeof(a) / sizeof(int);
	mySort(a, num2);
	myPrintf(a, num2);
}

int main() {
	test01();
	return 0;
}
```

#### 1.2.4 普通函数和函数模板的区别

**普通函数和函数模板的区别：**

- 普通函数在调用时可以发生自动类型转换（隐式类型转换）

- 函数模板在调用时如果是自动推导类型是不能发生自动类型转换的

- 函数模板在调用时如果是显示指定类型是可以发生自动类型转换的

示例：

```cpp
//普通函数类型
int func1(int a, int b) {
	return a + b;
}

//模板函数类型
template<typename T>
int func2(T a, T b) {
	return a + b;
}

void test01() {
	int a = 10;
	char b = 'c';
	//普通函数类型
	cout << func1(a, b) << endl;;//不会报错，会自动将char转换为int
	//cout << func2(a, b);//报错，a,b识别出来的类型不一致
	cout << func2<int>(a, b);//不会报错，会将b转换为int
}

int main() {
	test01();
	return 0;
}
```

#### 1.2.5 普通函数和函数模板的调用规则

**普通函数和函数模板的调用规则**：

- 普通函数和函数模板都可以调用时优先调用普通函数

- 可以通过空模板参数列表来强制调用函数模板

- 函数模板可以发生重载

- 当函数模板匹配性更好时，优先调用函数模板

示例：

```cpp
//普通函数类型
void func(int a, int b) {
	cout << "调用普通函数" << endl;
}

//模板函数类型
template<typename T>
void func(T a, T b) {
	cout << "调用函数模板" << endl;
}

//模板函数重载
template<typename T>
void func(T a, T b, T c) {
	cout << "调用模板重载" << endl;
}

void test01() {
	func(1, 1);//普通函数和函数模板都可以调用时优先调用普通函数
	func<>(1, 1);// 可以通过空模板参数列表来强制调用函数模板
	func(1, 1, 1);//函数模板可以发生重载
	func('a', 'b');//当函数模板匹配性更好时，优先调用函数模板
}

int main() {
	test01();
	return 0;
}
```

运行结果：

调用普通函数

调用函数模板

调用模板重载

调用函数模板

#### 1.2.6 模板的局限性

局限性：模板的通用性并不是万能的

例如：如果 a , b是两个数组的话就无法判断了

```cpp
template<typename T>
bool judge(T a,T b) {
	if(a == b) {
		return true;
	}
	else {
		return false;
	}
}
```

因此c++为了解决这些问题，提供了具体类型的模板的重载来解决问题

```cpp
class person {
public:
	person(string name, int age) {
		this->m_name = name;
		this->m_age = age;
	}
	string m_name;
	int m_age;
};

//判断两个数据类型是否相等的模板‘
template<typename T>
bool judge(T& a, T& b) {
	if (a == b) {
		return true;
	}
	else {
		return false;
	}
}

//对于特殊的数据类型提供重载
//具体化优先于常规模板
template<> bool judge(person& a, person& b) {
	if (a.m_name == b.m_name && a.m_age == b.m_age) {
		return true;
	}
	else {
		return false;
	}
}

void test01() {
	int a0 = 10;
	int b0 = 10;
	if (judge(a0, b0)) {
		cout << "相等" << endl;
	}
	else {
		cout << "不相等" << endl;
	}

	person a("张三", 18);
	person b("张三", 18);
	if (judge(a, b)) {
		cout << "相等" << endl;
	}
	else {
		cout << "不相等" << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

总结：

- 利用具体化模板可以解决自定义类型的通用化

- 学习模板并不是为了写模板，而是为了使用提供的STL模板

### 1.3 类模板

#### 1.3.1 类模板语法

类模板的作用：**建立一个通用类，类中的成员数据类型可以不确定，用一个通用的虚拟类来表示**

**语法：**

```cpp
template<class T>
类
```

解释：

template：表示后面要创建一个模板了

class：表示后面是一个虚拟的数据类型，可以用typename来表示

T：表示一个虚拟的数据类型，通常用大写字母来表示

示例：

```cpp
template<class NameType,class AgeType>
class person {
public:
	person(NameType name, AgeType age) {
		m_name = name;
		m_age = age;
	}
	void showPerson() {
		cout << "名称：" << this->m_name << "年龄：" << this->m_age << endl;
	}
	NameType m_name;
	AgeType m_age;
};

void test01() {
	person<string, int> a("张三", 18);
	a.showPerson();
}

int main() {
	test01();
	return 0;
}
```

总结：类模板和函数模板基本相同，在template后面加类就是类模板

#### 1.3.2 类模板和函数模板的区别

类模板和函数模板的区别主要有两种：

- 类模板没有自动类型推导的使用方法

- 类模板在模板参数列表中可以有默认参数

示例：

```cpp
template<class NameType,class AgeType=int>//类模板在模板参数列表中可以有默认参数
class person {
public:
	person(NameType name, AgeType age) {
		m_name = name;
		m_age = age;
	}
	void showPerson() {
		cout << "名称：" << this->m_name << "年龄：" << this->m_age << endl;
	}
	NameType m_name;
	AgeType m_age;
};

void test01() {
	person<string> a("张三", 18);//也不会报错，因为我们为AgeType赋了默认参数
	a.showPerson();
}

int main() {
	test01();
	return 0;
}
```

#### 1.3.3 类模板中的成员函数创建时机

类模板中的成员函数和普通类中的成员函数创建时机是不同的：

- 类模板中的成员函数是在调用时才会创建

- 普通类中的成员函数是在一开始就会创建

示例：

```cpp
class person1 {
public:
	void func1() {
		cout << "func1被调用" << endl;
	}
};

class person2 {
public:
	void func2() {
		cout << "func2被调用" << endl;
	}
};

template<class T>
class myClass {
public:

	void f1() {
		obj.func1();
	}

	void f2() {
		obj.func2();
	}

	T obj;
};

void test01() {
	myClass<person1> a;
	a.f1();//可以运行，虽然void f2()由语法错误，但由于没有调用，所以没关系
}

int main() {
	test01();
	return 0;
}
```

总结：类模板中的成员函数并不是一开始就创建的，而是在调用时才会创建

#### 1.3.4 类模板做函数参数

学习目标：

类模板示例化后的对象，作为参数向函数中传入的方式

一共有三种方法：

1. 指定传入类型：直接表明对象的数据类型

2. 参数模板化：将对象中的参数模板化后传递

3. 整个类模板化：将这个对象类型模板化后传递

示例：

```cpp
template<class T1,class T2>
class person {
public:
	person(T1 name, T2 age) {
		this->m_Name = name;
		this->m_Age = age;
	}
	void showPerson() {
		cout << m_Name << "的年龄是" << m_Age << endl;
	}

	T1 m_Name;
	T2 m_Age;
};

//01指定传入类型
void func1(person<string, int>& p) {
	p.showPerson();
}
void test01() {
	person<string, int> a("张三",17);
	func1(a);
}

//02参数模板化
template<class T1,class T2>
void func2(person<T1, T2>& p) {
	p.showPerson();
	cout << "T1的数据类型为：" << typeid(T1).name() << endl;
	cout << "T2的数据类型为：" << typeid(T2).name() << endl;
}
void test02() {
	person<string, int> b("李四", 12);
	func2(b);
}

//将创建对象的类模板化
template<class T>
void func3(T& p) {
	p.showPerson();
	cout << "T 的数据类型为：" << typeid(T).name() << endl;
}
void test03() {
	person<string, int> c("王五", 19);
	func3(c);
}

int main() {
	test01();
	cout << "---------------------------------------------------------------------------------" << endl;
	test02();
	cout << "---------------------------------------------------------------------------------" << endl;
	test03();
	return 0;
}
```

运行结果：

张三的年龄是17

李四的年龄是12

T1的数据类型为：class std::basic_string<char,struct std::char_traits,class std::allocator >

T2的数据类型为：int

王五的年龄是19

T 的数据类型为：class person<class std::basic_string<char,struct std::char_traits,class std::allocator >,int>

#### 1.3.5 类模板与继承

注意：

子类中必须要指定父类中T的类型

否则无法给子类分配内存

要想灵活的指定父类中T的数据类型，那么可以把子类也指定为一个类模板

示例：

```cpp
template<class T>
class base {
public:
	
	T obj;
};

class son :public base<int> {//必须要有<int>否则无法成功
public:
	son() {
		cout << "son被调用了" << endl;
	}
};

//如果想更加的灵活
template<class T1,class T2>
class son1 :public base<T1> {
public:
	son1() {
		cout << "son1中T1的数据类型为" << typeid(T1).name() << endl;
		cout << "son1中T2的数据类型为" << typeid(T2).name() << endl;
	}
	T2 obj;
};

void test() {
	son a;
	cout << "--------------------------------------\n";
	son1<int, char> p;
}

int main() {
	test();
	return 0;
}
```

总结：如果父类是模板，子类需要指出父类中T的数据类型

#### 1.3.6 类模板中的成员函数类外实现

学习目标：能够掌握类模板中的成员函数类外实现

示例：

```cpp
template<class T1,class T2>
class person {
public:
	person(T1 name, T2 age);
	void showPerson();
	T1 m_Name;
	T2 m_Age;
};

template<class T1,class T2>//注意格式。
person<T1,T2>::person(T1 name, T2 age) {//作用域后加尖括号
	this->m_Name = name;
	this->m_Age = age;
}

template<class T1,class T2>
void person<T1, T2>::showPerson() {
	cout << this->m_Name << "的年龄是" << this->m_Age << "岁" << endl;
}

void test() {
	person<string, int> p("张三", 13);
	p.showPerson();
}

int main() {
	test();
	return 0;
}
```

总结：类模板成员函数类外实现时，需要加上模板参数列表

#### 1.3.7 类模板分文件编写

问题：

类模板中的成员函数的创建时机是在调用阶段，导致分文件编写时链接不到

解决：

方法一：直接包含.cpp文件

```cpp
//源.cpp文件
#include<iostream>
using namespace std;
#include<string>
#include"person.cpp"//直接包含.cpp文件

void test() {
	person<string, int> p("张三", 13);
	p.showPerson();
}

int main() {
	test();
	return 0;
}

//person.h文件
#pragma once
#include<iostream>
using namespace std;
#include<string>

template<class T1, class T2>
class person {
public:
	person(T1 name, T2 age);
	void showPerson();
	T1 m_Name;
	T2 m_Age;
};

//person.cpp文件
#include"person.h"

template<class T1, class T2>
person<T1, T2>::person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

template<class T1, class T2>
void person<T1, T2>::showPerson() {
	cout << this->m_Name << "的年龄是" << this->m_Age << "岁" << endl;
}
```

方法二（常用）：将声明和实现都放在同一个.hpp文件中

```cpp
//源.cpp文件
#include<iostream>
using namespace std;
#include<string>
#include"person.hpp"//包含.hpp文件后即可

void test() {
	person<string, int> p("张三", 13);
	p.showPerson();
}

int main() {
	test();
	return 0;
}

//person.hpp文件，声明和实现都放在一起
#pragma once
#include<iostream>
using namespace std;
#include<string>

template<class T1, class T2>
class person {
public:
	person(T1 name, T2 age);
	void showPerson();
	T1 m_Name;
	T2 m_Age;
};

template<class T1, class T2>
person<T1, T2>::person(T1 name, T2 age) {
	this->m_Name = name;
	this->m_Age = age;
}

template<class T1, class T2>
void person<T1, T2>::showPerson() {
	cout << this->m_Name << "的年龄是" << this->m_Age << "岁" << endl;
}
```

总结：主流的解决方法是第二种

#### 1.3.8 类模板与友元

全局函数类内实现：直接在类内声明友元即可

```cpp
template<class T1,class T2>
class person {
	friend void showPerson(person<T1, T2> &p) {//全局函数类内实现
		cout << p.m_Name << p.m_Age << "岁" << endl;
	}
public:
	person(T1 name, T2 age) {
		m_Name = name;
		m_Age = age;
	}
private:
	T1 m_Name;
	T2 m_Age;
};

void test() {
	person<string, int> p("张三", 13);
	showPerson(p);
}

int main() {
	test();
	return 0;
}
```

全局函数类外实现：==需要提前让编译器知道全局函数的存在==

```cpp
template<class T1, class T2>
class person;//因为要让编译器提前知道全局函数的存在，又全局函数中用到了person，所以还要提前声明person
template<class T1, class T2>
void showPerson(person<T1, T2>& p) {
	cout << p.m_Name << p.m_Age << "岁" << endl;
}

template<class T1,class T2>
class person {
	friend void showPerson<>(person<T1, T2>& p);//因为在前面写全局函数时前面又template所以这个地方要加<>
public:
	person(T1 name, T2 age) {
		m_Name = name;
		m_Age = age;
	}
private:
	T1 m_Name;
	T2 m_Age;
};

void test() {
	person<string, int> p("张三", 13);
	showPerson(p);
}

int main() {
	test();
	return 0;
}
```

总结：建议使用类内实现

#### 1.3.9 类模板案例

案例描述：实现一个通用的数组类， 要求如下：

- 可以对内置数据类型和自定义数据类型数据进行存储，

- 将数组中的数据存放到堆区

- 构造函数中可以传入数组的容量

- 提供对应的拷贝构造函数与operator=来防止浅拷贝问题

- 提供尾插法和尾删法对数组中的数据进行增加和删除

- 可以通过下标的方式来实现对数组中元素的访问

- 可以获取数组中当前元素的个数和数组的容量

示例：myAarrayj.hpp文件中的代码

```cpp
template<class T>
class myArray {
public:
	myArray(int capacity) {
		cout << "myArray的构造函数调用" << endl;
		this->m_myAddress = NULL;
		this->m_capacity = capacity;
		this->m_size = 0;
	}

	myArray(const myArray& arr) {
		cout<<"myArray的拷贝构造函数被调用" << endl;
		this->m_capacity = arr.m_capacity;
		this->m_size = arr.m_size;
		this->m_myAddress = new T[arr.m_capacity];//深拷贝
		for (int i = 0; i < this->m_size; i++) {
			this->m_myAddress[i] = arr.m_myAddress[i];
		}
	}

	myArray& operator=(const myArray& arr) {
		cout << "myArray的operator=被调用" << endl;
		//先判断原来堆区是否又数据，有的话先释放干净
		if (this->m_myAddress != NULL) {
			delete[] this->m_myAddress;
			this->m_myAddress = NULL;
			this->m_capacity = 0;
			this->m_size = 0;
		}

		//深拷贝
		this->m_capacity = arr.m_capacity;
		this->m_size = arr.m_size;
		this->m_myAddress = new T[arr.m_capacity];
		for (int i = 0; i < this->m_size; i++) {
			this->m_myAddress[i] = arr.m_myAddress[i];
		}
		return *this;
	}

	//尾插法
	void push_back(const T& val) {
		if (this->m_capacity == this->m_size) {
			return;
		}
		this->m_myAddress[this->m_size] = val;
		this->m_size++;//更新数组大小
	}

	//尾删法
	void pop_back() {
		if (this->m_size == 0) {
			return;
		}
		this->m_size--;
	}

	T& operator[](int index) {
		return this->m_myAddress[index];
	}

	//返回数组容量
	int getcapacity() {
		return this->m_capacity;
	}
	
	//返回数组大小
	int getsize() {
		return this->m_size;
	}

	~myArray() {
		cout << "myArray的析构函数被调用" << endl;
		if (this->m_myAddress != NULL) {
			delete this->m_myAddress;
			this->m_myAddress = NULL;
		}
	}

private:
	T* m_myAddress;
	int m_capacity;
	int m_size;
};
```

## 2. STL初识

### 2.1 STL的诞生

- 长久一类，软件界一直希望提高代码的复用性

- c++的面向对象编程和范式编程思想一直就是为了这个目的

- 大多数情况下，数据结构和算法都没有一套标准，导致被迫从事大量重复工作

- 为了建立一套数据结构和算法的标准，诞生了STL

### 2.2 STL基本概念

- STL(Standard Template Library,标准模板库)

- STL从广义上分为容器(container)，算法(algorithm)，迭代器(iterator)

- 容器和算法之间通过迭代器进行无缝连接

- STL几乎所有的代码都用了模板类或模板函数

### 2.3 STL六大组件

STL大体分为六大组件，分别为：容器，算法，迭代器，仿函数，适配器（配接器），空间适配器

1. 容器：各种数据结构，如vector, list, deque, set, map等，用来存放数据

2. 算法：各种常用的算法，sort, find, copy, for_each等

3. 迭代器：扮演了容器和算法之间的胶合剂

4. 仿函数：行为类似函数，可作为算法的魔种策略

5. 适配器：一种用来修饰容器或仿函数或迭代器接口的东西

6. 空间适配器：负责空间的适配与管理

### 2.4 STL 中容器，算法，迭代器

容器：容物之所也

STL容器就是将运用最广泛的一些数据结构实现出来

常用的数据结构：数组，链表，树，栈，队列，集合，映射表等

这些容器分为序列式容器和关联式容器两种：

- 序列式容器：强调强调值的排序，序列式容器每个值都有自己固定的位置

- 关联式容器：二叉树结构，各元素没有严格的物理上的联系

算法：问题之解法也

有限的步骤，解决逻辑或数学上的问题，这一门学科我们称之为算法（Algorithms）

算法分为：质变算法和非质变算法

- 质变算法：是指运算过程中会改变区间内的元素的内容。例如：拷贝，替换，删除等

- 非质变算法：是指运算过程中不会改变区间内元素的内容。例如：查找，计数，遍历，查找极值等

迭代器：容器和算法之间的粘合剂

提供一种方法，是指能够依序寻访容器内的各个元素，而又无需暴露该容器的内部表达方式

每个容器都有自己的专属迭代器

迭代器的使用类似于指针

迭代器的种类：

种类
功能
支持运算

输入迭代器
对数据的制度访问
只读，支持：++，==，！=

输出迭代器
对数据的只写访问
只写，支持：++

前向迭代器
读写操作，并能向前推进迭代器
读写，支持：++，==，！=

双向迭代器
读写操作，并能向前和向后操作
读写，支持：++，–

随机访问迭代器
读写操作，可以任意的访问任意数据，功能最强的迭代器
读写，支持：++，–，[n]，-n，<，<=，>，>=

常用的迭代器为：双向迭代器，和随机访问迭代器

### 2.5 容器算法迭代器常识

#### 2.5.1 vector存放内置数据类型

容器：`vector`

算法：`for_each`

迭代器：`vector<int>::iterator`

示例：

```cpp
#include<vector>
#include<algorithm>

void myPrint(int val) {//使用遍历算法for_each时使用
	cout << val << endl;
}

void test() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);
	v.push_back(40);
	
	//第一种遍历方法
	vector<int>::iterator itBegin = v.begin();//起始迭代器，指向容器中的第一个元素
	vector<int>::iterator itEnd = v.end();//结束迭代器，指向容器中最后一个元素的下一个位置
	while (itBegin != itEnd) {
		cout << *itBegin << endl;
		itBegin++;
	}

	//第二种遍历方法
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << endl;
	}

	//第三种遍历方法
	for_each (v.begin(), v.end(), myPrint);

}

	

int main() {
	test();
	return 0;
}
```

运行结果：

102030401020304010203040

#### 2.5.2 vector存放自定义数据类型

学习目标：vector中存放自定义数据类型，并打印输出

```cpp
#include<vector>
#include<algorithm>

class person {
public:
	person(string name, int age) {
		m_name = name;
		m_age = age;
	}
	string m_name;
	int m_age;
};

void test() {
	person p1("a", 10);
	person p2("b", 20);
	person p3("c", 30);
	vector<person> p;
	p.push_back(p1);
	p.push_back(p2);
	p.push_back(p3);
	for (vector<person>::iterator it = p.begin(); it != p.end(); it++) {
		cout << "姓名：" << (*it).m_name << "年龄" << (*it).m_age << endl;
		//也可以将(*it).m_name替换成it->m_name;
	}
}

void test1() {
	person p1("a", 10);
	person p2("b", 20);
	person p3("c", 30);
	vector<person*> p;
	p.push_back(&p1);
	p.push_back(&p2);
	p.push_back(&p3);
	for (vector<person*>::iterator it = p.begin(); it != p.end(); it++) {
		cout << "姓名：" << (*it)->m_name << "年龄：" << (*it)->m_age << endl;
	}
}

int main() {
	test();
	test1();
	return 0;
}
```

#### 2.5.3 vector容器嵌套容器

示例：

```cpp
#include<vector>
#include<algorithm>

void test() {
	vector<vector<int>> v;
	vector<int> v1;
	vector<int> v2;
	vector<int> v3;
	for (int i = 0; i < 3; i++) {
		v1.push_back(i + 1);
		v2.push_back(i + 2);
		v3.push_back(i + 3);
	}
	v.push_back(v1);
	v.push_back(v2);
	v.push_back(v3);
	for (vector<vector<int>>::iterator it = v.begin(); it != v.end(); it++) {
		//容器：(*it)即为容器vector<int>
		for (vector<int>::iterator cv = (*it).begin(); cv != (*it).end(); cv++) {
			cout << *cv << " ";
		}
		cout << endl;
	}
}

int main() {
	test();
	return 0;
}
```

运行结果：

1 2 32 3 43 4 5

## 3. STL常用容器

### 3.1 string容器

#### 3.1.1 string基本概念

**本质**：string是c++风格的字符串，而string本质上是一个类

**string和char*的区别：**

char* 是一个指针

string是一个类，类内封装了`char*`管理这个字符串，是一个`char *`型的容器

**特点：**

string类内部封装了很多成员方法

例如：查找find，拷贝copy，删除delete，替换replace，插入insert

string管理char*分配的空间，不用担心复制越界和取值越界，由类内部负责

#### 3.1.2 string的构造函数

**构造函数原型：**

`string();`                                                  //创建一个空的字符串，例如string str；

`string(const char* s)`                         //使用字符串s初始化

`string(const string& str)`                 //使用一个string对象初始化另一个string对象

`string(int num,char c)`                        //使用num个字符c初始化

示例：

```cpp
void test() {
	string s1;//1.创建一个空的字符串，例如string str；

	string s2("Hello world!");//使用字符串s初始化
	cout << "s2:" << s2 << endl;

	string s3(s2);//使用一个string对象初始化另一个string对象
	cout << "s3:" << s3 << endl;

	string s4(3, 'a');
	cout << "s4:" << s4 << endl;
}

int main() {
	test();
	return 0;
}
```

#### 3.1.3 string赋值操作

功能描述：给string字符串进行赋值

赋值函数原型：

`string & operator=(const char* s);`                                       //将char*类型字符串赋值给当前的字符串

`string & operator=(const string& s);`                                   //把字符串s赋给当前字符串

`string & operator=(char c);`                                                     //字符赋给当前字符串

`string & assign(const char* s);`                                             //把字符串s赋给当前字符串

`string & assign(const char* s,int n);`                                //把字符串s的前n个字符赋给当前字符串

`string & assign(const string& s )；`                                     //把字符串string赋给当前字符串

`string & assign(int n,char c);`                                               //给字符串赋值n个字符c

示例：

```cpp
void test() {
	string s1;
	s1 = "hello world"; //将char*类型字符串赋值给当前的字符串
	cout << "s1:" << s1 << endl;

	string s2 = s1;//把字符串s赋给当前字符串
	cout << "s2:" << s2 << endl;

	string s3;
	s3 = 'a'; //字符赋给当前字符串
	cout << "s3:" << s3 << endl;

	string s4;
	s4.assign("Hello c++");//把字符串s赋给当前字符串
	cout << "s4:" << s4 << endl;

	string s5;
	s5.assign("Hello c++",5);//把字符串s的前n个字符赋给当前字符串
	cout << "s5:" << s5 << endl;

	string s6;
	s6.assign(s5);//把字符串string赋给当前字符串
	cout << "s6:" << s6 << endl;

	string s7;
	s7.assign(3, 'a');//给字符串赋值n个字符c
	cout << "s7:" << s7 << endl;
}

int main() {
	test();
	return 0;
}
```

总结：string赋值方法很多，operator=更加的实用

#### 3.1.4 string字符串拼接

**功能描述：**实现字符串末尾拼接字符串

**函数原型：**

`string & operator+=(const char* str);`                                            重载+=操作符

`string & operator+=(const char c);`                                                  重载+=操作符

`string & operator+=(const string& str);`                                        重载+=操作符

`string & append(const char* s);`                                                         将字符串s加到原字符串末尾

`string & append(const char* s,int n);`                                             将字符串s的前n个字符加到原字符串末尾

`string & append(const string & s);`                                                   将字符串s加到原字符串末尾

`string & append(const string & s,int pos,int n);`                    将字符串s的第post位置开始的n个字符加到原字符串末尾

示例：

```cpp
void test() {
	string s1="我";
	s1 += "爱玩游戏";//string & operator+=(const char* str);
	cout << s1 << endl;

	s1 += ':';//string & operator+=(const char c);
	cout << s1 << endl;

	string s2 = "LOL";
	s1 += s2;//string & operator+=(const string& str);
	cout << s1 << endl;

	s1.append(" DNF");//string & append(const char* s);
	cout << s1 << endl;

	s1.append(" CF和王者荣耀", 3);//string & append(const char* s,int n);
	cout << s1 << endl;

	string s3 = "玩游戏要适量";
	s1.append(s3);//`string & append(const string & s);` 
	cout << s1 << endl;

	string s4 = "玩游戏要适量，不要沉迷。";//string & append(const string & s,int pos,int n);
	s1.append(s4, 12, 12);//注意一个汉字占两个位置
	cout << s1 << endl;

}

int main() {
	test();
	return 0;
}
```

#### 3.1.5 string查找和替换

功能描述：

- 查找：查找指定字符串是否存在

- 替换：在指的定位置替换字符串

函数原型：

`int find(const string & str , int pos = 0) const;`

`int find(const char * s,int pos = 0) cosnt;`

`int find(const char * s, int pos ,int n) const;`

`int find(const char c, int pos=0) const;`

`int rfind(const string & str , int pos = npos) const;`

`int rfind(const char * s,int pos = npos) cosnt;`

`int rfind(const char * s, int pos ,int n) const;`

`int rfind(const char c, int pos=0) const;`

`string& replace(int pos,int n, const string& s);`

`string& replace(int pos,int n,const char* s);`

示例：

```cpp
void test() {//查找
	string s = "abcdefgde";
	int pos = s.find("de");
	if (pos == -1) {
		cout << "未找到" << endl;
	}
	else {
		cout << "位置pos=" << pos << endl;
	}
	//find是从左往右找，rfind是从右往左找
	pos = s.rfind("de");
	if (pos == -1) {
		cout << "未找到" << endl;
	}
	else {
		cout << "位置pos=" << pos << endl;
	}
}

void test1() {//替换
	string s = "abcdefg";
	s.replace(3, 3, "1111");
	cout << s << endl;
}

int main() {
	test();
	test1();
	return 0;
}
```

运行结果：

位置pos=3位置pos=7abc1111g

#### 3.1.6 string的字符串比较

功能描述：比较两个字符串是否相等以及ASCII的大小

比较方式：字符的比较通过ASCII比较

等于 返回  0；

大于 返回  1；

小于 返回 -1；

函数原型：

`int compare(const string &s ) const //与字符串s比较`

`int compare(const char* s) const //与字符串s比较`

示例：

```cpp
void test() {
	string s1 = "abcdefg";
	string s2 = "xbcdefg";
	if (s1.compare(s2) == 0) {
		cout << "s1等于s2" << endl;
	}
	else if (s1.compare(s2) > 0) {
		cout << "s1大于s2" << endl;
	}
	else {
		cout << "s1小于s2" << endl;
	}
}

int main() {
	test();
	return 0;
}
```

运行结果：

s1小于s2

#### 3.1.7 string字符存取

string中单个字符的存取方式有两种

- `char & operator[] (int n) ;      //通过[]来读取字符` 

- `char & at(int n);                //通过at来读取字符`

示例：

```cpp
void test() {
	string s;
	s = "Hello";
	for (int i = 0; i < s.size(); i++) {
		cout << s[i] << " ";
	}
	cout << endl;
	for (int i = 0; i < s.size(); i++) {
		cout << s.at(i) << " ";
	}
	cout << endl;
	s[0] = 'x';//修改
	for (int i = 0; i < s.size(); i++) {
		cout << s[i] << " ";
	}
	cout << endl;
	s.at(1) = 'x';
	for (int i = 0; i < s.size(); i++) {
		cout << s.at(i) << " ";
	}
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

H e l l oH e l l ox e l l ox x l l o

#### 3.1.8 string插入和删除

**功能描述：**

对string进行插入和删除功能

**函数原型：**

`string& insert(int pos, const char* s );`                //插入字符串

`string& insert(int pos, const string& s);`              //插入字符串

`string& insert(int pos, int n, char c);`                   //在指定位置插入n个字符c

`string& erase(int pos, int n=pos);`                              //删除从pos开始的n个字符

```cpp
void test() {
	string s;
	s = "Hllo";
	s.insert(1, "ee");
	cout << s << endl;
	s.erase(1,1);
	cout << s << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

Heello

Hello

#### 3.1.9 string子串获取

**功能描述：**

从字符串中获取想要的字串

**函数原型：**

`string sunstr(int pos = 0, int n = npos ) const;`返回由pos开始的n个字符组成的字符串

示例：

```cpp
void test() {
	string s;
	s = "Hello world";
	string subStr = s.substr(0, 5);
	cout << subStr << endl;
}

void test1() {//substr的实用操作
	string s = "zhangsan@gmail.com";
	int pos = s.find("@");
	string username = s.substr(0, pos);
	cout << "用户名为：" << username << endl;
}

int main() {
	test();
	test1();
	return 0;
}
```

运行结果为：

Hello用户名为：zhangsan

### 3.2 vector容器

#### 3.2.1 vector容器基本概念

**功能**：vector数据结构和数组非常类似，又称为**单端数组**

**vector与普通数组的区别：**普通数组是静态的，而vector是可以动态扩展的

**动态扩展：**

- 并不是在原数据空间进行操作，而是重新开辟空间，将原数据进行拷贝，释放原来的空间

- vector容器的迭代器是支持随机访问的迭代器

#### 3.2.2 vector容器的构造函数

**功能描述**：创建vector容器

函数原型：

`vector<T> v;`                                                  //采用模板类 实现，默认构造函数

`vector(v.begin(), v.end());`                   //将v区间（v.begin(),v.end()）拷贝

`vector(n,elme);`                                            //构造函数将n个elme拷贝给本身

`vector(const vector& v);`                         //拷贝构造将v拷贝给本身

示例：

```cpp
void myPrint(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it <<' ';
	}
	cout << endl;
}

void test() {
	vector<int> v;//默认构造 无参构造
	for (int i = 0; i <= 9; i++) {
		v.push_back(i);
	}
	myPrint(v);

	//通过区间构造
	vector<int> v2(v.begin(), v.end());
	myPrint(v2);

	//将n个elme赋值构造
	vector<int> v3(10, 100);
	myPrint(v3);

	//拷贝构造函数
	vector<int> v4(v3);
	myPrint(v4);
}

int main() {
	test();
	return 0;
}
```

运行结果：

0 1 2 3 4 5 6 7 8 90 1 2 3 4 5 6 7 8 9100 100 100 100 100 100 100 100 100 100100 100 100 100 100 100 100 100 100 100

#### 3.2.3 vector容器赋值操作

功能描述：给vector容器进行赋值操作

函数原型：

`vector& operator=(const vector& vec); `   //将vec赋值给当前vector函数

`assign(begin, end)`                                          // 将[beg,  end) 区间的vector数据进行复制到当前vector容器

`assign(n, elme)`                                                 // 将n个elme赋值给当前vector容器

示例：

```cpp
void myPrint(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it <<' ';
	}
	cout << endl;
}

void test() {
	vector<int> v;//默认构造 无参构造
	for (int i = 0; i <= 9; i++) {
		v.push_back(i);
	}
	myPrint(v);

	vector<int> v2;//通过另一个vector容器进行赋值
	v2 = v;
	myPrint(v2);

	vector<int> v3;//通过assign区间来赋值
	v3.assign(v.begin(), v.end());
	myPrint(v3);

	vector<int> v4;//通过n个elme来赋值
	v4.assign(10, 100);
	myPrint(v4);
}

int main() {
	test();
	return 0;
}
```

运行结果：

0 1 2 3 4 5 6 7 8 90 1 2 3 4 5 6 7 8 90 1 2 3 4 5 6 7 8 9100 100 100 100 100 100 100 100 100 100

#### 3.2.4 vector容量和大小

功能描述：对vector容器的容量和大小进行操作

函数原型：

`empty();`                                   //判断容器是否为空

`capacity();`                             //求得容器容量

`size(int num);`                       //求得容器大小

`resize(int num,elme);`        //重新指定容器大小，elme默认值为0

示例：

```cpp
void myPrint(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it <<' ';
	}
	cout << endl;
}

void test() {
	vector<int> v;//默认构造 无参构造
	for (int i = 0; i <= 9; i++) {
		v.push_back(i);
	}
	myPrint(v);

	if (v.empty()) {
		cout << "vector容器为空" << endl;
	}
	else {
		cout << "vector容器不为空" << endl;
		cout << "容量为" << v.capacity() << endl;
		cout << "大小为" << v.size() << endl;
	}

	v.resize(15, 100);
	myPrint(v);
	v.resize(5);
	myPrint(v);
}

int main() {
	test();
	return 0;
}
```

运行结果为：

0 1 2 3 4 5 6 7 8 9vector容器不为空容量为13大小为100 1 2 3 4 5 6 7 8 9 100 100 100 100 1000 1 2 3 4

#### 3.2.5 vector插入和删除

功能描述：对vector容器进行插入和删除操作

函数原型：

- `push_back(ele);`                                                                    //在容器末尾插入elme

- `pop_back();`                                                                            //删除容器末尾的数

- `insert(const_iterator pos, ele);`                                 //迭代器指向pos位置插入一个元素ele

- `insert(const_iterator pos, int count, ele);`          //迭代器指向pos位置插入count个元素ele

- `erase(const_iterator pos);`                                             //删除迭代器指向的位置的元素

- `erase(const_iterator beg, const_iterator end);`   //删除区间beg到end之间的元素

- `clear();`                                                                                   //清空容器中所有元素

```cpp
void myPrint(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it <<' ';
	}
	cout << endl;
}

void test() {
	vector<int> v;
	v.push_back(10);
	v.push_back(20);
	v.push_back(30);//尾插
	myPrint(v);

	v.pop_back();//尾删
	myPrint(v);

	v.insert(v.begin(), 10);//在指定位置插入val
	myPrint(v);

	v.insert(v.begin(), 2, 100);//在指定位置插入count个val
	myPrint(v);

	v.erase(v.begin(), v.end()-1);//清除区间_first到_end上的值
	myPrint(v);

	v.clear();//清空容器中的值
}

int main() {
	test();
	return 0;
}
```

运行结果：

10 20 3010 2010 10 20100 100 10 10 2020

#### 3.2.6 vector数据存取

功能描述：对vector容器数据进行存取操作

函数原型：

`at(int index);`            //访问位置下标为index的元素

`vector& operator[]`    //通过[]的方式来访问

`front();`                         //访问容器中第一个元素

`back();`                           //访问容器中最后一个元素

示例：

```cpp
void test() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	for (int i = 0; i < v.size(); i++) {//通过[]来访问
		cout << v[i] <<" ";
	}
	cout << endl;

	for (int i = 0; i < v.size(); i++) {//通过at来访问
		cout << v.at(i) << " ";
	}
	cout << endl;

	cout << v.front() << endl;//访问第一个元素

	cout << v.back() << endl;//访问最后一个元素
}

int main() {
	test();
	return 0;
}
```

#### 3.2.7 vector互换容器

功能描述：实现两个容器内元素互换，可以实现内存收缩操作

函数原型：swap(vec);                        //将vec与本身的元素进行互换

示例：

```cpp
void printVector(vector<int>& v) {
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	vector<int> v2;
	for (int i = 9; i >= 0; i--) {
		v2.push_back(i);
	}
	cout << "交换前\nv :";
	printVector(v);
	cout << "v2:";
	printVector(v2);

	v.swap(v2);
	cout << "交换后\nv :";
	printVector(v);
	cout << "v2:";
	printVector(v2);
}

void test1() {//swap的实用案例
	vector<int> v;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
	}
	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	v.resize(3);//重新指定大小为3
	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;

	vector<int>(v).swap(v);//巧用swap收缩内存，vector<int>(v)利用拷贝构造函数构造了一个新的容器
	cout << "v的容量为：" << v.capacity() << endl;
	cout << "v的大小为：" << v.size() << endl;
}
int main() {
	test();
	test1();
	return 0;
}
```

#### 3.2.8 vector预留空间

功能描述：减少vector在动态扩展内存进行的次数

函数原型：reserve(int len);    //容器预留len个元素长度，预留位置不初始化，元素不可访问

示例：

```cpp
void test() {
	vector<int> v;
	int num = 0;//用来记录动态拓展的次数
	int* p = NULL;
	for (int i = 0; i < 100000; i++) {
		v.push_back(i);
		if (p != &v[0]) {
			p = &v[0];
			num++;
		}
	}
	cout << "v动态拓展的次数是：" << num << endl;
	vector<int>v1;
	v1.reserve(100000);
	num = 0;
	for (int i = 0; i < 100000; i++) {
		v1.push_back(i);
		if (p != &v1[0]) {
			p = &v1[0];
			num++;
		}
	}
	cout << "v1动态拓展次数为：" << num << endl;
}
int main() {
	test();
	return 0;
}
```

### 3.3 deque容器

#### 3.3.1 deque容器基本概念

**功能：**双端数组，可以对头部和尾部进行增删操作

**deque和vector的区别：**

- vector对头部的数据插入效率极低，数据量越大，效率越低

- deque相对而言对头部的插入效率更高

- vector访问元素的速度会更快，这与两种容器的内部结构有关

**deque内部工作原理：**

- deque内部有个中控器，维护每段缓冲区中的内容，缓冲区中存放真实的数据

- 中控器维护的是每一段缓冲区的地址，使得使用deque时像一块连续的内存空间

deque容器的迭代器也是支持随机访问的

#### 3.2.2 deque构造函数

**功能描述：**

deque容器构造

**函数原型：**

deque  deqT;                  //默认构造形式

deque(beg,end);                   //构造函数将[beg, end) 区间的元素拷贝给本身

deque(n, elme)l;                   //构造函数将n个elme拷贝给本身

deque(const deque &dq);   //拷贝构造函数

示例：

```cpp
void printDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	deque<int>d;//默认构造形式
	for (int i = 0; i < 10; i++) {
		d.push_back(i);
	}
	printDeque(d);
	deque<int>d1(d.begin(), d.end());//构造函数将[beg, end) 区间的元素拷贝给本身
	printDeque(d1);
	deque<int>d2(10, 100);//构造函数将n个elme拷贝给本身
	printDeque(d2);
	deque<int>d3(d2);//拷贝构造函数
	printDeque(d3);
}
int main() {
	test();
	return 0;
}
```

#### 3.3.3 deque赋值操作

**功能描述：**给deque容器进行赋值

**函数原型：**

`deque& operator=(const deque  &deq);`              //重载等号操作符

`assign(beg, end);`                                                      //将[beg,end)区间中的数据拷贝给本身

`assign(n, elme);`                                                        //将n个饿了么拷贝赋值给本身

示例：

```cpp
void printDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	deque<int>d;//默认构造形式
	for (int i = 0; i < 10; i++) {
		d.push_back(i);
	}
	deque<int>d1;
	d1 = d; //重载等号操作符
	printDeque(d1);
	deque<int>d2;
	d2.assign(d1.begin(), d1.end());//将[beg,end)区间中的数据拷贝给本身
	printDeque(d2);
	deque<int>d3;
	d3.assign(10, 100);//将n个饿了么拷贝赋值给本身
	printDeque(d3);
}
int main() {
	test();
	return 0;
}
```

#### 3.3.4 deque大小操作

功能描述：对deque容器的大小进行操作

函数原型：

`deque. empty();`                             //用来判断是否为空

`deque. size();`                               //用来计算容器的大小

`deque. resize();`                           //用来重新指定容器大小,使用默认值进行填充操作

`deque. resize(n, ele);`              //用来重新指定容器大小，使用 ele 来进行填充操作

deque容器没有容量的概念。

示例：

```cpp
void printDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	deque<int>d;//默认构造形式
	for (int i = 0; i < 10; i++) {
		d.push_back(i);
	}
	printDeque(d);
	if (d.empty()) {
		cout << "容器是为空的" << endl;
	}
	else {
		cout << "容器不为空，且长度为" << d.size() << endl;
	}
	d.resize(12);//使用默认值
	printDeque(d);
	d.resize(15, 1);//使用1来填充
	printDeque(d);
}
int main() {
	test();
	return 0;
}
```

运行结果：

0 1 2 3 4 5 6 7 8 9容器不为空，且长度为100 1 2 3 4 5 6 7 8 9 0 00 1 2 3 4 5 6 7 8 9 0 0 1 1 1

#### 3.3.5 deque容器的插入和删除

**功能描述：**

向deque容器中插入和删除数据

**函数原型：**

两端插入和删除：

`push_back(elme);`                                        //在容器尾部插入数据

`pop_back(elme);`                                          //删除容器尾部的数据

`push_front(elme);`                                      //在容器头部插入数据

`pop_front(elme);`                                        //删除容器头部的数据

指定位置进行操作：

`insert(pos, elem);`                                    //在指定位置插入数据，elme, 返回新元素的位置

`insert(pos, n, elem);`                              //在指定位置插入n个elme元素，无返回值

`insert(pos, beg, end);`                            //在pos位置，插入[beg, end) 区间的数据

`clear();`                                                         //清空容器中的数据

`erase(beg, end);`                                        //删除区间[beg, end) 的数据，返回下一个数据的位置

`erase(pos);`                                                  //删除pos位置的数据，返回下一个数据的位置

示例：

```cpp
void printDeque(const deque<int>& d) {
	for (deque<int>::const_iterator it = d.begin(); it != d.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	//两端进行操作
	deque<int>d;
	d.push_back(10);
	d.push_back(20);
	d.push_front(100);
	d.push_front(200);
	printDeque(d);
	d.pop_back();
	d.pop_front();
	printDeque(d);
	//指定位置操作
	d.insert(d.begin(), 1000);
	d.insert(d.begin(), 10000);
	printDeque(d);
	deque<int>d1;
	d1.push_back(1);
	d1.insert(d1.begin(), d.begin(), d.end());
	printDeque(d1);
	d.erase(d.begin());
	printDeque(d);
	d.erase(d.begin(), d.end());
	printDeque(d);
	d1.clear();
	printDeque(d1);
}
int main() {
	test();
	return 0;
}
```

运行结果：

200 100 10 20100 1010000 1000 100 1010000 1000 100 10 11000 100 10

#### 3.3.6 deque数据存取

**功能描述：**

对deque中的数据存取操作

**函数原型：**

`at(int index);`             //通过at的方式来读取数据

`operator[];`                   //通过[]的方式来读取数据

`back();`                           //返回最后一个数据

`front();`                         //返回第一个数据

示例：

```cpp
void test() {
	deque<int>d;
	d.push_back(10);
	d.push_back(20);
	d.push_back(30);
	d.push_front(100);
	d.push_front(200);
	d.push_front(300);
	for (int i = 0; i < d.size(); i++) {
		cout << d[i] << " ";
	}
	cout << endl;
	for (int i = 0; i < d.size(); i++) {
		cout << d.at(i) << " ";
	}
	cout << endl;
	cout << d.back() << endl;
	cout << d.front() << endl;
}
int main() {
	test();
	return 0;
}
```

运行结果：

300 200 100 10 20 30300 200 100 10 20 3030300

#### 3.3.7 deque容器排序操作

**功能描述：**

利用算法对deque容器进行排序操作

**算法：**

`sort(iterator beg, iterator end);`                                                //对beg和end区间的数据进行排序

示例：

```cpp
void test() {
	deque<int>d;
	d.push_back(10);
	d.push_back(20);
	d.push_back(30);
	d.push_front(100);
	d.push_front(200);
	d.push_front(300);
	printDeque(d);
	sort(d.begin(), d.end());
	cout << "排序后：" << endl;
	printDeque(d);
}
int main() {
	test();
	return 0;
}
```

运行结果：

300 200 100 10 20 30

排序后：

10 20 30 100 200 300

### 3.4 案例评委打分

#### 3.4.1 案例描述

有五名选手，ABCD，有十名评委对他们进行打分，去掉最高分和最低分后算出平均分

#### 3.4.2 实现步骤

1. 创建一个vector容器存放五名选手

2. 遍历vector容器，对每个选手进行打分，并且将分数存放到deque容器中

3. 使用sort对deque容器进行排序，去掉最高分和最低分

4. deque容器进行遍历，计算总分

5. 计算平均分

代码：

```cpp
#include<deque>
#include<algorithm>
#include<string>
#include<vector>
#include<ctime>

class person {
public:
	person(string name, int score) {
		m_name = name;
		m_score = score;
	}
	string m_name;
	int m_score;
};

void creatPerson(vector<person>& v) {
	string name = "ABCDE";
	for (int i = 0; i < 5; i++) {
		string name1 = "选手";
		name1 += name[i];
		int score = 0;
		person p(name1, score);
		v.push_back(p);
	}
}

void getScore(vector<person>& v) {
	for (vector<person>::iterator it = v.begin(); it != v.end(); it++) {
		deque<int>d;
		for (int i = 0; i < 10; i++) {
			int score = rand() % 41 + 60;
			d.push_back(score);
		}
		sort(d.begin(), d.end());
		d.pop_back();
		d.pop_front();
		int sum = 0;
		for (deque<int>::iterator dit = d.begin(); dit != d.end(); dit++) {
			sum += (*dit);
		}
		(*it).m_score = sum / d.size();
	}
}

void printScore(vector<person>& v) {
	for (vector<person>::iterator it = v.begin(); it != v.end(); it++) {
		cout << "姓名：" << it->m_name << "\t分数：" << it->m_score << endl;
	}
}

int main() {
	srand((unsigned int)time(NULL));
	vector<person>v;
	//1.创建五名选手
	creatPerson(v);
	//2.对五名选手进行打分和计算平均分
	getScore(v);
	//打印五名选手的平均分
	printScore(v);
	return 0;
}
```

### 3.5 stack容器

概念：stack是一个先进后出(First In Last Out, FILO)的数据结构，只有一个出口

栈只有顶端的元素才可以访问，所以栈不支持遍历

栈中进入数据称为—–入栈`push`

栈中弹出数据称为—–出栈`pop`

例如：手枪中的弹匣

#### 3.5.1 stack常用接口

**功能描述：**栈容器常用的对外接口

**构造函数：**

- `stack<T> stk;`                                                                                  //默认构造函数

- `stack(const stack& stk)`                                                             //拷贝构造函数

**赋值操作：**

- `stack & operator=(const stack& stk);`                                  //重载等号运算符

**数据存取：**

- `push(elme);`                                                                                      //向栈顶添加元素

- `pop();`                                                                                                 //从栈顶弹出元素

- `top();`                                                                                                 //返回栈顶的元素

**大小操作：**

`empty();`                                                                                                    //判断栈内是否为空

`size();`                                                                                                      //返回栈的大小

示例：

```cpp
#include<stack>

void test() {
	stack<int>s;
	s.push(10);
	s.push(20);
	s.push(30);
	cout << "栈的大小为：" << s.size() << endl;
	while (!s.empty()) {
		cout << "栈顶元素为：" << s.top() << endl;
		s.pop();
	}
	cout << "栈的大小为：" << s.size() << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果为：

栈的大小为：3栈顶元素为：30栈顶元素为：20栈顶元素为：10栈的大小为：0

### 3.6 queue容器

#### 3.6.1 queue容器的基本概念

**概念：**Queue容器是一种先进先出(First In First Out, FIFO)的容器，它有两个出口

- 队列容器允许从一端新增数据，一段移除数据

- 队列中只有对头和队尾才可以被使用，因此队列不能进行遍历

- 队列中进数据称为——入队 `push`

- 队列中移除数据为——出队 `pop`

- 生活中的排队大多为队列

#### 3.6.2 queue常用接口

**功能描述：**栈容器常用的对外接口

**构造函数：**

`queue<T> que;`                                                //queue采用模板类实现，queue对象的默认构造形式

`queue( const queue & que );`                  //拷贝构造函数

**赋值操作：**

`queue& operator=(const queue &que);`  //重载等号运算符

**数据存取：**

`push(elme);`                                                    //往队尾添加元素

`pop();`                                                              //从队头移走元素

`back();`                                                            //返回最后一个元素

`front();`                                                          //返回第一个元素

**大小操作：**

`empty();`                                                         //判断是否为空

`size();`                                                          //返回栈的大小

示例：

```cpp
class person {
public:
	person(string name, int age) {
		m_name = name;
		m_age = age;
	}
	string m_name;
	int m_age;
};

void test() {
	queue<person>q;
	person p1("张三", 11);
	person p2("李四", 12);
	person p3("王五", 13);
	person p4("赵六", 16);
	//入队
	q.push(p1);
	q.push(p2);
	q.push(p3);
	q.push(p4);
	
	cout << "容器的大小：" << q.size() << endl;
	while (!q.empty()) {
		cout << "队头---姓名：" << q.front().m_name << "年龄：" << q.front().m_age << endl;
		cout << "队尾---姓名：" << q.back().m_name << "年龄：" << q.back().m_age << endl;
		q.pop();
	}
	cout << "容器的大小：" << q.size() << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

容器的大小：4队头—姓名：张三年龄：11队尾—姓名：赵六年龄：16队头—姓名：李四年龄：12队尾—姓名：赵六年龄：16队头—姓名：王五年龄：13队尾—姓名：赵六年龄：16队头—姓名：赵六年龄：16队尾—姓名：赵六年龄：16容器的大小：0

### 3.7 list容器

#### 3.7.1 list基本概念

**功能：**将数据进行链式存储

链表(list)是一种物理存储单元上非连续的存储，数据的顺序是根据链表中的指针链接实现的

**链表的组成：**链表由一系列结点组成

**结点的组成：**一个是数据元素的数据域，另一个是存储下一个结点位置的指针域

STL中的链表是一个双向循环链表

由于链表的存储方式并不是连续的存储空间，因此链表list中的迭代器只支持前移和后移，属于**双向迭代器**

**list的优点：**

采用动态存储分配，不会造成内存浪费或溢出

链表执行插入和删除操作十分方便，修改指针即可，不需要大量移动元素

**list的缺点：**

链表灵活，但是空间（指针域）和时间（遍历）额外耗费很大

list有一个重要的性质，插入操作和删除操作都不会造成原有迭代器失效，这在vector下是不成立的

**总结：**

list和vector作为最常用的容器各有优缺点

#### 3.7.2 list构造函数

**功能：**

- 创建list容器

**函数原型：**

list lst;                                                 //list采用默认模板类实现，对象的默认构造形式

list(beg, end);                                           //将list容器中[beg, end)区间的数据拷贝到当前list容器

lsit(n, elem);                                             //使用n个elme数据来构造；

list(const list&  lis);                                  //拷贝构造函数

示例：

```cpp
#include<list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	printList(L1);
	list<int>L2(L1.begin(), L1.end());
	printList(L2);
	list<int>L3(L2);
	printList(L3);
	list<int>L4(10, 1000);
	printList(L4);
}

int main() {
	test();
	return 0;
}
```

运行结果：

10 20 3010 20 3010 20 301000 1000 1000 1000 1000 1000 1000 1000 1000 1000

总结：list与STL中其他常用容器构造函数差不多，熟练掌握即可

#### 3.7.3 list赋值与交换

**功能描述：**

给list容器进行赋值，以及进行list容器交换

函数原型：

- `assign(beg, end);`                                      //将区间[beg, end) 区间的数据进行拷贝赋值给本身

- `assign(n, elem);`                                        //使用n个elem来给容器赋值

- `list& operato=(const list &lst);`       //重载等号运算符

- `swap(lst);`                                                     //将lst与本身的元素互换

示例：

```cpp
#include<list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	printList(L1);
	list<int>L2;
	L2.assign(L1.begin(), L1.end()); //将区间[beg, end) 区间的数据进行拷贝赋值给本身
	printList(L2);
	L2.assign(10, 1000);//使用n个elem来给容器赋值
	printList(L2);
	list<int>L3;
	L3 = L2;//重载等号运算符
	printList(L3);
	//交换前
	cout << "交换前L1:";
	printList(L1);
	cout << "交换前L3:";
	printList(L3);
	swap(L1, L3);
	//交换后
	cout << "交换后L1:";
	printList(L1);
	cout << "交换后L3:";
	printList(L3);
	
}

int main() {
	test();
	return 0;
}
```

#### 3.7.4 list大小操作

**功能描述：**

- 对list容器的大小操作

**函数原型：**`size();`                               //返回容器中元素的个数

`empty();`                             //判断容器是否就为空

`resize(num);`                     //重新指定容器的大小，填充时用默认值0

`resize(num, elem);`         //重新指定容器的大小，填充时用给定的值

示例：

```cpp
#include<list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	list<int>L1;
	L1.push_back(10);
	L1.push_back(20);
	L1.push_back(30);
	if (L1.empty()) {
		cout << "容器是为空的" << endl;
	}
	else {
		cout << "容器不为空" << endl;
		cout << "容器的大小为：" << L1.size() << endl;
	}
	L1.resize(12);
	printList(L1);
	L1.resize(15);
	printList(L1);
	L1.resize(6);
	printList(L1);
}

int main() {
	test();
	return 0;
}
```

#### 3.7.5 list 插入与删除

**功能描述：**

- 对list容器进行数据的插入和删除

**函数原型：**

`push_back(elem);`                       //在容器尾部插入数据elem

`pop_back();`                                 //删除尾部的数据

`push_front(elem);`                    //在头部插入数据elem

`pop_front();`                              //删除头部的数据

`insert(pos, elem);`                 //在pos（是迭代器）位置插入数据elem，返回新数据的位置

`insert(pos, n, elem);`           //在pos位置插入n个数据elem，无返回值

`insert(pos, beg, end);`        //在pos位置插入区间[beg, end)的值，无返回值

`clear();`                                     //移除容器中所有的数据

`erase(beg, end);`                    //删除[beg, end)区间的所有数据

`erase(pos);`                              //删除，pos位置的数据

`remove(elem);`                          //删除容器中所有与elem相等的元素

示例：

```cpp
#include<list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	list<int>L1;
	L1.push_back(10); //在容器尾部插入数据elem
	L1.push_back(20);
	L1.push_back(30);
	printList(L1);
	L1.push_front(100); //在头部插入数据elem
	L1.push_front(200);
	L1.push_front(300);
	printList(L1);
	L1.pop_back();  //删除尾部的数据
	printList(L1);
	L1.pop_front(); //删除头部的数据
	printList(L1);
	L1.insert(L1.begin(),1, 300); //删除头部的数据//在pos位置插入n个数据elem，无返回值
	list<int>L2;
	L2.push_back(1);
	L2.insert(L2.begin(), L1.begin(), L1.end());//在pos位置插入区间[beg, end)的值，无返回值
	printList(L2);
	L2.clear(); //移除容器中所有的数据
	printList(L2);
	L1.erase(L1.begin());//删除，pos位置的数据
	printList(L1);
	list<int>::iterator it = L1.end();
	it--;
	L1.erase(L1.begin(),it);//删除[beg, end)区间的所有数据
	L1.push_back(1000);
	L1.remove(1000);//删除容器中所有与elem相等的元素
	printList(L1);
}

int main() {
	test();
	return 0;
}
```

运行结果：

10 20 30300 200 100 10 20 30300 200 100 10 20200 100 10 20300 200 100 10 20 1

200 100 10 2020

#### 3.7.6 list数据存取

功能描述：

对list容器中的数据进行存取

函数原型：

front();   //返回第一个元素

back();   //返回最后一个元素

示例：

```cpp
#include<list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	list<int>L1;
	L1.push_back(10); //在容器尾部插入数据elem
	L1.push_back(20);
	L1.push_back(30);
	cout << L1.front() << endl;
	cout << L1.back() << endl;
	list<int>::iterator it = L1.begin();
	it++;//it++和it--操作是支持的
	it--;//而it+1，it-1操作是不支持的，因为list迭代器是不支持随机访问的，可以+1也可以+2所以不支持
}

int main() {
	test();
	return 0;
}
```

运行结果：

1030

#### 3.7.7 list反转和排序

**功能描述：**

- 将容器中的元素反转，以及将容器中的数据进行排序

**函数原型：**

- `reverse();` //反转链表

- `sort(); `       //链表排序

**示例：**

```cpp
#include<list>

void printList(const list<int>& L) {
	for (list<int>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

bool myCompare(int a, int b) {//返回职位布尔类型
	return a > b;//a>b为升序，a<b为降序
}

void test() {
	list<int>L1;
	L1.push_back(10); 
	L1.push_back(30);
	L1.push_back(20);
	//反转链表
	L1.reverse();
	printList(L1);
	//排序链表
	//对于所有迭代器不能随机访问的容器，都不能使用标准算法
	//对于所有迭代器不能随机访问的容器，内部会自己提供一些相应的算法
	L1.sort();//默认排序是升序
	cout << "升序排序后：";
	printList(L1);
	L1.sort(myCompare);//升序自己构造一个bool函数即可
	cout << "降序排序后：";
	printList(L1);
}

int main() {
	test();
	return 0;
}
```

运行结果：

20 30 10升序排序后：10 20 30降序排序后：30 20 10

#### 3.7.8 排序案例

**案例描述：**将person自定义数据类型进行排序，person中属性有姓名，年龄，身高

**排序规则：**按照年龄进行升序，如果年龄相同按照身高降序

**示例：**

```cpp
#include<list>

class person {
public:
	person(string name, int age, int height) {
		m_name = name;
		m_age = age;
		m_height = height;
	}
	string m_name;
	int m_age;
	int m_height;
};

void printList(const list<person>& L) {
	for (list<person>::const_iterator it = L.begin(); it != L.end(); it++) {
		cout << "姓名：" << (*it).m_name << " 年龄：" << (*it).m_age << "身高：" << (*it).m_height << endl;
	}
	cout << endl;
}

bool myCompare(person& a, person& b) {
	if (a.m_age == b.m_age) {
		return a.m_height < b.m_height;
	}
	return a.m_age > b.m_age;
}

void test() {
	list<person>L1;
	person p1("赵云", 30, 180);
	person p2("韩信", 30, 176);
	person p3("关羽", 40, 186);
	person p4("张飞", 36, 166);
	person p5("李元芳", 18, 160);
	//将数据入容器
	L1.push_back(p1);
	L1.push_back(p2);
	L1.push_back(p3);
	L1.push_back(p4);
	L1.push_back(p5);
	L1.sort(myCompare);
	printList(L1);
}

int main() {
	test();
	return 0;
}
```

运行结果：

姓名：关羽 年龄：40身高：186姓名：张飞 年龄：36身高：166姓名：韩信 年龄：30身高：176姓名：赵云 年龄：30身高：180姓名：李元芳 年龄：18身高：160

### 3.8 set/multiset 容器

#### 3.8.1 set基本概念

**简介：**

- 所有元素在插入时都会被自动排序

**本质：**

- set/multiset属于**关联式容器**，底层实现是用的**二叉树**

**set和multiset区别：**

- set不允许容器中有重复的元素出现

#### 3.8.2 set构造和赋值

**功能描述：**创建set容器以及赋值

**构造：**

- `set<T> st;`                                  //默认构造函数

- `set(const set &st);`               //拷贝构造函数

**赋值：**

- `set& operator=(const set &st);`   //重载等号运算符

**示例：**

```cpp
#include<set>

void printSet(const set<int> &s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	set<int> s1;//默认构造
	//set插入值时只有insert
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	s1.insert(30);
	s1.insert(0);
	printSet(s1);
	set<int> s2(s1);//拷贝构造
	printSet(s2);
	set<int> s3 = s2;//重载等号运算符
	printSet(s3);
}

int main() {
	test();
	return 0;
}
```

运行结果：

0 10 20 300 10 20 300 10 20 30

#### 3.8.3 set大小和交换

**功能描述：**

- 统计set容器大小以及交换set容器

**函数原型：**

`size();`             //返回容器中元素的个数     

`empty();`           //判断容器是否为空

`swap(st);`         //交换两个集合容器

**示例：**

```cpp
#include<set>

void printSet(const set<int> s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	set<int> s1;//默认构造
	//set插入值时只有insert
	s1.insert(10);
	s1.insert(30);
	s1.insert(20);
	printSet(s1);
	if (s1.empty()) {
		cout << "s1容器为空" << endl;
	}
	else {
		cout << "s1容器为空" << endl;
		cout << "s1容器的大小为：" << s1.size() << endl;
	}
	//交换
	set<int>s2;
	s2.insert(100);
	s2.insert(200);
	s2.insert(300);
	cout << "交换前s1:";
	printSet(s1);
	cout << "交换前s2:";
	printSet(s2);
	s1.swap(s2);
	cout << "交换后s1:";
	printSet(s1);
	cout << "交换后s2:";
	printSet(s2);
}

int main() {
	test();
	return 0;
}
```

运行结果：

10 20 30s1容器为空s1容器的大小为：3交换前s1:10 20 30交换前s2:100 200 300交换后s1:100 200 300交换后s2:10 20 30

#### 3.8.4 set插入和删除

**功能描述：**

- set容器进行插入数据和删除数据

**函数原型：**

- `insert(elem);`                //在容器插入数据

- `clear();`                          //清空容器

- `erase(pos);`                    //删除pos迭代器指向的元素，返回下一个元素的迭代器

- `erase(beg, end);`         //删除区间[beg, end)的所有元素，返回下一个元素的迭代器

- `erase(elem);`                 //删除容器中值为elem的元素

示例：

```cpp
#include<set>

void printSet(const set<int> s) {
	for (set<int>::const_iterator it = s.begin(); it != s.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

void test() {
	set<int> s1;//默认构造
	//set插入值时只有insert
	s1.insert(30);
	s1.insert(10);
	s1.insert(20);
	printSet(s1);
	s1.erase(s1.begin());
	printSet(s1);
	//删除的重载版本
	s1.erase(30);
	printSet(s1);
	s1.clear();//清空
	printSet(s1);
}

int main() {
	test();
	return 0;
}
```

运行结果：

10 20 3020 3020此行为空行

#### 3.8.5 set查找和统计

**功能描述：**

- 对set容器进行查找数据和统计数据

**函数原型：**

- `find(key);`                                //查找key是否存在，若存在返回该元素的迭代器，若不存在返回s.end()；

- `count(key);`                              //统计key的元素个数

示例：

```cpp
#include<set>

void test() {
	set<int> s1;
	s1.insert(30);
	s1.insert(10);
	s1.insert(20);
	s1.insert(30);
	s1.insert(30);
	set<int>::iterator it = s1.find(30);
	if (it != s1.end()) {
		cout << "找到了元素" << *it << endl;
		cout << "该元素的个数为：" << s1.count(30) << "个" << endl;
	}
	else {
		cout << "未找到" << endl;
	}

}

int main() {
	test();
	return 0;
}
```

运行结果：

找到了元素30该元素的个数为：1个

#### 3.8.6 set和multiset区别

**学习目标：**

- 掌握set和multiset的区别

**区别：**

- set不可以插入重复的数据，而multiset可以

- set插入数据的同时会返回插入结果，表示插入是否成功

- multiset不会检测数据，因此可以插入重复的数据

示例：

```cpp
#include<set>

void test() {
	set<int> s1;
	pair<set<int>::iterator,bool> ret=s1.insert(10);//底层是pair<iterator,bool>
	if (ret.second) {
		cout << "第一次插入成功" << endl;
	}
	else {
		cout << "第一次插入失败" << endl;
	}
	ret = s1.insert(10);
	if (ret.second) {
		cout << "第二次插入成功" << endl;
	}
	else {
		cout << "第二次插入失败" << endl;
	}
	multiset<int>s2;//multiset可以插入重复的数据
	s2.insert(10);
	s2.insert(10);
	for (multiset<int>::iterator it = s2.begin(); it != s2.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

第一次插入成功第二次插入失败10 10 

#### 3.8.7 pair对组创建

**功能描述：**

- 成对出现的数据，利用队组可以返回两个数据

**两种创建方式：**

- pair<type, type> p (value1, value2);

- pair<type, type> p = make_pair(value1, value2);

示例：

```cpp
#include<iostream>
using namespace std;
#include<string>

void test() {
	pair<string, int> p("张三", 18);
	cout << "姓名：" << p.first << "\t年龄：" << p.second << endl;
	pair<string, int> p1 = make_pair("tom", 18);
	cout << "姓名：" << p1.first << "\t年龄：" << p1.second << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

姓名：张三      年龄：18姓名：tom       年龄：18

#### 3.8.8 set容器排序

**学习目标：**

- set容器的默认排序规则是从小到大，掌握如何改变排序规则

**主要技术点：**

- 利用仿函数，可以改变排序规则

示例一    set存放内置数据类型

```cpp
#include<iostream>
using namespace std;
#include<set>

class mycompare {
public:
	bool operator()(int v1, int v2)const {
		return v1 > v2;
	}
};

void test() {
	set<int, mycompare>s1;
	s1.insert(10);
	s1.insert(30);
	s1.insert(50);
	s1.insert(20);
	for (set<int, mycompare>::iterator it = s1.begin(); it != s1.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

50 30 20 10

示例二 set存放自定义数据类型

```cpp
#include<set>

class person {
public:
	person(string name, int age) {
		m_name = name;
		m_age = age;
	}
	string m_name;
	int m_age;
};

class mYcompare {
public:
	bool operator()(const person v1,const person v2)const {
		return v1.m_age < v2.m_age;
	}
};

void test() {
	person p1("刘备", 30);
	person p2("关羽", 28);
	person p3("张飞", 26);
	person p4("赵云", 23);
	set<person, mYcompare>s1;
	s1.insert(p1);
	s1.insert(p2);
	s1.insert(p3);
	s1.insert(p4);
	for (set<person, mYcompare>::iterator it = s1.begin(); it != s1.end(); it++) {
		cout << "姓名：" << it->m_name << "年龄：" << it->m_age << endl;
	}
}

int main() {
	test();
	return 0;
}
```

运行结果：

姓名：赵云      年龄：23姓名：张飞      年龄：26姓名：关羽      年龄：28姓名：刘备      年龄：30

### 3.9 map/multimap容器

#### 3.9.1 map基本概念

**简介：**

- map中所有元素都是pair

- pair中第一个元素为key（键值），起到 索引作用，第二个元素为value（实值）

- 所有元素都会根据元素的键值自动排序

**本质:**

- map/multimap属于关联式容器，底层由二叉树实现。

**优点：**

- 可以根据key值快速找到value值

**map和multimap区别：**

- map不允许容器中有重复的key值出现

- multimap允许容器中有重复的key值出现

#### 3.9.2 map构造和赋值

**功能描述：**

对map容器进行构造和赋值操作

**函数原型：**

- `map<T1, T2> mp;`                 //map默认构造函数

- `map(const map &mp);`        //拷贝构造函数

**赋值：**

- `map& operator=(const map &mp);`            //重载等号操作符

**示例：**

```cpp
#include<map>

void printMap(const map<int,int>& m) {
	for (map<int, int>::const_iterator it = m.begin(); it != m.end(); it++) {
		cout << "key = " << it->first << "  value = " << it->second << endl;
	}
}

void test() {
	map<int, int>mp;
	mp.insert(pair<int, int>(2, 20));
	mp.insert(pair<int, int>(1, 10));
	mp.insert(pair<int, int>(3, 30));
	printMap(mp);
	map<int, int>mp2(mp);
	cout << "mp:" << endl;
	printMap(mp2);
	map<int, int>mp3;
	cout << "mp2:" << endl;
	mp3 = mp2;
	cout << "mp3:" << endl;
	printMap(mp3);
}

int main() {
	test();
	return 0;
}
```

运行结果：

key = 1  value = 10key = 2  value = 20key = 3  value = 30mp:key = 1  value = 10key = 2  value = 20key = 3  value = 30mp2:mp3:key = 1  value = 10key = 2  value = 20key = 3  value = 30

#### 3.9.3 map大小和交换

**功能描述：**

- 统计map容器大小以及交换map容器

**函数原型：**

- `size();`                              //返回容器中元素的数目

- `empty();`                            //判断容器是否为空

- `swap(st);`                          //交换两个集合容器

**示例：**

```cpp
#include<map>

void printMap(const map<int,int>& m) {
	for (map<int, int>::const_iterator it = m.begin(); it != m.end(); it++) {
		cout << "key = " << it->first << "  value = " << it->second << endl;
	}
	cout << endl;
}

void test() {
	map<int, int>mp;
	mp.insert(pair<int, int>(2, 20));
	mp.insert(pair<int, int>(1, 10));
	mp.insert(pair<int, int>(3, 30));
	if (mp.empty()) {
		cout << "容器是为空的" << endl;
	}
	else {
		cout << "容器不是空的" << endl;
		cout << "容器大小为：" << mp.size() << endl;
	}
	map<int, int>mp2;
	mp2.insert(pair<int, int>(4, 40));
	mp2.insert(pair<int, int>(5, 50));
	mp2.insert(pair<int, int>(6, 60));
	cout << "交换前：" << endl;
	printMap(mp);
	printMap(mp2);
	mp.swap(mp2);
	cout << "交换后：" << endl;
	printMap(mp);
	printMap(mp2);
}

int main() {
	test();
	return 0;
}
```

运行结果：

容器不是空的容器大小为：3交换前：key = 1  value = 10key = 2  value = 20key = 3  value = 30

key = 4  value = 40key = 5  value = 50key = 6  value = 60

交换后：key = 4  value = 40key = 5  value = 50key = 6  value = 60

key = 1  value = 10key = 2  value = 20key = 3  value = 30

#### 3.9.4 map插入和删除

**功能描述:**

- map容器进行插入和删除数据

**函数原型：**

- `insert(elem);`//在容器中插入元素

- `clear();`//清除所有元素

- `erase(pos);`//删除pos迭代器所指的元素，返回下一个元素的迭代器

- `erase(beg, end);`//删除区间[beg, end)的所有元素，返回下一个元素的迭代器

- `erase(key);`//删除容器中值为key的元素

示例：

```cpp
#include<map>

void printMap(const map<int,int>& m) {
	for (map<int, int>::const_iterator it = m.begin(); it != m.end(); it++) {
		cout << "key = " << it->first << "  value = " << it->second << endl;
	}
	cout << endl;
}

void test() {
	map<int, int>mp;
	mp.insert(pair<int, int>(1, 10));//插入方法一
	mp.insert(make_pair(2, 20));//插入方法二
	mp.insert(map<int, int>::value_type(3, 30));//插入方法三
	mp[4] = 40;//插入方法四
	printMap(mp);
	mp.erase(mp.begin());
	printMap(mp);
	mp.erase(4);
	printMap(mp);
}

int main() {
	test();
	return 0;
}
```

运行结果：

key = 1  value = 10key = 2  value = 20key = 3  value = 30key = 4  value = 40

key = 2  value = 20key = 3  value = 30key = 4  value = 40

key = 2  value = 20key = 3  value = 30

#### 3.9.5 map查找和统计

**功能描述：**

- 对map容器进行查找数据以及统计数据

**函数原型：**

- `find(key);`                       //查找key是否存在，如果存在返回该键值的迭代器；若不存在，返回set.end()；

- `count(key);`                    //统计key的元素个数

示例：

```cpp
#include<map>

void test() {
	map<int, int>mp;
	mp.insert(pair<int, int>(2, 20));
	mp.insert(pair<int, int>(1, 10));
	mp.insert(pair<int, int>(3, 30));
	map<int, int>::iterator pos = mp.find(3);
	if (pos != mp.end()) {
		cout << "找到了key值为3的数" << endl;
	}
	else {
		cout << "未找到key值为3的数" << endl;
	}
	int num = mp.count(3);
	cout << "key值为3的个数为：" << num << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

找到了key值为3的数key值为3的个数为：1

#### 3.9.6 map容器排序

**学习目标：**

- map容器默认排序规则为按照key值进行排序，掌握如何改变排序规则

**主要技术点：**

- 利用仿函数，可以改变排序规则

示例：

```cpp
#include<map>
#include<string>

class person {
public:
	person(string name, int age) {
		m_name = name;
		m_age = age;
	}
	string m_name;
	int m_age;
};

class mycoppare {
public:
	bool operator()(const person& a, const person& b) const {//注意加const
		return a.m_age < b.m_age;
	}
};

void test() {
	map<person, person,mycoppare>mp;
	person p1("张三", 11);
	person p2("李四", 13);
	person p3("王五", 12);
	mp.insert(make_pair(p1, p1));
	mp.insert(make_pair(p2, p2));
	mp.insert(make_pair(p3, p3));
	for (map<person, person>::iterator it = mp.begin(); it != mp.end(); it++) {
		cout << "姓名：" << it->first.m_name << "\t年龄：" << it->second.m_age << endl;
	}
}

int main() {
	test();
	return 0;
}
```

运行结果：

姓名：张三      年龄：11姓名：王五      年龄：12姓名：李四      年龄：13

总结：

利用仿函数可以指定map容器的排序规则

对于自定义数据类型，map必须要指定排序规则，同set容器

### 3.10 案例-员工分组

#### 3.10.1 案例描述

- 公司今天招聘了10个员工（ABCDEFGHIJ），10名员工进入公司后，需要指派员工在哪个部门工作

- 员工信息有：姓名，工资组成 ；部门分为：策划，美术，研发

- 随机给10名员工分配部门和工资

- 通过multimap进行信息的插入 key（部门编号），valu（员工）

- 分部门显示员工信息

#### 3.10.2 创建步骤

1. 创建十名员工，放到vector中

2. 遍历vector容器，取出每个员工，进行随机分组

3. 分组后，将员工部门编号作为key，具体员工作为value，放入到multimap容器中

4. 分部门显示员工信息

示例：

```cpp
#include <vector>
#include <map>
#include <string>
#define CEHUA 0
#define MEISHU 1
#define YANFA 2

class worker {
public:
	string m_name;
	int m_salary;
};

void setWorker(vector<worker>& w) {
	string nameNum = "ABCDEFGHIJ";
	for (int i = 0; i < 10; i++) {
		worker wo;
		string name = "员工";
		name += nameNum[i];
		int salary = rand() % 1000000+600000;
		wo.m_name = name;
		wo.m_salary = salary;
		w.push_back(wo);
	}
}

void setGroup(multimap<int, worker>& w_map, vector<worker>& w_vector) {
	for (vector<worker>::iterator it = w_vector.begin(); it != w_vector.end(); it++) {
		int key0 = rand() % 3;
		w_map.insert(make_pair(key0, *it));
	}
}

void showGroup(multimap<int, worker>& m_w) {
	multimap<int,worker>::iterator pos=m_w.find(CEHUA);
	int num=m_w.count(CEHUA);
	int num1 = 0;
	cout << "策划部：" << endl;
	for (; pos != m_w.end()&&num1<=num; pos++) {
		cout << "姓名：" << pos->second.m_name << "工资：" << pos->second.m_salary << endl;
		num1++;
	}
	cout << "---------------------------\n" << "美术部：" << endl;
	pos = m_w.find(MEISHU);
	num = m_w.count(MEISHU);
	num1 = 0;
	for (; pos != m_w.end() && num1 <= num; pos++) {
		cout << "姓名：" << pos->second.m_name << "工资：" << pos->second.m_salary << endl;
		num1++;
	}
	cout << "---------------------------\n" << "研发部：" << endl;
	pos = m_w.find(YANFA);
	num = m_w.count(YANFA);
	num1 = 0;
	for (; pos != m_w.end() && num1 <= num; pos++) {
		cout << "姓名：" << pos->second.m_name << "工资：" << pos->second.m_salary << endl;
		num1++;
	}
}

int main() {
	srand((unsigned)time(NULL));
	vector<worker> M_worker;
	setWorker(M_worker);
	multimap<int, worker>m_wo;
	setGroup(m_wo, M_worker);
	showGroup(m_wo);
	return 0;
}
```

## 4 STL-函数对象

### 4.1 函数对象

#### 4.1.1 函数对象概念

**概念：**

- 重载函数调用操作符的类，其对象常称为函数对象

- 函数对象使用重载()时，行为另外函数调用，也叫仿函数

**本质：**

- 函数对象(仿函数)时一个类，不是一个函数

#### 4.1.2 函数对象使用

**特点：**

- 函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值

- 函数对象超出普通函数的概念，函数对象可以有自己的状态

- 函数对象可以作为参数传递

```cpp
//函数对象在使用时，可以像普通函数那样调用，可以有参数，可以有返回值
class Add {
public:
	int operator()(int v1, int v2){
		this->m_count++;
		return v1 + v2;
	}
	int m_count=0;
};

//函数对象超出普通函数的概念，函数对象可以有自己的状态
class myPrint {
public:
	void operator()(string test) {
		cout << test << endl;
	}
};

void doPrint(myPrint&m,string test) {
	m(test);
}

void test01() {
	Add myadd;
	cout << myadd(3, 3) << endl;//有参数有返回值
	cout << "被调用次数：" << myadd.m_count << endl;//函数对象超出普通函数的概念，函数对象可以有自己的状态
	myPrint mp;
	doPrint(mp, "hello c++");//函数对象可以作为参数传递
}

int main() {
	test01();
	return 0;
}
```

运行结果：

6被调用次数：1hello c++

### 4.2 谓词（pred）

#### 4.2.1 谓词概念

概念：

返回bool类型的仿函数称为谓词

如果operator()接受一个参数，那么叫做一元谓词

如果operator()接受两个参数，那么叫做二元谓词

#### 4.2.2 一元谓词

示例：

```cpp
#include<vector>
#include<algorithm>

class GreaterFive {
public:
	bool operator()(int v1){
		if (v1 > 5) {
			return true;
		}
		else {
			return false;
		}
	}
};
void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	vector<int>::iterator it = find_if(v.begin(), v.end(), GreaterFive());
	if (it != v.end()) {
		cout << "找到了该数为：" << *it << endl;
	}
	else {
		cout << "未找到该数" << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

运行结果：

找到了该数为：6

#### 4.2.3 二元谓词

```cpp
#include<vector>
#include<algorithm>

class mycompare {
public:
	bool operator()(int v1,int v2){
		return v1 > v2;
	}
};
void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	sort(v.begin(), v.end(), mycompare());
	for (vector<int>::iterator it = v.begin(); it != v.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

总结：参数有两个的谓词，称为二元谓词

### 4.3 内建函数对象

#### 4.3.1 内建函数对象意义

**概念：**

- STL内建了一些函数对象

**分类：**

- 算数仿函数

- 关系仿函数

- 逻辑仿函数

**用法：**

- 这些仿函数所产生的对象，用法和一般函数完全相同

- 使用内建函数对象，需要引入头文件`#include<functional>`

#### 4.3.2 算数仿函数

功能描述：

实现四则运算

其中negate时一元运算，其他都是二元运算

仿函数原型：

`template<class T> T plus<T>`                                 //加法仿函数

`template<class T> T minus<T>`                               //减法仿函数

`template T multiplies                       //乘法仿函数

`template<class T> T divides<T>`                           //除法仿函数

`template<class T> T modulus<T>`                           //取模仿函数

`template<class T> T negate<T>`                             //取反仿函数

示例：

```cpp
#include<functional>

void test01() {
	negate<int> n;
	cout << n(50) << endl;
	plus<int> p;
	cout << p(3, 3) << endl;
}

int main() {
	test01();
	return 0;
}
```

运算结果：-506

#### 4.3.3 关系仿函数

**功能描述：**

- 实现关系对比

**仿函数原型：**

`template<class T>  bool equal_to<T>`                                //等于

`template<class T>  bool not_equal_to<T>`                       //不等于

`template<class T>  bool greater<T>`                                  //大于

`template<class T>  bool greater_equal<T>`                     //大于等于

`template<class T>  bool less<T>`                                        //小于

`template<class T>  bool less_equal<T>`                           //小于等于

示例：

```cpp
#include<functional>
#include<vector>
#include<algorithm>

void test01() {
	vector<int> v;
	v.push_back(10);
	v.push_back(50);
	v.push_back(30);
	v.push_back(20);
	v.push_back(40);
	sort(v.begin(), v.end(), greater<int>());//sort底层实现为less
	for(vector<int>::iterator it = v.begin(); it != v.end(); it++){
	cout << *it << " ";
	}
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

**运行结果：**

50 40 30 20 10

**总结：**关系仿函数中最常用的便是greater<>大于

#### 4.3.4 逻辑仿函数

**功能描述：**

- 实现逻辑运算

**函数原型：**

- template bool logical_and       //逻辑与

- template bool logical_or         //逻辑或

- template bool logical_not       //逻辑非

示例：

```cpp
#include<functional>
#include<vector>
#include<algorithm>

void test01() {
	vector<bool> v;
	v.push_back(true);
	v.push_back(true);
	v.push_back(false);
	v.push_back(false);
	v.push_back(true);
	for(vector<bool>::iterator it = v.begin(); it != v.end(); it++){
	cout << *it << " ";
	}
	cout << endl;
	//将v容器中的内容搬运到另一个容器中，并且取反
	vector<bool> v2;
	v2.resize(v.size());//使用transform前要制指定新容器的大小
	transform(v.begin(), v.end(), v2.begin(), logical_not<bool>());
	for (vector<bool>::iterator it = v2.begin(); it != v2.end(); it++) {
		cout << *it << " ";
	}
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

运行结果：

1 1 0 0 1

0 0 1 1 0

## 5 STL-常用的算法

**概述：**

- 算法主要头文件 

- 是所有STL头文件中最大的一个，范围涉及到比较，交换，查找，遍历操作，复制，修改等等

-  体积很小，只包括几个序列上面进行简单数学运算的模板函数

- 定义了一些模板类，用以声明函数对象

### 5.1 常用遍历算法

**学习目标：**

- 掌握常用的遍历算法

**算法简介：**

- for_each//遍历容器

- transform//搬用容器到另一个容器中

#### 5.1.1 for_each

**功能描述：**

- 实现遍历容器

**函数原型：**

- `for_each(iterator beg,iterator end,_func);`

​       //遍历算法，遍历容器元素

​      //beg开始迭代器

​      //end结束迭代器

​     //_func函数或者函数对象

示例：

```cpp
#include<vector>
#include<algorithm>

void print01(int val) {
	cout << val << " ";
}

class print02 {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	for_each(v.begin(), v.end(), print01);
	cout << endl;
	for_each(v.begin(), v.end(), print02());
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

运行结果：

0 1 2 3 4 5 6 7 8 9

0 1 2 3 4 5 6 7 8 9

#### 5.1.2 transform

**功能描述：**

- 搬用容器到另一个容器中

**函数原型：**

- `transform(iterator beg1,iterator end1,iterator beg2,_func);`

​       //beg1源容器开始迭代器

​       //end1源容器结束迭代器

​       //beg2目标容器开始迭代器

​       //_func函数或者函数对象

示例：

```cpp
#include<vector>
#include<algorithm>

class TransForm {
public:
	int operator()(int v) {
		return v;
	}
};

class print02 {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	vector<int> v2;
	v2.resize(v.size());//目标容器要提前指定大小
	transform(v.begin(), v.end(), v2.begin(), TransForm());
	for_each(v2.begin(), v2.end(), print02());
	cout << endl;
}

int main() {
	test01();
	return 0;
}
```

总结：搬用的目标容器必须要提前开辟空间，否则无法正常搬用

### 5.2 常用的查找算法

**算法简介：**

- `find`                                //查找条件

- `find_if`                          //按条件查找

- `adjacent_find `             //查找相邻重复元素

- `binary_search`             //二分查找法

- `count`                              //统计元素个数

- `count_if`                       //按条件统计元素个数

#### 5.2.1 find

**功能描述：**

- 查找指定指定元素，找到返回指定元素的迭代器，找不到返回结束迭代器end()

**函数原型：**

- `find(iterator beg, iterator end,value);`

- //按照查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

- //beg开始迭代器

- //end结束迭代器

- //value查找的元素

示例：

```cpp
#include<vector>
#include<algorithm>

class person {
public:
	person(string name, int age) {
		this->m_name = name;
		this->m_age = age;
	}
	bool operator==(const person& p) {//对于自定义数据类型，要重载等号运算符
		if (this->m_name == p.m_name && this->m_age == p.m_age) {
			return true;
		}
		else {
			return false;
		}
	}
	string m_name;
	int m_age;
};
void test01() {
	vector<int> v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	vector<int>::iterator it = find(v.begin(), v.end(), 6);
	if (it != v.end()) {
		cout << "找到了该元素" << endl;
	}
	else {
		cout << "未找到该元素" << endl;
	}
	person p1("a", 1);
	person p2("b", 2);
	person p3("c", 3);
	vector<person> p;
	p.push_back(p1);
	p.push_back(p2);
	p.push_back(p3);
	person pp("b", 2);
	vector<person>::iterator pit = find(p.begin(), p.end(), pp);
	if (pit != p.end()) {
		cout << "找到了，姓名是：" << pit->m_name << "年龄为：" << pit->m_age << endl;
	}
	else {
		cout << "没找到" << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

运行结果：

找到了该元素

找到了，姓名是：b年龄为：2

#### 5.2.2 find_if

**功能描述：**

- 按条件查找元素

**函数原型：**

- `find_if(iterator beg, iterator end,  _Pred);`

​        //按值查找元素，找到指定位置迭代器，找不到返回结束迭代器位置

​       //beg开始迭代器

​       //end结束迭代器

​       //_Pred函数或者谓词

示例：

```cpp
#include<vector>
#include<algorithm>

class greaterFive {
public:
	bool operator()(int val) {
		if (val > 5) {
			return true;
		}
		else {
			return false;
		}
	}
};

class person {
public:
	person(string name,int age) {
		this->m_name = name;
		this->m_age = age;
	}
	string m_name;
	int m_age;
};

class greater2 {
public:
	bool operator()(const person& p) {
		if (p.m_age > 2) {
			return true;
		}
		else {
			return false;
		}
	}
};

void test01() {
	vector<int> v1;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
	}
	vector<int>::iterator it = find_if(v1.begin(), v1.end(), greaterFive());
	if (it != v1.end()) {
		cout << "找到了，该数为：" << *it << endl;
	}
	else {
		cout << "未找到" << endl;
	}

	person pa("a", 1);
	person pb("b", 2);
	person pc("c", 3);
	person pd("d", 4);
	vector<person> p;
	p.push_back(pa);
	p.push_back(pb);
	p.push_back(pc);
	p.push_back(pd);
	vector<person>::iterator pit = find_if(p.begin(), p.end(), greater2());
	if (pit != p.end()) {
		cout << "找到了，姓名为：" << pit->m_name << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

#### 5.2.3 adjacent_find

**功能描述：**

- 查找相邻重复元素

**函数原型：**

- `adjacent_find (iterator beg, iterator end);`

- `adjacent_find(iterator beg, iterator end);`

​       //查找相邻重复元素，返回相邻元素的第一个位置迭代器

​       //beg开始迭代器

​       //end结束迭代器

示例：

```cpp
#include<vector>
#include<algorithm>

void test01() {
	vector<int> v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(3);
	v.push_back(6);
	v.push_back(6);
	v.push_back(5);
	vector<int>::iterator it = adjacent_find(v.begin(), v.end());
	if (it!=v.end()) {
		cout << "找到了重复元素：" << *it << endl;
	}
	else {
		cout << "没有重复元素" << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

运行结果：

找到了重复元素：6

#### 5.2.4 binary_search

**功能描述：**

- 查找指定元素是否存在

**函数原型：**

- `bool binary_search(iterator beg,iterator end,value);`

​       //查找指定元素，查到返回true，否则返回false

​       //注意：**在无序序列中不可用（因为用的是二分查找法）**

​       //beg开始迭代器

​       //end开始迭代器

​       //value查找的元素

示例：

```cpp
#include<vector>
#include<algorithm>

void test01() {
	vector<int>v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	bool ret = binary_search(v.begin(), v.end(), 6);
	if (ret) {
		cout << "yes" << endl;
	}
	else {
		cout << "no" << endl;
	}
}

int main() {
	test01();
	return 0;
}
```

运行结果：

yes

#### 5.2.5 count

**功能描述：**

- 统计元素个数

**函数原型：**

- `count(iterator beg, iterator end, value );`

​       //统计元素出现次数

​       //beg开始迭代器

​       //end(结束迭代器)

​       //value统计的元素

示例：

```cpp
#include<vector>
#include<algorithm>

class person {
public:
	person(string name, int age) {
		m_name = name;
		m_age = age;
	}
	bool operator==(const person& p) {
		if (this->m_age == p.m_age) {
			return true;
		}
		return false;
	}
	string m_name;
	int m_age;
};

void test01() {
	vector<person>v;
	person p1("刘备", 35);
	person p2("关羽", 35);
	person p3("张飞", 33);
	person p4("赵云", 30);
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	int num = count(v.begin(), v.end(), person("曹操", 35));
	cout << "有" << num << "个人" << endl;
}

void test02() {
	vector<int>v;
	v.push_back(1);
	v.push_back(1);
	v.push_back(1);
	v.push_back(2);
	v.push_back(2);
	int num = count(v.begin(), v.end(), 1);
	cout << "有" << num << "个1" << endl;
}

int main() {
	test01();
	test02();
	return 0;
}
```

运行结果：

有2个人

有3个1

#### 5.2.6 count_if

**功能描述：**

- 按条件统计元素个数

**函数原型：**

- `count_if(iterator beg, iterator end, _pred);`

​       //按条件统计元素出现次数

​       //beg开始迭代器

​       //end结束迭代器

​       //_pred 谓词

示例：

```cpp
#include<vector>
#include<algorithm>

class person {
public:
	person(string name, int age) {
		m_name = name;
		m_age = age;
	}
	string m_name;
	int m_age;
};

class greater33 {
public:
	bool operator()(const person & p) {
		return p.m_age > 33;
	}
};

class greater1 {
public:
	bool operator()(int val) {
		return val > 1;
	}
};

void test01() {
	vector<person>v;
	person p1("刘备", 35);
	person p2("关羽", 35);
	person p3("张飞", 33);
	person p4("赵云", 30);
	v.push_back(p1);
	v.push_back(p2);
	v.push_back(p3);
	v.push_back(p4);
	int num = count_if(v.begin(), v.end(),greater33());
	cout << "大于的33有" << num << "个人" << endl;
}

void test02() {
	vector<int>v;
	v.push_back(1);
	v.push_back(1);
	v.push_back(1);
	v.push_back(2);
	v.push_back(2);
	int num = count_if(v.begin(), v.end(),greater1());
	cout << "大于1的数有" << num << "个" << endl;
}

int main() {
	test01();
	test02();
	return 0;
}
```

运行结果：

大于的33有2个人

大于1的数有2个

### 5.3 常用的排序算法

**算法简介：**

- `sort`                           //对容器内元素进行排序

- `random_shuffle `      //洗牌 指定范围内的元素随机调整次序

- `merge`                         //容器元素合并，并存储到另一容器中

- `reverse`                     //反转指定范围的元素

#### 5.3.1 sort

**功能描述：**

- 对容器内元素进行排序

**函数原型：**

- `sort(iterator beg,iterator end, _Pred);`

​        //按值查找元素，找到返回指定位置迭代器，找不到返回结束迭代器位置

​        //beg 开始迭代器

​		//end 结束迭代器

​		//_Pred 谓词

示例：

```cpp
#include<vector>
#include<algorithm>

void myprint(int val) {
	cout << val << " ";
}

void test() {
	vector<int>v;
	v.push_back(3);
	v.push_back(1);
	v.push_back(2);
	v.push_back(6);
	v.push_back(5);
	//升序
	sort(v.begin(), v.end());
	for_each(v.begin(), v.end(), myprint);
	cout << endl;
	//降序
	sort(v.begin(), v.end(), greater<int>());
	for_each(v.begin(), v.end(), myprint);
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

1 2 3 5 6

6 5 3 2 1

#### 5.3.2 random_shuffle

**功能描述：**

- 洗牌 指定范围内的元素随机调整次序

**函数原型：**

- `random_shuffle(iterator beg, iterator end );`

​		//指定范围内的元素随机调整次序

​		//beg 开始迭代器

​		//end 结束迭代器

示例：

```cpp
#include<vector>
#include<algorithm>
#include<ctime>

void myprint(int val) {
	cout << val << " ";
}

void test() {
	srand((unsigned int)time(NULL));
	vector<int>v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	random_shuffle(v.begin(), v.end());
	for_each(v.begin(), v.end(), myprint);
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

#### 5.3.3 merge

**功能描述：**  

- 两个有序容器合并，并存储到另一个容器中

**函数原型：**

- `merge(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

​		//容器元素合并，并存储到另一个容器中

​		//注意：**两个容器必须是有序的**

示例：

```cpp
#include<vector>
#include<algorithm>

void myprint(int val) {
	cout << val << " ";
}

void test() {
	vector<int>v1;
	vector<int>v2;
	vector<int>vTarget;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i + 10);
	}
	vTarget.resize(v1.size() + v2.size());//一定要开辟好容量
	merge(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	for_each(vTarget.begin(), vTarget.end(), myprint);
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19

#### 5.2.4 reverse

**功能描述：**

- 将容器内元素进行反转

**函数原型：**

- `reserve（iterator beg, iterator end);`

​		//反转指定范围的元素

​		//beg开始迭代器

​		//end结束迭代器

示例：

```cpp
#include<vector>
#include<algorithm>

void myprint(int val) {
	cout << val << " ";
}

void test() {
	vector<int>v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	cout << "反转前：";
	for_each(v.begin(), v.end(), myprint);
	cout << endl;
	reverse(v.begin(), v.end());
	cout << "反转后：";
	for_each(v.begin(), v.end(), myprint);
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

反转前：0 1 2 3 4 5 6 7 8 9

反转后：9 8 7 6 5 4 3 2 1 0

### 5.4 常用的拷贝和替换算法

**算法简介：**

`copy`                       //容器内指定范围内的元素拷贝到另一个容器中

`replace`                //将容器内指定范围的旧元素替换为新元素

`replace_if`          //容器内指定范围满足条件元素替换为新元素

`swap`                      //互换两个容器的元素

#### 5.4.1 copy

**功能描述：**

- 容器内指定范围内的元素拷贝到另外一个容器中

**函数原型：**

- `copy(iterator beg, iterator end, iterator dest );`

示例：

```cpp
#include<vector>
#include<algorithm>

void myprint(int val) {
	cout << val << " ";
}

void test() {
	vector<int>v;
	vector<int>vTarget;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	vTarget.resize(v.size());
	copy(v.begin(), v.end(), vTarget.begin());
	cout << "容器v：";
	for_each(v.begin(), v.end(), myprint);
	cout << endl;
	cout << "容器vTarget：";
	for_each(vTarget.begin(), vTarget.end(), myprint);
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

容器v：0 1 2 3 4 5 6 7 8 9

容器vTarget：0 1 2 3 4 5 6 7 8 9

#### 5.4.2 replace

**功能描述：**

- 将容器内指定范围内的旧元素修改为新元素

**函数原型：**

- `replace(iterator beg, iterator end, oldvalue, newvalue );`

​		//将区间内就元素，替换为新元素

示例：

```cpp
#include<vector>
#include<algorithm>

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};

void test() {
	vector<int>v;
	v.push_back(1);
	v.push_back(2);
	v.push_back(1);
	v.push_back(2);
	v.push_back(1);
	cout << "替换前容器v：";
	for_each(v.begin(), v.end(), myprint());
	cout << endl;
	replace(v.begin(), v.end(), 2, 1);
	cout << "替换后容器v：";
	for_each(v.begin(), v.end(), myprint());
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

替换前容器v：1 2 1 2 1

替换后容器v：1 1 1 1 1

#### 5.4.3 replace_if

**功能描述：**

- 将区间内满足条件的元素，替换成指定元素

**函数原型：**

- `replace_if(iterstor beg, iterator end, _pred, newvalue);`

​		//按条件替换元素为指定元素

示例：

```cpp
#include<vector>
#include<algorithm>

class l5 {
public:
	bool operator()(int val) {
		return val < 5 || val == 9;
	}
};

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test() {
	vector<int>v;
	for (int i = 0; i < 10; i++) {
		v.push_back(i);
	}
	replace_if(v.begin(), v.end(), l5(), 6);
	cout << "替换前容器v：";
	for_each(v.begin(), v.end(), myprint());
	cout << endl;
	replace(v.begin(), v.end(), 2, 1);
	cout << "替换后容器v：";
	for_each(v.begin(), v.end(), myprint());
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

替换前容器v：6 6 6 6 6 5 6 7 8 6

替换后容器v：6 6 6 6 6 5 6 7 8 6

#### 5.4.4 swap

**功能描述：**

- 互换两个容器的元素

**函数原型：**

- `swap(container c1, container c2);`

​		//互换两个容器的元素

示例：

```cpp
#include<vector>
#include<algorithm>

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test() {
	vector<int>v1;
	vector<int>v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(10 + i);
	}
	cout << "交换前容器v1和v2：\n";
	for_each(v1.begin(), v1.end(), myprint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myprint());
	cout << endl;
	swap(v1, v2);
	cout << "交换后容器v1和v2：\n";
	for_each(v1.begin(), v1.end(), myprint());
	cout << endl;
	for_each(v2.begin(), v2.end(), myprint());
	cout << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

交换前容器v1和v2：

0 1 2 3 4 5 6 7 8 9

10 11 12 13 14 15 16 17 18 19

交换后容器v1和v2：

10 11 12 13 14 15 16 17 18 19

0 1 2 3 4 5 6 7 8 9

#### 5.5 常用算数生成算法

**注意 ：**

- 算数生成算法属于小型算法，使用时包含的头文件为`#include<numeric>`

**算法简介：**

`accumulate`       //计算容器元素累加

`fill` 				  //向容器中添加元素

#### 5.5.1 accumulate

**功能描述：**

- 计算区间内容器元素累计总和

**函数原型：**

- `accumulate(iterator beg, iterator end, value);`

​		//计算容器元素累计总和

​		//value为累加初始值

示例：

```cpp
#include<vector>
#include<numeric>

void test() {
	vector<int>v;
	for (int i = 0; i <= 100; i++) {
		v.push_back(i);
	}
	int total = accumulate(v.begin(), v.end(), 0);//
	cout << "总和为：" << total << endl;
}

int main() {
	test();
	return 0;
}
```

运行结果：

总和为：5050

#### 5.5.2 fill

**功能描述：**

- 将容器中填充指定的元素

**函数原型：**

- fill(iterator beg, iterator end, value);

示例：

```cpp
#include<vector>
#include<numeric>
#include<algorithm>

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test() {
	vector<int>v;
	v.resize(10);
	cout << "填充前：" << endl;
	for_each(v.begin(), v.end(), myprint());
	cout << "\n填充后：" << endl;
	fill(v.begin(), v.end(), 1);
	for_each(v.begin(), v.end(), myprint());
}

int main() {
	test();
	return 0;
}
```

运行结果：

填充前：

0 0 0 0 0 0 0 0 0 0

填充后：

1 1 1 1 1 1 1 1 1 1

### 5.6 常用的集合算法

**算法简介：**

`set_intersection `              //求两个容器的交集

`set_union`                            //求两个容器的并集

`set_difference `                 //求两个容器的差集

#### 5.6.1 set_intersection

**功能描述：**

- 求两个**有序**容器的交集

**函数原型：**

- `set_intersection(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

​		//求两个集合的交集

​		//返回最后一个交集元素的下一个位置迭代器

示例：

```cpp
#include<vector>
#include<numeric>
#include<algorithm>

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test() {
	vector<int>v1;
	vector<int>v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i + 5);
	}
	vector<int>vTarget;
	vTarget.resize(min(v1.size(), v2.size()));
	vector<int>::iterator itEnd = set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	for_each(vTarget.begin(), itEnd, myprint());
}

int main() {
	test();
	return 0;
}
```

运行结果：

5 6 7 8 9

#### 5.6.2 set_union

**功能描述：**

- 求两个**有序**集合的并集

**函数原型：**

- `set_union(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

​		//求两个集合的并集

示例：

```cpp
#include<vector>
#include<numeric>
#include<algorithm>

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test() {
	vector<int>v1;
	vector<int>v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i + 5);
	}
	vector<int>vTarget;
	vTarget.resize(v1.size()+ v2.size());
	vector<int>::iterator itEnd = set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	cout << "并集为：" << endl;
	for_each(vTarget.begin(), itEnd, myprint());
}

int main() {
	test();
	return 0;
}
```

运行结果：

并集为：

0 1 2 3 4 5 6 7 8 9 10 11 12 13 14

#### 5.6.3 set_difference

**功能描述：**

- 求两个**有序**集合的差集

**函数原型：**

- `set_difference(iterator beg1, iterator end1, iterator beg2, iterator end2, iterator dest);`

​		//求两个集合的差集

​		//注意：两个集合必须是有序集合

示例：

```cpp
#include<vector>
#include<numeric>
#include<algorithm>

class myprint {
public:
	void operator()(int val) {
		cout << val << " ";
	}
};
void test() {
	vector<int>v1;
	vector<int>v2;
	for (int i = 0; i < 10; i++) {
		v1.push_back(i);
		v2.push_back(i + 5);
	}
	vector<int>vTarget;
	vTarget.resize(max(v1.size(), v2.size()));
	vector<int>::iterator itEnd = set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), vTarget.begin());
	cout << "v1和v2的差集为：" << endl;
	for_each(vTarget.begin(), itEnd, myprint());
	itEnd = set_difference(v2.begin(), v2.end(), v1.begin(), v1.end(), vTarget.begin());
	cout << "\nv2和v1的差集为：" << endl;
	for_each(vTarget.begin(), itEnd, myprint());
}

int main() {
	test();
	return 0;
}
```

运行结果：

v1和v2的差集为：

0 1 2 3 4

v2和v1的差集为：

10 11 12 13 14
