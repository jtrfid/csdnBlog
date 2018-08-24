##C++ 检测两个集合是否相等 ##
**定义于头文件`<algorithm>`的算法：**
两个集合，按照元素的顺序逐个比较。
- 如果范围 [first1, last1) 和范围 [first2, last2) 相等，返回 true ，否则返回 false
  template< class InputIt1, class InputIt2 >
  bool equal( InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2 ); 
- 如果范围 [first1, last1) 和范围 [first2, first2 + (last1 - first1) 相等，返回 true ，否则返回 false
  template< class InputIt1, class InputIt2 >
  bool equal( InputIt1 first1, InputIt1 last1, InputIt2 first2 );


###附完整示例代码

```
#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <set>
#include <algorithm>

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

using namespace std;

// 下面的代码使用 equal() 来测试字符串是否是回文
void test(const std::string& s)
{
	if (std::equal(s.begin(), s.begin() + s.size() / 2, s.rbegin())) {
		std::cout << "\"" << s << "\" is a palindrome\n";
	}
	else {
		std::cout << "\"" << s << "\" is not a palindrome\n";
	}
}

int main()
{
	cout << "回文检测" << endl;
	test("radar"); // "radar" is a palindrome
	test("hello"); // "hello" is not a palindrome
    
    
    // 相同比较函数，检测是否相等才有意义,按顺序比较。
	// 排序后，前三个元素相同，P10, 1 | P2, 2 | P3, 3
	map<string, int, less<string>> map1 = { {"P10",1},{"P2",2},{"P3",3} };
	map<string, int, less<string>> map2 = { { "P3",3 },{ "P2",2 },{ "P10",1 },{ "P4",4 } };
	
	// 排序后，前三个元素不同
	//map<string, int, greater<string>> map1 = { { "P10",1 },{ "P2",2 },{ "P3",3 } };
	//map<string, int, greater<string>> map2 = { { "P3",3 },{ "P2",2 },{ "P10",1 },{ "P4",4 } };

	// 排序后，前三个元素不同
	//map<string, int, cmpByKey> map1 = { { "P10",1 },{ "P2",2 },{ "P3",3 } };
	//map<string, int, cmpByKey> map2 = { { "P3",3 },{ "P2",2 },{ "P10",1 },{ "P4",4 } };

	for (auto it = map1.begin(); it != map1.end(); it++)
	{
		if (next(it) == map1.end()) cout << it->first << "," << it->second << endl;
		else cout << it->first << "," << it->second << " | ";
	} // P10, 1 | P2, 2 | P3, 3

	for (auto it = map2.begin(); it != map2.end(); it++)
	{
		if (next(it) == map2.end()) cout << it->first << "," << it->second << endl;
		else cout << it->first << "," << it->second << " | ";
	} // P10, 1 | P2, 2 | P3, 3 | P4, 4

	// 检测两个集合是否相等，如果范围 [first1, last1) 和范围 [first2, last2) 相等，返回 true ，否则返回 false
	bool isEqual = std::equal(map1.begin(), map1.end(), map2.begin(), map2.end());
	cout << isEqual << endl; // 0, 表示不相等

	// 检测两个集合是否相等，仅比较范围[first1, last1) 和范围 [first2, first2 + (last1 - first1)
	isEqual = std::equal(map1.begin(), map1.end(),map2.begin());
	cout << isEqual << endl; // 1, 表示相等


	system("pause");
	return 0;
}

```

************************************************************
##参考##
[C++参考手册 https://zh.cppreference.com/w/cpp/algorithm/equal](https://zh.cppreference.com/w/cpp/algorithm/equal)