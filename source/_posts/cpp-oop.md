---
title: C++面向对象编程
date: 2022-06-14 03:00:00
updated: 2025-11-13 00:59:47
categories:
  - C++
tags:
  - C++
  - 学习笔记
---

# c++面向对象编程

## 1. 内存分区模型

**C＋＋程序在执行时，将内存大方向划分为4个区域**

- 代码区：存放函数体的二进制代码，由操作系统进行管理的

- 全局区：存放全局变量和静态变量以及常量

- ·栈区：由编译器自动分配释放，存放函数的参数值，局部变量等

- ·堆区：由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收

### 1.1 程序运行前

在程序编译后，生成了exe可执行程序，**未执行该程序**前分为两个区域

**代码区：**

​	存放CPU 执行的机器指令

​	代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码即可，代码区是只读的，使其只读的原因是防止程序意外地修改了它的指令

**全局区：**

​	全局变量和静态变量存放在此.

​	全局区还包含了常量区，字符串常量和其他常量（const修饰的变量）也存放在此.

​	**该区域的数据在程序结束后由操作系统释放.**

### 1.2 程序运行后

**栈区：**

由编译器自动释分配释放，存放函数的参数值，局部变量等；

**注意：**不要返回局部变量的**地址**，栈区开辟的地址由编译器自动释放;

```cpp
int* fuct(){
	int a=10;
    return &a;
}
int main(){
	int* p =fuct();
    cout<<*p<<endl;
    cout<<*p<<endl;
}
```

**堆区**

由程序员分配释放,若程序员不释放,程序结束时由操作系统回收;

在c++中主要利用new来在堆区中开辟内存;

```cpp
int* fuct() {
    int* p = new int(10);//new之后直接返回地址
    return p;//在这里new出来的数值是在堆上的,而指针本身在栈上;
}
int main() {
    int *p = fuct();
    cout << *p << endl;
    cout << *p << endl;//不管打印几遍值都为10;
    return 0;
}
```

### 1.3 new操作符

**new的基本语法**

```cpp
int *p = new int(10);//new之后返回的是一个地址
double *p = new double(10);//什么类型的new就用什么类型来接收
```

```cpp
int* func(){
    int* p = new int(10);
    return p;
}
int main(){
	int *p = func();
    cout << *p << endl;
    cout << *p << endl;
    delete p;
    cout << *p << endl;//当delete释放堆中的内存之后再打印会报错因为访问到了不能访问的地址（引发了异常: 读取访问权限冲突）；
    return 0;
}
```

**在堆区new一个数组**

```cpp
int* func(){
	int* arr = new int[10];//new一个数组返回的是首地址
    for(int i=0;i < 10;i++){
	arr[i] = i;
    }
    delete[] arr;//在释放时加一个[]表明是数组；
}
```

## 2. 引用

### 2.1 引用的基本使用

**作用：**给变量起别名

**语法：**数据类型  &别名 = 全名

```cpp
int a = 10;
int &b = a;//不论是单独改变a还是b，实际上改变的是同一内存的东西，所以输出的结果相同；
```

### 2.2 引用的注意事项

1. 引用必须要初始化

2. 引用在初始化之后不可以更改

### 2.3 引用做函数参数

可以实现形参改实参

```cpp
void swap(int& a, int& b) {
    int temp = a;
    a = b;
    b = temp;
}
int main() {
    int a = 10;
    int b = 20;
    swap(a, b);
    cout << "a= " << a << endl;
    cout << "b= " << b << endl;
    return 0;
}
```

### 2.4 引用做函数的返回值

不能返回局部变量的引用(和讲栈区一样)[^注]

```cpp
int& a() {
    int a = 10;
    return a;
}
int main() {
    int& b = a();
    cout << b << endl;
    cout << b << endl;
    return 0;
}
```

[^注]: 与栈区一样，按理说第二遍输出是不同的但是编译后依然相同，目前我无法解释，等学会了再来；注去dev cpp实验了一下确实是错误的；

可以作为左值使用

```cpp
int& a() {
    static int a = 10;
    return a;
}
int main() {
    int& b = a();
    cout << b << ' ';
    cout << b << ' ';
    a() = 100000;//作为左值使用
    cout << b << ' ';
    cout << b << endl;
    return 0;
}//运行结果未10 10 100000 100000
```

### 2.5 引用的本质

引用的本质是常量指针(常量指针指向的地址是不可以修改的，指向的值是可以修改的)

```cpp
int& a = b; //等价于int const *  a = b;
a = 10;//实际上是*a = 10;
```

### 2.6 常量引用

作用：防止形参改变实参

```cpp
int& a = 10;//错误的
const int& a = 10;//正确的
```

```cpp
right：int func(int& b){
    b = 100;
	cout << b <<endl;	
}
error：int func(const int& b){
    b = 100;//会报错，因为加了const 所以b不可以被修改了；
	cout << b <<endl;	
}
int main(){
	int a=10;
    func( a );
    return 0;
}
```

## 3. 函数提高

### 3.1 函数默认参数

语法：函数类型   函数名 （参数类型 参数名称 = 默认值）

eg:`int func (int a=10);`

当函数有默认值时，调用时可以不传入参数；

**注意事项：**

- 函数有多个参数时，当其中有一个参数有默认值时，其后面的参数都要有默认值

```cpp
right : int func(int a, int b = 10, int c = 10) {}
error : int func(int a, int b = 10, int c ) {}
```

函数声明和函数实现不能同时都有默认值，否则会被认为是重定义

```cpp
int func(int a = 10, int b = 10, int c = 10);
int main(){
	cout << func() << endl;
    return 0;
}
int func(int a , int b , int c){//不可重定义
    return a+b+c;
}
```

### 3.2 函数的占位参数

语法：`函数类型  函数名（占位参数类型）`

```cpp
void func( int ){
	cout<<"This is function!"
}
int main(){
    func(10);//占位参数的地方必须要传入相应的数据类型
	return 0;
}
```

### 3.3 函数重载

#### 3.3.1 概述

函数重载：指的是相同的函数名可以多次使用

条件：

1. 函数名相同

2. 函数作用域相同

3. 函数的参数类型，顺序，个数不同；

`简单来讲其实就是函数的括号内的数要不一样；`

```cpp
int func();
int func(int a,int b);
int func(fouble a,int b);
```

#### 3.3.2 函数重载的注意事项

**引用作为重载条件**

```cpp
int func(int& a){
	cout<<"This is first function!";
}
int func(const int& a){
	cout<<"This is second function!";
}
int main(){
    int b=10;
	func(b);//调用第一个
    func(10);//调用第二个
    retutn 0;
}
```

**当函数重载遇到默认参数时**

```cpp
int func(int a,int b=10){
	cout<<"This is first function!";
}
int func(int a){
	cout<<"This is second function!";
}
int main(){
    int b=10;
    func(b);//此时会报错因为可以调用两个函数，要避免这种情况，就要在写函数的时候多加注意，尽量避免函数重载和默认参数同时出现；
	return 0;
}
```

## 4. 类和对象

c++面向对象的三大特性：**封装，继承，多态**

c++认为万事万物都为对象，对象上有其**属性**和**行为**

eg：

### 4.1 封装

#### 4.1.1 封装的意义

封装的意义：

1. 将属性和行为作为一个整体表现生活中的事物

2. 将属性和行为加以权限控制

**封装意义一：**

在设计类的时候，属性和行为写在一起，表现事物；

**语法：**`class 类名 { 访问权限   属性 / 行为 }；`

[^一些专业术语]: 1.类中的属性和行为我们统一称为成员   2.属性又称为成员属性或成员变量   3.行为又称为成员函数或成员方法

举例：定义一个学生类，要求展示学生的姓名，学号

eg 1:

```cpp
class student {
public:
    string name;
    string num;
    void pri() {
        cout << name << endl;
        cout << num << endl;
    }
};
int main() {
    student st1;
    cin >> st1.name;
    cin >> st1.num;
    st1.pri();
    return 0;
}
```

eg 2: 可以用行为定义属性

