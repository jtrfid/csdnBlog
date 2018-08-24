##C++ STL 集合排序 ##

###`map<key,value>`和`set<key>`的内置键值比较函数排序###
```
// 按照键值升序排列
map<string,int,less<string>> map1; // 等效map<string,int> map1; 
set<string,less<string>> set1; // 等效set<string>;

// 按照键值降序排列
map<string,int,greater<string>> map1; 
set<string,greater<string>> set1; 
```

###`map<key,value>`和`set<key>`的自定义比较函数排序###

**首先定义比较函数,例如：**
```
/***************************************************
 * map key compare
 * include compare string's length
 * 用于排序map的keys，如："P1","P2","P10",...
 * 用法：
 * （1）map键值排序：map<string,int,cmpByKey> map1;
 * （2）set排序: set<string,cmpByKey> set1;
 * （3）vector<string>排序：
 *      #include <algorithm>
 *      vector<string> vec1;
 *      std::sort(vec1.begin(), vec1.end(), cmpByKey());
 ****************************************************/
struct cmpByKey {
	bool operator()(const std::string& lhs, const std::string& rhs) const
	{
		bool ret;
		if (lhs.size() == rhs.size())
			ret = lhs < rhs ? true : false;
		else
			ret = lhs.size() < rhs.size() ? true : false;
		return ret;
	}
};
```
**其次声明使用自定义比较函数的对象**
```
// 使用自定义键值比较函数的map,set对象
map<string,int,cmpByKey> map1;
set<string,cmpByKey> set1;
```

###定义比较函数的"严格弱序化"要求###

   比较函数必须满足数学意义上的标准**严格弱序化**，否则这些方法或容器的行为将不可预知。
假设f(x,y)是一个比较函数。 如果该函数满足如下条件则它是**严格弱序化的**。
1. 非自反性(irreflexive)：`f(x,x) = false;`
2. 非对称性(antisymmetric): `if f(x,y) then !f(y,x)`
3. 可传递性(transitivity): `if f(x,y) and f(y,z) then f(x,z)`
4. 等效传递性(transitivity of equivalence):
   `if !f(x,y)&&!f(y,x) then x==y; if x==y and y==z then x==z;`

   比较方法能够满足对相等元素永远返回false（记住一个准则：永远让比较函数对相同元素返回false），那你的方法就满足要求了。

   其实，set容器在判定已有元素a和新插入元素b是否相等时，是这么做的：1）将a作为左操作数，b作为右操作数，调用比较函数，并返回比较值  2）将b作为左操作数，a作为右操作数，再调用一次比较函数，并返回比较值。如果1、2两步的返回值都是false，则认为a、b是相等的，则b不会被插入set容器中；如果1、2两步的返回值都是true，则可能发生未知行为，因此，记住一个准则：永远让比较函数对相同元素返回false。

###`vector<>`集合的排序###

**定义比较函数同上**
**使用algorithm中的sort()函数排序**
```
#include <algorithm>
vector<string> vec1;
std::sort(vec1.begin(), vec1.end(), cmpByKey());
```

###附完整示例代码

