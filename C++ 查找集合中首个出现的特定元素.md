##C++ 查找集合中出现的首个特定元素  ##

###`map<key,value>`,`set<key>`等集合###
   
使用各自的成员函数find()完成此任务。

###`vector<>`集合###
使用定义于头文件`<algorithm>`的算法:
返回范围 [first, last) 中满足特定判别标准的首个元素。
template< class InputIt, class T >
InputIt find( InputIt first, InputIt last, const T& value );

###定义于头文件`<algorithm>`的其他相关算法##
* search 查找一个元素区间 
* search_n 在区间中搜索连续一定数目次出现的元素 
* adjacent_find 查找彼此相邻的两个相同（或其它的关系）的元素 
* 二分搜索操作（在已排序范围上）
  - lower_bound 返回指向第一个不小于给定值的元素的迭代器 
  - upper_bound 返回指向第一个大于给定值的元素的迭代器 
  - binary_search 判断一个元素是否在区间内 
  - equal_range 返回匹配特定键值的元素区间 

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

int main()
{
	// set<>示例
	set<string> set1 = { "s1","s2","s3" };
	if (set1.find("s2") != set1.end()) cout << "found\n"; // found
	else cout << "not found!\n";
	if (set1.find("s4") != set1.end()) cout << "found\n";
	else cout << "not found!\n";  // not found!

	// vector<>示例
	int n1 = 3;
	int n2 = 5;
	std::vector<int> v{ 0, 1, 2, 3, 4 };
	auto result1 = std::find(std::begin(v), std::end(v), n1);
	auto result2 = std::find(std::begin(v), std::end(v), n2);
	if (result1 != std::end(v)) {
		std::cout << "v contains: " << n1 << '\n'; // v contains: 3
	}
	else {
		std::cout << "v does not contain: " << n1 << '\n';
	}

	if (result2 != std::end(v)) {
		std::cout << "v contains: " << n2 << '\n';
	}
	else {
		std::cout << "v does not contain: " << n2 << '\n'; // v does not contain: 5
	}

	system("pause");
	return 0;
}
```

************************************************************
##参考##
[C++参考手册 https://zh.cppreference.com/w/cpp/algorithm/](https://zh.cppreference.com/w/cpp/algorithm/)