```cpp
class student {
public:
    void setName(na){
		name = na;
	}
	void setNum(nu){
		num = nu;
	}
    void pri() {
        cout << name << endl;
        cout << num << endl;
    }
public:
    string name;
    string num;
};
int main() {
    student st1;
    string na , nu;
    cin >> na >> nu;
    setName (na);
    setNum (nu);
    st1.pri ();
    return 0;
}
```

**封装意义二**：

类在设计时可以把属性和行为放在不同的权限下，加以控制

访问权限有三种：

1. public			 类内外都可以访问

2. protected      类内可以访问，类外不可以访问

3. private           类内可以访问，类外不可以访问
注：关于protected和private在继承时会讲到，主要涉及到父与子的关系，protected子可以访问，而private子不能读到

eg: 

```cpp
class person(){
public:
    string name;
protected:
    string id;
private:
    string password;   
public :
    void func(){
		id = "123";
        password = "123";
    }
}:
int main(){
    person p;
    p.name = "张三";
	return 0;
}
```

#### 4.1.2 struct和class的区别

唯一的区别是默认访问权限不同：

- struct默认访问权限是公有；

- class默认访问权限是私有；

#### 4.1.3 成员属性设置为私有

优点：

- 可以自由的控制所有成员属性的读写权限

- 对于写权限，我们可以检测数据的有效性

eg：

```cpp
class person {
public:
    void setname(string name) {
        p_name = name;
    }
    string getname() {
        return p_name;
    }//姓名可读可写
    void setage(string age) {
        p_age = age;
    }//年龄只写
    string lover() {
        p_lover = "美女";
        return p_lover;
    }//爱人只读不可写
private:
    string p_name;
    string p_age;
    string p_lover;
};
int main() {
    person p1;
    string name, age;
    cin >> name;
    p1.setname(name);
    cout << p1.getname() << endl;
    cin >> age;
    p1.setage(age);
    cout << p1.lover() << endl;
    return 0;
}
```

eg：利用成员函数判断两个立方体类是否相等

```cpp
#include <iostream>
using namespace std;
class cube {
public:
    void getlwh(int l, int w, int h) {
        m_l = l;
        m_w = w;
        m_h = h;
    }
    bool issame(cube& c) {
        if (m_l == c.m_l && m_w == c.m_w && m_h == c.m_h) {
            return true;
        }
        else{
            return false;
        }
    }
private:
    int m_l;
    int m_w;
    int m_h;
};
int main() {
    cube a, b;
    a.getlwh(10, 10, 10);
    b.getlwh(10, 10, 10);
    bool ret = a.issame(b);
    if (ret) {
        cout << "相同" << endl;
    }
    else {
        cout << "不同" << endl;
    }
    return 0;
}
```

eg: 给定义一个圆和一个点，判断点和圆的关系；

知识点：

- 在一个类中可以使用别的类作为本类中的成员

- 如何将类拆分为 .h文件和 .cpp文件
`第一种，将所有代码都放在同一个.cpp文件中`

```cpp
class point {
public:
	void setX(int x) {
		m_x = x;
	}
	int getX() {
		return m_x;
	}
	void setY(int y) {
		m_y = y;
	}
	int getY() {
		return m_y;
	}
private:
	int m_x;
	int m_y;
};
class circle {
public:
	void setR(int r) {
		m_r = r;
	}
	int getR() {
		return m_r;
	}
	void setC(point center) {
		m_center = center;
	}
	point getcen() {
		return m_center;
	}
private:
	int m_r;
	point m_center;
}; 
int judge(circle &c,point &p) {
	int l = (c.getcen().getX() - p.getX()) * (c.getcen().getX() - p.getX()) +
		(c.getcen().getY() - p.getY()) * (c.getcen().getY() - p.getY());
	return l;
}
int main() {
	point p,c0;
	p.setX(10);
	p.setY(10);
	c0.setX(10);
	c0.setY(0);
	circle c;
	c.setR(10);
	c.setC(c0);
	int l = judge(c, p);
	if (l > (c.getR() * c.getR())) {
		cout << "点在圆外";
	}
	else if (l == (c.getR() * c.getR())) {
		cout << "点在圆上";
	}
	else cout << "点在圆内";
	return 0;
}
```

`第二种，将类进行拆分`		`主函数`

```cpp
#include <iostream>
using namespace std;
#include"circle.h"
#include"point.h"
int judge(circle &c,point &p) {
	int l = (c.getcen().getX() - p.getX()) * (c.getcen().getX() - p.getX()) +
		(c.getcen().getY() - p.getY()) * (c.getcen().getY() - p.getY());
	return l;
}
int main() {
	point p,c0;
	p.setX(10);
	p.setY(10);
	c0.setX(10);
	c0.setY(0);
	circle c;
	c.setR(10);
	c.setC(c0);
	int l = judge(c, p);
	if (l > (c.getR() * c.getR())) {
		cout << "点在圆外";
	}
	else if (l == (c.getR() * c.getR())) {
		cout << "点在圆上";
	}
	else cout << "点在圆内";
	return 0;
}
```

`point.h文件与point.cpp`

```cpp
#pragma once
#include<iostream>
using namespace std;
class point {
public:
	void setX(int x);
	int getX();
	void setY(int y);
	int getY();
private:
	int m_x;
	int m_y;
};

#include"point.h"//注意要在每一个函数前加上作用域指明其为成员函数
	void point::setX(int x) {
		m_x = x;
	}
	int point::getX() {
		return m_x;
	}
	void point::setY(int y) {
		m_y = y;
	}
	int point::getY() {
		return m_y;
	}
```

`circle.h与circle.cpp文件`

```cpp
#pragma once
#include<iostream>
using namespace std;
#include"point.h"
class circle {
public:
	void setR(int r);
	int getR();
	void setC(point center);
	point getcen();
private:
	int m_r;
	point m_center;
};

#include"circle.h"//注意要在每一个函数前加上作用域指明其为成员函数
void circle::setR(int r) {
	m_r = r;
}
int circle::getR() {
	return m_r;
}
void circle::setC(point center) {
	m_center = center;
}
point circle::getcen() {
	return m_center;
}
```

### 4.2 对象的初始化和清理

c++中的面向对象来源于生活，每个对象都会有初始化设置和对象销毁前清理数据的设置。

#### 4.2.1 构造函数和析构函数

对象的初始化和清理十分重要，c++中使用构造和析构函数解决问题，这两个函数会编译器自动调用，完成对象初始化和清理工作。对象的初始化和清理工作是编译器强制要求我们做的，因此如果我们不提供构造函数和析构函数，编译器会提供空实现的构造函数和析构函数。

析构函数：

`语法：类名() { };`

特点：

1. 没有返回值，不用void

2. 可以有参数，可以发生函数重载

3. 会自动调用，且只会调用一次。

析构函数：

`语法：~类名() { };`

特点：

1. 没有返回值，不写void

2. 没有参数，不可发生函数重载

3. 会自动调用，且只会调用一次。

```cpp
class s{
public:
	s(){
        cout<<"构造函数被调用"<<endl;
    }  
    ~s(){
		cout<<"析构函数被调用"<<endl;
    }
};
int main(){
	s s1;
    return 0;
}
```

#### 4.2.2 构造函数的分类和重定义

两种分类方法：

1. 按参数分类： 有参构造和无参构造

2. 按类型分为： 普通构造和拷贝构造

三种调用方法：

1. 括号法

2. 显示法

3. 隐式转换法

eg：

```cpp
class s {
public:
    s() {//无参/默认
        cout << "无参构造函数被调用" << endl;
    }
    s(int a) {//有参
        age = 10;
        cout << "有参构造函数被调用" << endl;
    }
    s(const s &s1) {//拷贝
        age = s1.age;
        cout << "拷贝构造函数被调用" << endl;
    }
    ~s() {
        cout << "析构函数被调用" << endl;
    }
public:
    int age;
};
int main() {
    //三种调用方法
    s p1;//无参不要加(),否则会被认为是函数声明
    s p2(10);
    s p3(p2);//括号法
    s p_1;
    s p_2 = s(10);
    s p_3 = s(p_2);//显示法
    //对于s(10)，称之为匿名对象；当前执行结束后会立即回收匿名对象，没有s(p1)因为s(p1)==s p1;会发生重定义
    s p_1_ = 10;//相当于s p_1_ = s p_1_(10);
    s p_2_ = p1;//相当于s p_2_ = s p_2_(p1);隐式转换法
    return 0;
}
```