```
#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <set>
#include <algorithm>

using namespace std;

/***************************************************
 * map key compare(要求严格弱序化,即相等元素,返回false)
 * include compare string's length
 * 用于排序map的keys，如："P1","P2","P10",...
 * 用法：
 * （1）map键值排序：map<string,int,cmpByKey> map1;
 * （2）set排序: set<string,cmpByKey> set1;
 * （3）vector<string>排序：
 *      #include <algorithm>
 *      vector<string> vec1;
 *      std::sort(vec1.begin(), vec1.end(), cmpByKey());
 ****************************************************/
struct cmpByKey {
	bool operator()(const std::string& lhs, const std::string& rhs) const
	{
		bool ret;
		if (lhs.size() == rhs.size())
			ret = lhs < rhs ? true : false;
		else
			ret = lhs.size() < rhs.size() ? true : false;
		return ret;
	}
};

int main()
{
	cout << "map内置比较函数排序(升序)" << endl;
	map<string, int, less<string>> map1;  // 使用默认系统定义的键值比较函数(升序)，等效于map<string, int> map1;
	map1.insert(make_pair("P10", 1));
	map1.insert(make_pair("P2", 2));
	map1.insert(make_pair("P3", 3));
	map1.insert(make_pair("P3", 33)); // 无效
	map1["P4"] = 4;  // 插入key "P4"的value为4
	map1["P4"] = 44;  // 修改key "P4"的value为44
	for (auto it = map1.begin(); it != map1.end(); it++)
	{
		if (next(it) == map1.end()) cout << it->first << "," << it->second << endl;
		else cout << it->first << "," << it->second << " | ";
	} // P10, 1 | P2, 2 | P3, 3 | P4, 44

	cout << "map内置比较函数排序(降序)" << endl;
	map<string, int, greater<string>> map2; // 使用系统定义的键值比较函数(降序)
	map2.insert(make_pair("P10", 1));
	map2.insert(make_pair("P2", 2));
	map2.insert(make_pair("P3", 3));
	for (auto it = map2.begin(); it != map2.end(); it++)
	{
		if (next(it) == map2.end()) cout << it->first << "," << it->second << endl;
		else cout << it->first << "," << it->second << " | ";
	} // P3,3 | P2,2 | P10,1

	cout << "set内置比较函数排序(降序)" << endl;
	set<string, greater<string>> set1;  // 降序(greater)，升序(less)
	set1.insert("P10");
	set1.insert("P2");
	set1.insert("P3");
	for (auto it = set1.begin(); it != set1.end(); it++)
	{
		if (next(it) == set1.end()) cout << *it << endl;
		else cout << *it << ",";
	} // P3,P2,P10
	
	cout << "map自定义比较函数排序" << endl;
	map<string, int, cmpByKey> map3;
	map3.insert(make_pair("P10", 1));
	map3.insert(make_pair("P2", 2));
	map3.insert(make_pair("P3", 3));
	map3.insert(make_pair("P3", 33)); // 无效
	map3["P4"] = 4;  // 插入key "P4"的value为4
	map3["P4"] = 44;  // 修改key "P4"的value为44

	for (auto it = map3.begin(); it != map3.end(); it++)
	{
		if(next(it) == map3.end()) cout << it->first << "," << it->second << endl;
		else cout << it->first << "," << it->second << " | ";
	} // P2,2 | P3,3 | P4,44 | P10,1

	cout << "set自定义比较函数排序" << endl;
	set<string, cmpByKey> set2;
	set2.insert("P10");
	set2.insert("P2");
	set2.insert("P3");
	set2.insert("P3"); // 无效
	for (auto it = set2.begin(); it != set2.end(); it++)
	{
		if (next(it) == set2.end()) cout << *it << endl;
		else cout << *it << ",";
	} // P2,P3,P10
	
	cout << "vector<>内置比较函数排序(升序)" << endl;
	vector<string> vect1 = { "P10","P2","P3","P2" };
	vector<string> vect1 = { "P10","P2","P3","P2" };
	// std::sort(vect1.begin(), vect1.end(), less<string>()); // 等效默认
    std::sort(vect1.begin(), vect1.end()); // 默认
	for (auto it = vect1.begin(); it != vect1.end(); it++)
	{
		if (next(it) == vect1.end()) cout << *it << endl;
		else cout << *it << ",";
	} // P10,P2,P2,P3

	cout << "vector<>内置比较函数排序(降序)" << endl;
	vector<string> vect2 = { "P10","P2","P3","P2" };
	std::sort(vect2.begin(), vect2.end(), greater<string>());
	for (auto it = vect2.begin(); it != vect2.end(); it++)
	{
		if (next(it) == vect2.end()) cout << *it << endl;
		else cout << *it << ",";
	} // P3,P2,P2,P10

	cout << "vector<>自定义比较函数排序" << endl;
	vector<string> vect3 = { "P10","P2","P3","P2" };
	std::sort(vect3.begin(), vect3.end(), cmpByKey());
	for (auto it = vect3.begin(); it != vect3.end(); it++)
	{
		if (next(it) == vect3.end()) cout << *it << endl;
		else cout << *it << ",";
	} // P2,P2,P3,P10

	system("pause");
    return 0;
}

```

************************************************************
##参考##
[C++参考手册 https://zh.cppreference.com/](https://zh.cppreference.com/)
[c++中std::set自定义去重和排序函数 - 南宫轩诺 - 博客园](https://www.cnblogs.com/litaozijin/p/6665595.html)
[c++ set容器排序准则 - 每天一点积累](https://www.cnblogs.com/hong2016/p/6700146.html)