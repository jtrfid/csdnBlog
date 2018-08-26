##C++ 复制构造函数与函数返回对象 ##

 **函数返回内部局部对象,例如: **
```
A fun1()
{
	A a(20); // 调用自定义构造函数，构造a对象
	return a;
}
```
**函数调用时，自动调用复制构造函数，赋值给左端对象.例如: **
```
// 调用复制构造函数，把fun1()的返回值拷贝给a1. 本语句结束，调用析构函数，销毁函数返回对象
A a1 = fun1();  
```

###附完整示例代码

```
#include "stdafx.h"
#include <iostream>
#include <cstdlib>

using namespace std;
class A
{
public:
	A() :n(0)  // 默认构造函数
	{
		cout << "默认构造函数：" << n << endl;
	}
	A(int n) :n(n) // 自定义构造函数
	{
		cout << "自定义构造函数：" << n << endl;
	}
	A(const A& a) :n(a.n) // 复制构造函数
	{
		cout << "复制构造函数：" << n << endl;
	}
	const A& operator=(const A& r) // 重载操作符=
	{
		n = r.n;
		return(*this);
	}
	~A()
	{
		cout << "析构函数:" << n << endl;
	}
	int n;
};

A fun1()
{
	A a(20); // 调用自定义构造函数，构造a对象
	return a;
}

// 【非法】虽然程序能执行，但是返回一个局部变量的地址是不可取的
A& fun2()
{
	A a(30);   // 调用自定义构造函数，构造a对象
	return a;  // warning C4172 : 返回局部变量或临时变量的地址: a
}

void fun3()
{
	// 调用复制构造函数，把fun1()的返回值拷贝给a1. 本语句结束，调用析构函数，销毁函数返回对象(n=20)
    A a1 = fun1();  
	cout << a1.n << endl;
	a1.n = 30;
	
    A aa; // 调用默认构造函数，构造aa对象
	aa = a1; // 调用操作符=重载
	aa.n = 40;

	// error C2440: “初始化”: 无法从“A”转换为“A &”
	// note: 非常量引用只能绑定到左值
	//A& a2 = fun1(); 
	//cout << a2.n << endl;

	// 以下调用不推荐
	// A a3 = fun2();
	// cout << a3.n << endl;
}

int main()
{
	fun3(); // 本语句结束，调用析构函数，销毁fun3()中的a1对象(n=30)
	system("pause");
	return 0;
}

```
**程序执行结果**
自定义构造函数：20 ` fun1()中构造局部对象a` 
复制构造函数：20 `fun1()返回对象复制给a1 `
析构函数:20 ` 销毁函数返回对象 `
20
默认构造函数：0 ` 构造aa对象 `
析构函数:40 ` 销毁aa对象 `
析构函数:30 ` 销毁a1对象 `