#### 4.2.3 拷贝构造函数调用时机

1. 使用一个已经创建完毕的对象来初始化新创建的对象

2. 值传递的方式给函数参数传值

3. 以值方式返回局部对象

eg：

```cpp
class s {
public:
    s() {//无参/默认
        cout << "无参构造函数被调用" << endl;
    }
    s(const s &s1) {//拷贝
        age = s1.age;
        cout << "拷贝构造函数被调用" << endl;
    }
    ~s() {
        cout << "析构函数被调用" << endl;
    }
    int age;
};
//
void test01() {//使用一个已经创建完毕的对象来初始化新创建的对象
    s s1;
    s1.age = 20;
    s s2(s1);//此时调用拷贝构造函数
}
void dotest(s s1) {//括号内会进行拷贝构造函数

}
void test02() {//值传递的方式给函数参数传值
    s s1;
    dotest(s1);
}
s test03() {
    s s1;
    return s1;//返回值时会进行拷贝构造函数
}
int main() {
    test01();
    test02();
    s s1= test03();
    return 0;
}
```

#### 4.2.4 构造函数调用规则

在默认情况下c++会提供三个构造函数

分别是：

- 默认/无参构造函数，函数体为空

- 有参构造函数，函数体为空

- 拷贝构造函数，对属性进行值拷贝

函数调用规则为：

如果自己构造了有参构造函数，那么编译器将不会再提供默认/无参构造函数

如果自己构造了拷贝构造函数，那么编译器将不会再提供默认/无参构造函数与有参构造函数

#### 4.2.5 深拷贝与浅拷贝

深拷贝与浅拷贝是面试经典问题，也是常见的坑

浅拷贝：简单的赋值运算

深拷贝：在堆区重新申请空间，进行拷贝操作

eg：浅拷贝

```cpp
class s {
public:
    
    ~s() {
       cout << "析构函数被调用" << endl;
        if (height != NULL) 
        delete height;//会报错无法执行，因为s1和s2的height指向同一片地址，不能重复释放，第二次释放会访问到非法区域
        height = NULL;
    }
    int age;
    int* height;
};
void test01() {
    s s1;
    s1.age = 18;
    s1.height = new int(10);
    s s2(s1);
    cout << "s2的年龄是  " << s2.age << endl;
    cout << "s1的身高是  " << *s1.height << endl;
    cout << "s2的身高是  " << *s2.height << endl;
}
int main() {
    test01();
    return 0;
}
```

eg：深拷贝

```cpp
class s {
public:
    s() {

    }
    s(const s &s1) {//浅拷贝
       age = s1.age;
       height = new int(*s1.height);//利用深拷贝，在堆中再开辟一块地址，让两个height记录不同的地址
       cout << "深拷贝构造函数被调用" << endl;
    }
    ~s() {
       cout << "析构函数被调用" << endl;
        if (height != NULL) 
        delete height;
        height = NULL;
    }
    int age;
    int* height;
};
void test01() {
    s s1;
    s1.age = 18;
    s1.height = new int(10);
    s s2(s1);
    cout << "s2的年龄是  " << s2.age << endl;
    cout << "s1的身高是  " << *s1.height << endl;
    cout << "s2的身高是  " << *s2.height << endl;
}
int main() {
    test01();
    return 0;
}
```

#### 4.2.6 初始化列表

作用：对属性进行初始化

`语法：类名（）：属性1（初始值1），属性2（初始值2），属性3（初始值3）{}`

eg：

```cpp
class Person {
public:
    Person(int a,int b,int c): A(a),B(b),C(c) {}
    int A;
    int B;
    int C;
};
void test01() {
    Person p(10,20,30);
    cout << p.A << endl;
    cout << p.B << endl;
    cout << p.C << endl;
}
int main() {
    test01();
    return 0;
}
```

#### 4.2.7 类对象作为类成员

c++中一个类中的成员可以是另一个类，该成员称为成员对象

假设有两个类：a和b，当a作为b中的成员时，a,b的构造函数和析构函数的调用顺序是：

构造函数：a先于b

析构函数：b先于a

eg：

```cpp
class Phone {
public:
    Phone(string Pname): m_Pname(Pname) {
        cout << "Phone的构造函数调用" << endl;
    }
    ~Phone() {
        cout << "Phone的构造函数调用" << endl;
    }
    string m_Pname;
};
class Person {
public:
    Person(string Name,string p_name) :m_Name(Name), m_Pname(p_name) {
        cout << "Person的构造函数调用" << endl;
    }
    
    
    ~Person() {
        cout << "Person的构造函数调用" << endl;
    }
    string m_Name;
    Phone m_Pname;
};
void test01() {
    Person p01("张三", "华为");
    cout << p01.m_Name << "拿着" << p01.m_Pname.m_Pname << "手机" << endl;
}
int main() {
    test01();
    return 0;
}
```

运行结果为：

Phone的构造函数调用		Person的构造函数调用		张三拿着华为手机		Person的析构函数调用		Phone的析构函数调用

#### 4.2.8 静态成员

静态成员是指再成员变量或成员函数前加上static就变成了静态成员

静态成员分为：

1. 静态成员变量：
所有静态成员共享同一份数据
类内声明类外初始化
在编译阶段分配内存

2. 静态成员函数：
所有对象共享同一个静态成员函数
静态成员函数只能访问静态成员变量

eg：静态成员

```cpp
class Phone {
public:
    static int m_a;
private:
    static int m_b;
};
int Phone::m_a = 100;
int Phone::m_b = 1000;//类外初始化
void test01() {
   //通过对象访问
    Phone p1;
    p1.m_a = 10000;
    cout << "p1.m_a = " << p1.m_a << endl;
    Phone p2;
    p1.m_a = 20;
    p2.m_a = 200;//静态变量共享一组数据，所以最后的定义为准
    cout << "p1.m_a = " << p1.m_a << endl;
    cout << "p2.m_a = " << p2.m_a << endl;
    //通过类名访问
    cout << "p1.m_a = " << Phone::m_a << endl;
    cout << "p2.m_a = " << Phone::m_a << endl;
}
int main() {
    test01();
    return 0;
}
```

运行结果：

p1.m_a = 10000		p1.m_a = 200		p2.m_a = 200		p1.m_a = 200		p2.m_a = 200

eg：静态函数

```cpp
#include<iostream>
using namespace std;
#include<string>
class Phone {
public:
    static void pp() {
        cout << "公共区域静态成员函数被调用" << endl;
        cout << "静态成员函数m_a=在静态成员函数被调用" << m_a << endl;
        //cout<<m_b<<endl;会报错因为编译器搞不清除到底是谁的m_b;注意：静态成员函数是所有的对象共有的；
    };
    static int m_a;
    int m_b;
private:
    static void ppp() {
        cout << "私有区域的静态成员函数被调用" << endl;
    };
};
int Phone::m_a = 10;
void test01() {
    //通过对象访问
    Phone p;
    p.pp();
    //通过类名访问
    Phone::pp();
    //Phone::ppp();会报错该静态成员函数是private所以只能在类内被访问
}   
int main() {
    test01();
    return 0;
}
```

运行结果：

公共区域静态成员函数被调用		静态成员函数m_a=在静态成员函数被调用10		公共区域静态成员函数被调用		静态成员函数m_a=在静态成员函数被调用10

### 4.3 c++对象模型和this指针

#### 4.3.1成员变量和成员函数分开存储

在C++ 中类的成员变量和成员函数分开存储

只有非静态成员变量才属于类的对象上

```cpp
class stu1 {

};
class stu2 {
    int a = 10;//只有非静态变量在类内存储其他的都在另外地方存储
    static int b;
    void func() {}
    static void func02(){}
};
int stu2::b = 10;//初始化静态变量
void test01() {
    //测试空类占用的内存空间
    stu1 s101;
    cout << "空类占用内存空间为" << sizeof(s101) << endl;
}
void test02() {
    stu2 s201;
    cout << "stu2类占用内存空间为" << sizeof(s201) << endl;
}
int main() {
    test01();
    test02();
    return 0;
}
```

运行结果为：

空类占用内存空间为1		stu2类占用内存空间为4

#### 4.3.2 this指针概念

通过4.3.1知道成员变量和成员函数是分开存储的

每一个静态成员函数指挥诞生一份函数实例，也就是说多个成员函数会调用同一块代码

那么问题是：如何区分到底是哪一个对象调用函数

c++通过提供特殊的对象指针，this来解决问题，**this指针指被调用的成员函数所属的对象对象**

this指针的用途：

当形参和成员变量重名时，可以用this指针来区分

在类的非静态成员函数中返回对象本身，用return *this，但要注意函数返回值类型要写成`类名&`

```cpp
class person{
public:
    person (int age) {
        //age = age;这样是不对的只会改变形参的值
        //解决方法0：将成员属性age改为m_age;
        //解决方法1：
        this->age = age;
    }
    person& getAddressage(person &p1) {//必须使用person&;如果使用person，则不会引用而是会拷贝一份，本身不会改变
        this->age += p1.age;
        return *this;
    }
    int age;
};
void test01() {
    person p(10);
    cout << "p的年龄为" << p.age << "岁" << endl;
}
void test02() {
    person p1(10);
    person p2(10);
    p2.getAddressage(p1).getAddressage(p1).getAddressage(p1);
    cout << "p2的年龄为" << p2.age << "岁" << endl;
}
int main() {
    test01();
    test02();
    return 0;
}
```

运行结果为：

p的年龄为10岁

p2的年龄为40岁

#### 4.3.3 空指针访问成员函数

c++中空指针也可以调用成员函数，但要注意有没有用到this指针

如果用到this，需要加以判断保证代码健壮性

```cpp
class person{
public:
    void ThisClassName() {
        cout << "This name is Person" << endl;
    }
    void ThisAge() {
        if (this == NULL) {//来保证函数健壮性
            return;
        }
        cout << "this age = " << m_Age << endl;//若只写这一行会报错，因为m_Age默认是this->m_Age
    }
    int m_Age;
};
void test01() {
    person* p = NULL;
    p->ThisClassName();
    p->ThisAge();
}
int main() {
    test01();
    return 0;
}
```

运行结果：

This name is Person

#### 4.3.4 const修饰成员函数

常函数：

- 成员函数后加const我们称为常函数

- 常函数中不可以修改成员属性

- 如果在成员属性前加上mutable之后在常函数依然是可以修改的

常对象：

- 声明对象前加const称之为常函数

- 常对象中的属性是不可以修改的，加上mutable后可以

- 常对象只能调用常函数

eg：

```cpp
class person{
public:
    void ThisClassName() const{
        //m_Age = 10;报错，m_Age不可以修改
        //事实上：此处的m_Age为this->m_Age 而this为person* const this，此时指针的指向是不可以修改的
        //当加上const之后变成了const person* const this，此时指针指向的值都不可以修改了
        m_Name = 01;//可以修改因为加了mutable
    }
    void ThisAge() {
        
    }
    int m_Age;
    mutable int m_Name;
};
void test01() {
    person p;
    p.ThisClassName();
    p.ThisAge();//正常对象两个函数都可以 修改
    const person p01;
    //p01.m_Age = 10;同样不可以修改，会报错
    p01.m_Name = 01;//可以修改
    //p01.thisAge();报错不可以调用
    p01.ThisClassName();//可以调用
}
int main() {
    test01();
    return 0;
}
```

### 4.4 友元

生活中家里有卧室和客厅。我是是一个公共的场所（public），而卧室则比较私密了（private）。一般情况别人不能进，但好朋友是可以的。类也是这样的，通过关键字friend来实现。

友元的目的是让一个函数或者类访问另一个类中的private成员

友元实现有三种：

- 全局函数做友元

- 类做友元

- 成员函数做友元

#### 4.3.1 全局函数做友元

```cpp
class budling{
public:
    friend void goodfriend(budling& budling);
    budling() {
        seetingRoom = "客厅";
        bedRoom = "卧室";
    }
    string seetingRoom;
private:
    string bedRoom;
    
};
void goodfriend(budling& budling) {
    cout << "好朋友正在" << budling.seetingRoom << endl;
    cout << "好朋友又去了" << budling.bedRoom << endl;//本来是无法访问卧室的，有了friend关键字之后就可以了
}
void test01() {
    budling b;
    goodfriend(b);
}
int main() {
    test01();
    return 0;
}
```

运行结果：

好朋友正在客厅

好朋友又去了卧室

#### 4.3.2 类做友元

```cpp
class Budling{
public:
    friend class goodgay;//与全局函数做友元声明类似
    Budling() {
        seetingRoom = "客厅";
        bedRoom = "卧室";
    }
    string seetingRoom;
private:
    string bedRoom;
    
};
class goodgay {
public:
    goodgay() {
        budling = new Budling;
    }
public:
    void visit();
private:
    Budling * budling;
};
void goodgay::visit() {//在类外声明成员函数的方法
        cout << "好朋友正在" << budling->seetingRoom << endl;
        cout << "好朋友又去了" << budling->bedRoom << endl;
}
void test01() {
    goodgay gg;
    gg.visit();
}
int main() {
    test01();
    return 0;
}
```

运行结果：

好朋友正在客厅

好朋友又去了卧室

#### 4.3.3 成员函数做友元

```cpp
class Budling;
class Goodgay {
public:
    Goodgay();
    void visit01();
    void visit02();
private:
    Budling* budling;
};
class Budling {
    friend void Goodgay::visit01();//通过这一行来使visit01可以访问bedRoom;
public:
    Budling();
    string sittingRoom;
private:
    string bedRoom;
};
Budling::Budling() {
    sittingRoom = "客厅";
    bedRoom = "卧室";
}
Goodgay::Goodgay() {
    budling = new Budling;
}
void Goodgay::visit01() {
    cout << "好朋友正在" << budling->sittingRoom << endl;
    cout << "好朋友正在" << budling->bedRoom << endl;
}
void Goodgay::visit02() {
    cout << "好朋友正在" << budling->sittingRoom << endl;
}
void test01() {
    Goodgay gg;
    gg.visit01();
    gg.visit02();
}
int main() {
    test01();
    return 0;
}
```

### 4.5 运算符重载

运算符重载的概念：对已有的运算符进行重新定义以适应不同的的数据类型

#### 4.5.1 加号运算符重载

作用：实现两个自定义数据类型的相加

```cpp
class person {
public:
    //第一种，成员函数重载
    /*person operator+(person& a) {
        person temp;
        temp.m_A = this->m_A + a.m_A;
        temp.m_B = this->m_B + a.m_B;
        return temp;
    }*/
    int m_A;
    int m_B;
};
//第二种全局函数
person operator+(person& p1, person& p2) {
    person temp;
    temp.m_A = p1.m_A + p2.m_A;
    temp.m_B = p1.m_B + p2.m_B;
    return temp;
}
//运算符重载也可以函数重载：也就是operator+可以多次使用，括号内不一样即可；
person operator+(person& a, int num) {
    person temp;
    temp.m_A = a.m_A + num;
    temp.m_B = a.m_B + num;
    return temp;
}
void test01() {
    person p1;
    p1.m_A = 10;
    p1.m_B = 10;
    person p2;
    p2.m_A = 10;
    p2.m_B = 10;
    person p3;
    p3 = p1 + p2;//本质是：p3 = p1.operator+(p2);
    cout << "p3.m_A=" << p3.m_A << endl;
    cout << "p3.m_B=" << p3.m_B << endl;
    person p001;
    p001 = p1 + 50;//本质是：p001 = operator+(p1,50)
    cout << "p001.m_A=" << p001.m_A << endl;
    cout << "p001.m_B=" << p001.m_B << endl;
}
int main() {
    test01();
    return 0;
}
```

运算结果为：

p3.m_A=20

p3.m_B=20

p001.m_A=60

p001.m_B=60

总结：

1. 对于内置的运算符的数据类型是不可能改变的

2. 不要滥用运算符重载

#### 4.5.2 左移运算符重载

作用：可以输出自定义数据类型

```cpp
#include<iostream>
using namespace std;
class person {
    friend ostream& operator<< (ostream& cout, person& p);
public:
    person(int a, int b) {
        this->m_A = a;
        this->m_B = b;
    }//不推荐用成员函数进行左移用算符重载，因为无法是cout在左边，会变成p<<cout;
private:
    int m_A;
    int m_B;
};
ostream& operator<< (ostream& cout, person& p) {//返回ostream才可以链式编程
    cout << "p.m_A=" << p.m_A <<endl<< "p.m_B=" << p.m_B<<endl;
    return cout;
}
void test01() {
    person pp(10, 10);
    cout << pp << "hello world!" << endl;//链式编程
}
int main() {
    test01();
    return 0;
}
```

运行结果：

p.m_A=10

p.m_B=10

hello world!

#### 4.5.3 递增运算符重载

作用：通过重载递增运算符，实现自己的整形数据

```cpp
class MyInteger {
    friend ostream& operator<<(ostream& cout, MyInteger myint);
public:
    MyInteger() {
        m_Num = 0;
    }
    MyInteger& operator++() {//前置递增运算符
        m_Num++;
        return *this;
    }
    MyInteger operator++(int) {//后置递增运算符
        MyInteger temp = *this;//记录当前本身的值，然后让本身加一，返回原来的值；
        m_Num++;
        return temp;
    }
private:
    int m_Num;
};
//将左移运算符定义
ostream& operator<<(ostream &cout,MyInteger myint) {
    cout << myint.m_Num;
    return cout;
}
void test01() {//测试前置递增
    MyInteger myint;
    cout << ++myint << endl;
}
void test02() {//测试后置递增
    MyInteger myint;
    cout << myint++ << endl;
    cout << myint << endl;
}
int main() {
    test01();
    test02();
    return 0;
}
```

总结：前置递增返回引用，后置递增返回值

#### 4.5.4 赋值运算符重载

c++编译器至少会给一个类四个默认函数

1. 默认构造函数（无参，函数体为空）

2. 默认析构函数（无参，函数体为空）

3. 默认拷贝构造函数，对属性进行值拷贝

4. 赋值运算符operator=对属性进行值拷贝

如果类中有属性指向堆区，也会涉及深浅拷贝问题

eg：

```cpp
class Person {
public:
    Person(int age) {
        m_Age = new int(age);
    }
    Person& operator=(Person p) {//返回Person是为了实现连等操作，也就是之前一直提到的链式操作
        if (m_Age != NULL) {
            delete m_Age;
            m_Age = NULL;
        }
        m_Age = new int(*p.m_Age);//在堆区重新申请一块地址
        return *this;
    }
    ~Person() {
        if (m_Age != NULL) {
            delete m_Age;
        }
    }
    int *m_Age;
};
void test01() {
    Person p01(10);
    Person p02(11);
    Person p03(12);
    //利用类自带的operator=
    p03 = p02 = p01;//当未添加析构函数时，不会报错，正常运行，当添加析构函数对堆区进行内存释放会重复释放，从而出错，因为p01,p02指向的是同一块地址
    cout << "p02的年龄为" << *p02.m_Age << endl;
    cout << "p03的年龄为" << *p03.m_Age << endl;
}
int main() {
    test01();
    return 0;
}
```

#### 4.5.5 关系运算符重载

作用：重载两个关系运算符，可以比较两个自定义数据类型

```cpp
class Person {
public:
    Person(string s, int age) {
        m_Name = s;
        m_Age = age;
    }
    //通过成员函数进行运算符重载
    //bool operator==(Person &p) {//==运算符重载
    //    if (m_Name == p.m_Name && m_Age == p.m_Age) {
    //        return true;
    //    }
    //    else return false;
    //}
    //bool operator!=(Person& p) {//!=运算符重载
    //    if (m_Name == p.m_Name && m_Age == p.m_Age) {
    //        return false;
    //    }
    //    else return true;
    //}
    string m_Name;
    int m_Age;
};
//通过全局函数重载运算符
bool operator==(Person& p1, Person& p2) {
    if (p1.m_Name == p2.m_Name && p1.m_Age == p2.m_Age) {
        return true;
    }
    else {
        return false;
    }
}
bool operator!=(Person& p1, Person& p2) {
    if (p1.m_Name == p2.m_Name && p1.m_Age == p2.m_Age) {
        return false;
    }
    else {
        return true;
    }
}
void test01() {
    Person p01("Tom",18);
    Person p02("Tom", 18);
    if (p01 == p02) {
        cout << "通过 == 判断是同一个人" << endl;
    }
    else {
        cout << "通过 == 判断不是同一个人" << endl;
    }
    if (p01 != p02) {
        cout << "通过 != 判断不是同一个人" << endl;
    }
    else {
        cout << "通过 != 判断是同一个人" << endl;
    }
}
int main() {
    test01();
    return 0;
}
```

#### 4.5.6 函数调用运算符重载

- 函数调用运算符（）也可以重载

- 由于重载后的使用方法非常的像函数调用，所以称之为仿函数

- 仿函数没有固定写法，非常灵活

eg：

```cpp
class MyPrint {
public:
    void operator()(string test) {
        cout << test << endl;
    }
};
class MyAdd {
public:
    int operator()(int num1, int num2) {
        return num1 + num2;
    }
};
void test01() {
    MyPrint myprint;
    myprint("hello world!");
}
void test02() {
    MyAdd myadd;
    int temp=myadd(100, 100);
    cout << temp << endl;
    //也可以不创建对象直接利用匿名函数对象调用
    //匿名函数对象为类名加括号
    int temp01=MyAdd()(100, 100);
    cout << temp01 << endl;
}
int main() {
    test01();
    test02();
    return 0;
}
```

### 4.6 继承

**继承是面向对象的三大特性之一**

有些类与类之间存在明显的关系

例如：动物类包含狗类和猫类，而狗类又包含许多细化的狗类，猫类又包含许多细化的猫类

我们发现，定义这些类时，下级别的成员除了拥有上级别的共性，还有自己的特性

这是我们考虑利用继承来减少代码量

#### 4.6.1 继承的基本语法

例如我们看到的许多网页有公共的头，公共的底，公共的侧边栏，只有中间包含的内容不同

普通实现代码量太大，这里不再写了

```cpp
class BasePage {
public:
    void header() {
        cout << "头部" << endl;
    }
    void footer() {
        cout << "底部" << endl;
    }
    void left() {
        cout << "侧边栏" << endl;
    }
};
class CPP :public BasePage {
public:
    void content() {
        cout << "c++内容" << endl;
    }
};
class JAVA :public BasePage {
public:
    void content() {
        cout << "java内容" << endl;
    }
};
class PYTHON :public BasePage {
public:
    void content() {
        cout << "python内容" << endl;
    }
};
void test01() {
    CPP cpp;
    cpp.header();
    cpp.footer();
    cpp.left();
    cpp.content();
    cout << "--------------------------------------------------" << endl;
    JAVA java;
    java.header();
    java.footer();
    java.left();
    java.content();
    cout << "--------------------------------------------------" << endl;
    PYTHON python;
    python.header();
    python.footer();
    python.left();
    python.content();
}
int main() {
    test01();
    return 0;
}
```

**总结：**

继承的好处：可以减少重复的代码

class A : public B

A称为子类或派生类

B称为父类或基类

**派生类中的成员包含两大部分：**

一类是从基类或父类继承过来的，一类是自己增加的成员

从基类继承来的体现了共性，而自己的成员则体现了特性 

#### 4.6.2 继承方式

继承的语法：`class 子类 ：继承方式  父类`

继承方式有三种：

- 公共继承：保持父类中的成员类型不变，但父类的私有属性访问不到

- 保护继承：公共属性变为保护属性，保护属性不变，但父类的私有属性访问不到

- 私有继承：全部变为保护属性，但父类的私有属性访问不到

```cpp
//父类
class base {
public:
    int m_A=10;
protected:
    int m_B=10;
private:
    int m_C=10;
};
//（1）公共继承
class son01 :public base {
public:
    void pri() {
        cout << m_A << endl;
        cout << m_B << endl;
        //无法访问m_C子类继承不到
    }
};
//（2）保护继承
class son02 :protected base {
public:
    void pri() {
        cout << m_A << endl;
        cout << m_B << endl;
        //无法访问m_C子类继承不到
    }
};
//（3）私有继承
class son03 :private base {
public:
    void pri() {
        cout << m_A << endl;
        cout << m_B << endl;
        //无法访问m_C子类继承不到
    }
};
//（201）保护继承后公共继承
class grandson0201 :public son02 {
public:
    void pri() {
        cout << m_A << endl;
        cout << m_B << endl;//依然可以访问
    }
};
//（301）私有继承后公共继承
class grandson0301 :private son03 {
public:
    void pri() {
        /*cout << m_A << endl;
        cout << m_B << endl;报错，访问不到*/
        cout << "无论是m_A,还是m_B，还是m_C都访问不到了" << endl;
    }
};
void test01() {
    //01公共继承
    son01 s01;
    s01.pri();
    s01.m_A;//可以直接打印m_A，是公共属性
    //02保护继承
    son02 s02;
    s02.pri();
    /* s02.m_A;
    s02.m_B;都是错误的，保护属性访问不到*/
    //03私有继承
    son03 s03;
    s03.pri();
    /* s02.m_A;
    s02.m_B;都是错误的，私有属性访问不到*/
    //0201验证是保护属性
    grandson0201 s0201;
    s0201.pri();
    //0301验证是私有属性
    grandson0301 s0301;
    s0301.pri();
}
int main() {
    test01();
    return 0;
}
```

#### 4.6.3 继承中的对象模型

问题：从父类继承过来的成员那些属于子类

eg：

```cpp
//父类
class base {
public:
    int m_A=10;
protected:
    int m_B=10;
private:
    int m_C=10;
};
class son:public base {
private :
    int m_D;
};
void test01() {
    cout << "sizeof(son)=" << sizeof(son) << endl;//结果是16 说明整个父类都继承下来了
    //但是编译器替我们做了隐藏，所以访问不到，通过开发人员命令提示工具可以查看
}
int main() {
    test01();
    return 0;
}
```

运行结果：

sizeof(son)=16

利用vs的开发人员命令提示工具查看类的相关信息的一般步骤：

首先打开Developer Command Prompt for VS 2022

进入E盘

cd 相关文件目录

dir 查看当前目录文件

输入cl /d1 reportSingleClassLayoutson(son为类名) “cpp文件名称”

![](C:\Users\dre\Desktop\屏幕截图 2022-07-26 113606.png)

#### 4.6.4 继承中构造和析构的顺序

子类继承父类后，当创建子类对象时，也会调用父类构造函数

问题：父类和子类的构造和析构谁先谁后的问题

eg：

```cpp
//父类
class base {
public:
    base() {
        cout << "父类构造函数调用" << endl;
    }
    ~base() {
        cout << "父类析构函数调用" << endl;
    }
};
class son:public base {
public:
    son() {
        cout << "子类构造函数调用" << endl;
    }
    ~son() {
        cout << "子类析构函数调用" << endl;
    }
};
void test01() {
    son s;
}
int main() {
    test01();
    return 0;
}
```

运行结果：

父类构造函数调用

子类构造函数调用

子类析构函数调用

父类析构函数调用

#### 4.6.5 继承同名成员处理方法

问题：当子类和父类出现同名成员属性时，如何通过子类对象进行调用

- 访问子类重名成员时直接调用即可（当子类和父类出现重名函数时编译器会隐藏掉所有的父类重名成员（包括重载的））

- 访问到父类重名成员时，需要加作用域

eg：

```cpp
//父类
class base {
public:
    base() {
        m_A = 100;
    }
    void func() {
        cout << "父类重名成员函数调用" << endl;
    }
    int m_A;
};
class son:public base {
public:
    son() {
        m_A = 200;
    }
    void func() {
        cout << "子类重名成员函数被调用" << endl;
    }
    int m_A;
};
void test01() {
    son s;
    //重名成员变量调用
    cout << "s.m_A = " << s.m_A << endl;
    cout << "s.base::m_A = " << s.base::m_A << endl;
    //重名成员函数调用
    s.func();
    s.base::func();
}
int main() {
    test01();
    return 0;
}
```

运行结果：

s.m_A = 200

s.base::m_A = 100

子类重名成员函数被调用

父类重名成员函数调用

总结：

1. 子类对象可以直接访问到子类中的同名成员

2. 子类对象需要加作用域才能访问到父类中的同名成员

3. 当子类和父类出现重名成员时，子类会隐藏掉父类中的同名成员

#### 4.6.6 继承同名静态成员处理方式

问题：继承中同名的静态成员如何在子类对象上访问

静态成员和非静态成员处理方式一致，只不过非静态有两种访问方法

- 访问子类重名成员时直接调用即可（当子类和父类出现重名函数时编译器会隐藏掉所有的父类重名成员（包括重载的））

- 访问到父类重名成员时，需要加作用域

eg：

```cpp
//父类
class base {
public:
    static void func() {
        cout << "父类重名成员函数调用" << endl;
    }
    static int m_A;
};
int base::m_A=100;
class son:public base {
public:
    static void func() {
        cout << "子类重名成员函数被调用" << endl;
    }
    static int m_A;
};
int son::m_A = 200;
void test01() {
    son s;
    //第一种调用方法，通过对象调用
    cout << "s.m_A = " << s.m_A << endl;
    cout << "s.base::m_A = " << s.base::m_A << endl;
    s.func();
    s.base::func();
    //第二种调用方法，通过类名调用
    cout << "son::m_A = " << son::m_A << endl;
    //第一个::代表通过类名方式访问，第二个::代表base作用域下访问
    cout << "son::base::m_A = " << son::base::m_A << endl;
    son::func();
    son::base::func();
}
int main() {
    test01();
    return 0;
}
```

运行结果：

s.m_A = 200

s.base::m_A = 100

子类重名成员函数被调用

父类重名成员函数调用

son::m_A = 200

son::base::m_A = 100

子类重名成员函数被调用

父类重名成员函数调用

#### 4.6.7 多继承语态

c++允许一个类继承多个类

语法：`class 类名：继承方式 类名，继承方式 类名......`

多继承可能会引发父类中有重名成员的出现，需要加作用域加以区分

**C++实际开发中不建议使用多继承**

eg：

```cpp
//父类1
class base1 {
public:
    base1() {
        m_A = 101;
        m_B = 102;
    }
    int m_A;
    int m_B;
};
//父类2
class base2 {
public:
    base2() {
        m_A = 103;
    }
    int m_A;
};
//子类
class son:public base1,public base2 {
public:
    son() {
        m_D = 105;
    }
    int m_D;
};
void test01() {
    son s;
    cout << "sizeof(s) = " << sizeof(s) << endl;
    //访问父类1中的成员
    cout << "s.base1::m_A = " << s.base1::m_A << endl;//不能直接访问s.m_A,父类成员重名了;
    cout << "s.m_B = " << s.m_B << endl;
    //访问父类2中的成员
    cout << "s.base2::m_A = " << s.base2::m_A << endl;//不能直接访问s.m_A,父类成员重名了;
    //访问自身成员
    cout << "s.m_D = " << s.m_D << endl;
}
int main() {
    test01();
    return 0;
}
```

运行结果：

sizeof(s) = 16

s.base1::m_A = 101

s.m_B = 102

s.base2::m_A = 103

s.m_D = 105

#### 4.6.8 菱形继承

菱形继承概念：

- 两个派生类继承了同一个基类

- 又有一个类同时继承了这两个派生类

- 这种继承被称为菱形继承

典型的菱形继承案例：

- 父类：动物

- 子类：羊，驼

- 孙类：羊驼

菱形继承问题：

羊继承了动物的数据，驼同样继承了动物的数据，而羊驼则将羊继承的动物的数据和驼继承的动物的数据又各继承了一遍，事实上，我们只需要继承一遍就可以了

eg：

```cpp
class animal {
public:
    animal() {
        m_Age = 100;
    }
    int m_Age;
};
//继承前加上virtual关键字后变为了虚继承
//此时公共的父类animal称为虚基数
class sheep : virtual public animal{};
class tuo : virtual public animal {};
class sheeptuo:public sheep,public tuo{};
void test01() {
    //此时sheeptuo便可以直接访问m_Age了，因为虚继承是指针两个子函数指针指向相同成员
    sheeptuo s;
    cout << s.m_Age << endl;
    s.sheep::m_Age = 101;
    cout << s.m_Age << endl;
    s.tuo::m_Age = 102;
    cout << s.m_Age << endl;
    //不管是那种访问方式，都只有一份m_Age数据
}
int main() {
    test01();
    return 0;
}
```

运行结果：

100

101

102

总结：

- 菱形继承的主要问题是子类继承两分一样的数据，从而导致资源浪费

- 利用虚继承可以解决菱形继承问题

### 4.7 多态

#### 4.7.1 多态的基本概念

**多态是c++面向对象的三大特性之一**

多态分为两类：

- 静态多态：函数重载和运算符重载属于静态多态，复用函数名

- 动态多态：派生类和虚函数实现运行时多态

静态多态和静态多态区别：

- 静态多态的函数地址早绑定-编译阶段确定函数地址

- 动态多态的函数地址晚绑定-运行阶段确定函数地址

下面 通过案例讲解多态

```cpp
class animal {
public:
    //加入virtual后变为虚函数
    //在运行时加载地址
    virtual void speak() {
        cout << "动物正在说话" << endl;
    }
};
class Cat:public animal {
    void speak() {
        cout << "小猫正在说话" << endl;
    }
};
class Dog:public animal {
    void speak() {
        cout << "小狗正在说话" << endl;
    }
};
//当animal中的speak不是虚函数时，地址在编译时会确定所以无论传进来时cat还是dog都会打印animal中speak的话
void doSpeak(animal& animals) {//即为animal& animals = cat/dog;
    animals.speak();
}
void test01() {
    Cat cat;
    doSpeak(cat);//在父函数中speak变成虚函数后便会调用子函数的speak
    Dog dog;
    doSpeak(dog);
}
int main() {
    test01();
    return 0;
}
```

运行结果：

小猫正在说话

小狗正在说话

**总结：**

多态满足条件:

- 有继承关系

- 子类重写父类中的虚函数

多态使用条件：

- 父类指针或引用指向子类中的对象

重写：函数返回值类型  函数名  参数列表  完全一致称为重写

关于改例子的解释：当我们在父类中创建虚函数，会产生一个vfptr即虚函数指针，此时这个虚函数指针指向vftable即虚函数表而虚函数表中为&animal::speak;当我们在子类中重写虚函数时，vfptr指向的虚函数表中的内容会被覆盖为&Cat::cat;

#### 4.7.2 多态案例-计算器类

案例描述：

分别利用普通写法和多态写法写出计算器

多态优点：

- 代码结构清晰

- 代码可读性强

- 利于代码的前期和后期维护

eg：

```cpp
//普通写法：
class caculator {
public:
    int jisuan(string str) {
        if (str == "+") {
            return m_num1 + m_num2;
        }
        if (str == "-") {
            return m_num1 - m_num2;
        }
        if (str == "*") {
            return m_num1 * m_num2;
        }
    }
    int m_num1;
    int m_num2;
};

//特殊写法：
class AbstractCaculator {
public:
    virtual int jisuan() {
        return 0;
    }
    int m_num3;
    int m_num4;
};
class AddClass:public AbstractCaculator {//加法
public:
    virtual int jisuan() {
        return m_num3 + m_num4;
    }
};
class SubtractionClass :public AbstractCaculator {//减法
public:
    virtual int jisuan() {
        return m_num3 - m_num4;
    }
};
class MultiplicationClass :public AbstractCaculator {//乘法
public:
    virtual int jisuan() {
        return m_num3 * m_num4;
    }
};
void test01() {
    //普通写法进行计算：
    caculator cc;
    cc.m_num1 = 10;
    cc.m_num2 = 10;
    cout << cc.m_num1 << "+" << cc.m_num2 << "=" <<cc.jisuan("+") << endl;
    cout << cc.m_num1 << "-" << cc.m_num2 << "=" << cc.jisuan("-") << endl;
    cout << cc.m_num1 << "*" << cc.m_num2 << "=" << cc.jisuan("*") << endl;

    //多态写法写的，这次我们不用引用，用指针
    //加法
    AbstractCaculator* cc1 = new AddClass;
    cc1->m_num3 = 100;
    cc1->m_num4 = 100;
    cout << cc1->m_num3 << "+" << cc1->m_num4  << "=" << cc1->jisuan() << endl;
    delete cc1;//注意对堆区new出来的值进行释放

    //减法
    cc1 = new SubtractionClass;
    cc1->m_num3 = 100;
    cc1->m_num4 = 100;
    cout << cc1->m_num3 << "-" << cc1->m_num4 << "=" << cc1->jisuan() << endl;
    delete cc1;//注意对堆区new出来的值进行释放

    //乘法
    cc1 = new MultiplicationClass;
    cc1->m_num3 = 100;
    cc1->m_num4 = 100;
    cout << cc1->m_num3 << "*" << cc1->m_num4 << "=" << cc1->jisuan() << endl;
    delete cc1;//注意对堆区new出来的值进行释放
}
int main() {
    test01();
    return 0;
}
```

运行结果：

10+10=20

10-10=0

1010=100

100+100=200

100-100=0

100*100=10000

`总结：c++中很提倡利用多态设计程序架构，因为多态的优点很多`

#### 4.7.3 纯虚函数和抽象类

在多态中通常父类中的虚函数实现是毫无意义的，主要都是调用子类中重写的内容

因此可以将虚函数改写为纯虚函数

语法：`virtual 返回值类型 函数名 （参数列表） =0；`

当类中有了纯虚函数，这个类变成了抽象类

抽象类特点：

- 抽象类无法实例化对象

- 子类必须重写纯虚函数，否则子类也是抽象类

实例：

```cpp
class base {
public:
    virtual void fun() = 0;
};

class son:public base {
public:
    void fun() {

    }
};
void test01() {
    base* ba = NULL;
    //ba = new base; //报错base是抽象类无法实例化对象
    ba = new son;//可以实例化对象
}

int main() { 
    test01();
    return 0;
}
```

#### 4.7.4 多态案例制作饮品

代码实现：

```cpp
class abstractDrink {
public:
    virtual void water() = 0;
    virtual void chonpao() = 0;
    virtual void incup() = 0;
    virtual void milk() = 0;
    virtual void makeDrink() {
        water();
        chonpao();
        incup();
        milk();
    }
};

class coffee:public abstractDrink {
public:
    virtual void water() {
        cout << "煮好矿泉水" << endl;
    }
    virtual void chonpao() {
        cout << "熬煮一段时间咖啡" << endl;
    }
    virtual void incup() {
        cout << "倒入马克杯中" << endl;
    }
    virtual void milk() {
        cout << "加入柠檬和牛奶" << endl;
    }
};

class tea :public abstractDrink {
public:
    virtual void water() {
        cout << "煮好山泉水" << endl;
    }
    virtual void chonpao() {
        cout << "熬煮一段时间茶叶" << endl;
    }
    virtual void incup() {
        cout << "倒入茶杯中" << endl;
    }
    virtual void milk() {
        cout << "加入枸杞和红枣" << endl;
    }
};

void getDrink(abstractDrink* dd) {
    dd->makeDrink();
    delete dd;
    cout << "------------------" << endl;
}
void test01() {
    getDrink(new coffee);
    getDrink(new tea);
}

int main() { 
    test01();
    return 0;
}
```

#### 4.7.5 虚析构和纯虚析构

多态使用时，如果有子类的属性开辟到堆区，那么父类指针在释放时无法调用到子类的析构代码

解决方法：将父类中的析构写为虚析构或纯虚析构

虚析构和出虚析构的共性：

- 可以解决父类指针释放子类对象问题

- 都需要又具体的函数实现

虚析构和纯虚析构的区别：

- 如果是纯虚析构，该类属于抽象类，无法实例化对象

虚析构语法：`virtual ~类名(){}`

纯虚析构语法：`virtual ~类名()=0;``类名::~类名(){}`

eg：

```cpp
class animal {
public:
    virtual void speak() {
        cout << "动物正在说话" << endl;
    }
    virtual ~animal() = 0;//与一般的纯虚函数不同，纯虚析构函数必须要进行行为化，在类外
};
animal::~animal(){}
class Cat :public animal {
public:
    Cat(string s) {
        m_Name = new string(s);
    }
    void speak() {
        cout << *m_Name << "小猫在说话" << endl;
    }
    ~Cat() {
        cout << "Cat析构函数被调用" << endl;
        if (m_Name != NULL) {
            delete m_Name;
        }
    }
    string* m_Name;
};

void test01() {
    animal* an;
    an = new Cat("tom");
    an->speak();
    delete an;//当delete时才会调用Cat的析构函数；
}

int main() { 
    test01();
    return 0;
}
```

运行结果：

tom小猫在说话

Cat析构函数被调用

总结：

1. 虚析构或纯虚析构就是用来解决父类指针释放子类对象

2. 如果子类中没有堆区数据，可以不写为虚析构和纯虚析构

3. 拥有纯虚析构函数的类也属于抽象类

#### 4.7.6 多态案例三–电脑组装

案例描述：

用来自Lenovo和Intel零件类cpu，显卡，内存来组装出三台电脑

eg：

```cpp
//cpu
class centerCpu {
public:
    virtual void cpu01() = 0;
};

//显卡
class VideoCard {
public:
    virtual void videoDisplay() = 0;
};

//内存条
class memoryLong {
public:
    virtual void storage() = 0;
};

//组装电脑
class work {
public:
    void makeComputer(centerCpu* cpu0, VideoCard* vc, memoryLong* m) {
        m_cpu = cpu0;
        m_vc = vc;
        m_m = m;
        m_cpu->cpu01();
        m_vc->videoDisplay();
        m_m->storage();
    };
    ~work() {
        delete m_cpu;
        delete m_vc;
        delete m_m;
    }
private:
    centerCpu* m_cpu;
    VideoCard* m_vc;
    memoryLong* m_m;
};

//各品牌
//intel:
class intelcenterCpu :public centerCpu {
public:
    virtual void cpu01() {
        cout << "intel的cpu开始工作了" << endl;
    }
};

class intelVideoCard :public VideoCard{
public:
    virtual void videoDisplay() {
        cout << "intel的显卡开始工作了" << endl;
    }
};

class intelmemoryLong :public memoryLong{
public:
    virtual void storage() {
        cout << "intel的内存开始工作了" << endl;
    }
};

//lenovo:
class lenovocenterCpu :public centerCpu {
public:
    virtual void cpu01() {
        cout << "lenovo的cpu开始工作了" << endl;
    }
};

class lenovoVideoCard :public VideoCard{
public:
    virtual void videoDisplay() {
        cout << "lenovo的显卡开始工作了" << endl;
    }
};

class lenovomemoryLong :public memoryLong{
public:
    virtual void storage() {
        cout << "lenovo的内存开始工作了" << endl;
    }
};

void test01(){
    cout << "第一台：" << endl;
    work wo;
    wo.makeComputer(new intelcenterCpu, new intelVideoCard, new intelmemoryLong);
    cout << "---------------" << endl;
    cout << "第二台：" << endl;
    wo.makeComputer(new lenovocenterCpu, new lenovoVideoCard, new lenovomemoryLong);
    cout << "---------------" << endl;
    cout << "第三台：" << endl;
    wo.makeComputer(new intelcenterCpu, new lenovoVideoCard, new lenovomemoryLong);
}

int main() {
    test01();
    return 0;
}
```

## 5 文件操作

程序运行时产生的数据都属于临时数据，程序一旦运行结束都会释放

**通过文件可以将数据持久化**

c++中对文件操作需要包含头文件****

文件类型分为两种：

1.文本文件：文件以文本的ASCII码形式存储在计算机中

2.二进制文件：文件以文本的二进制方式存储在文件中，用户一般不能直接读懂他们

操作文件的三大类：

1.ofstream-写操作

2.ifstream -读操作

3.fstream  -读写操作

### 5.1 文本文件

#### 5.1.1 写文件

写文件步骤如下：

1. 包含头文件
`#include<fstream>`

2. 创建流对象
`ofstream ofs;`

3. 打开文件
`ofs.open("文件路径",打开方式)`

4. 写数据
`ofs<<"写入数据";`

5. 关闭文件
`ofs.close();`

文件打开方式：

打开方式
解释

`ios::in`
为读文件而打开文件

`ios::out`
为写文件而打开文件

`ios::ate`
初始位置：文件尾

`ios::app`
追加方式写文件

`ios::trunc`
如果文件存在，先删除，在创建

`ios::binary`
二进制方式

注意：文件打开方式可以配合使用，利用|运算符

eg：利用二进制方式写文件

`iOS::out | ios::binary`

eg：

```cpp
//step1
#include<fstream>

void test01(){
    //step2
    ofstream ofs;
    //step3
    ofs.open("test.txt", ios::out);//当写为文件名时，会自动创建在当前.cpp所在目录下
    //step4
    ofs << "hello world!" << endl;
    ofs << "你好 C++" << endl;
    //step5
    ofs.close();
}

int main() {
    test01();
    return 0;
}
```

总结：

- 文件操作必须包含头文件fstream

- 读文件可以利用ofstream，或者fstream类

- 打开文件时需要指定打开文件的路径，以及打开方式

- 利用<<可以向文件中写数据

- 操作完毕，要关闭文件

#### 5.1.2 读文件

读文件与写文件比较相似，但是读取方式比较多

读文件步骤如下：

1. 包含头文件
`#include<fstream>`

2. 创建流对象
`ifstream ifs;`

3. 打开文件
`ifs.open("文件路径",打开方式)`

4. 读数据
四周方式读取

5. 关闭文件
`ifs.close();`

eg：

```cpp
void test01(){
    //step2
    ifstream ifs;

    //step3读文件在打开时要判断是否打开成功
    ifs.open("test.txt", ios::in);
    if (!ifs.is_open()) {//一般判断文件是否打开成功语句要取反
        cout << "文件打开失败了！" << endl;
        return;
    }

    //step4读文件
    //法一：
    //char buf[1024] = { 0 };
    //while (ifs >> buf) {
    //    cout << buf <<endl;
    //}
    //法二：
    //char buf1[1024] = { 0 };
    //while (ifs.getline(buf1, sizeof(buf1))) {
    //    cout << buf1 << endl;
    //}
    //法三：
    //string buf2;
    //while (getline(ifs, buf2)) {
    //    cout << buf2 << endl;
    //}
    //法四：
    char c;
    while ((c = ifs.get()) != EOF) {//EOF即为end of file
        cout << c;
    }

    //step5
    ifs.close();
}

int main() {
    test01();
    return 0;
}
```

总结：

- 推荐记住读文件的法二和法三

- 读文件可以利用ifstream和fstream类

- 利用ios_open函数可以判断文件是否打开成功

- close关闭文件

### 5.2 二进制文件

以二进制方式堆文件进行读写操作

打开方式要要指定为ios::binary

#### 5.2.1 写文件

二进制写文件主要利用流对象调用成员函数write

函数原型：`ostream& write(const char* buffer ,int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

eg：

```cpp
 class person {
public:
    /*person(char name[66], int age) {
        m_Name = name;
        m_Age = age;
    }*/
    char m_Name[64];
    int m_Age;
};
void test01(){
    ofstream ofs;//也可以省略掉打开的这一步，直接在创建时写为：ofstream ofs("person.txt", ios::out | ios::binary);
    ofs.open("person.txt", ios::out | ios::binary);
    person p={ "张三",18 };
    ofs.write((const char*)&p, sizeof(person));
    ofs.close();
}

int main() {
    test01();
    return 0;
}
```

总结：

- 文件输出流对象 可以通过write函数，以二进制方式写数据

#### 5.2.2 读文件

二进制方式读文件主要利用流对象调用成员函数read

函数原型：`istream& read(char* buffer,int len);`

参数解释：字符指针buffer指向内存中一段存储空间。len是读写的字节数

eg：

```cpp
class person {
public:
    char m_Name[64];
    int m_Age;
};
void test01(){
    ifstream ifs("person.txt", ios::out | ios::binary);
    person p={ "张三",18 };
    ifs.read((char*)&p, sizeof(person));
    cout << "姓名：" << p.m_Name << "年龄：" << p.m_Age << endl;
    ifs.close();
}

int main() {
    test01();
    return 0;
}
```

总结：

- 文件输入流对象 可以通过read函数，以二进制方式读数据
