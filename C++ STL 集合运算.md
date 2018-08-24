##C++ STL 集合运算 ##
**定义于头文件`<algorithm>`的算法：**
 
 **集合运算的前提是两个集合必须按照同样的规则排序就绪，否则不能进行集合运算! **    
   - **map,set是有序集合,可以直接参加运算;vector是无序集合,参与运算前必须首先排序.**

    template< class InputIt1, class InputIt2, class OutputIt >
    前两个参数是输入参数，代表参与集合运算的两个集合，最后一个是输出参数，输出运算结果(**也是相同排序规则的集合**)。
    
- **交集(set_intersection) $A \cap B$**
  OutputIt set_intersection( InputIt1 first1, InputIt1 last1,InputIt2 first2, InputIt2 last2, OutputIt d_first );
- **并集(set_union) $A \cup B$**
  OutputIt set_union( InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first );
- **差集(set_difference) $A - B$**
  OutputIt set_difference( InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first );
- **对称差集(set_symmetric_difference) $A \cup B - (A \cap B)$**                 
  OutputIt set_symmetric_difference( InputIt1 first1, InputIt1 last1, InputIt2 first2, InputIt2 last2, OutputIt d_first ); 
- **如果使用比较函数，末尾再追加一个参数，用法见示例.**

###附完整示例代码

```
#include "stdafx.h"
#include <iostream>
#include <string>
#include <vector>
#include <map>
#include <set>
#include <algorithm>
#include <iterator>

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

// 使用默认比较函数less<>排序
int main1()
{
	std::vector<int> v1{ 1,2,3,4,5,6,7,8 };
	std::vector<int> v2{ 5,  7,  9,10 };
	std::sort(v1.begin(), v1.end());  // 升序排序,默认使用less<int>比较函数
	std::sort(v2.begin(), v2.end());  

	// A交B
	cout << "set_intersection:" << endl;
	std::vector<int> v_intersection;
	std::set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(v_intersection));

	for (int n : v_intersection) std::cout << n << ' '; cout << endl; // 5 7

	// A并B
	cout << "set_union" << endl;
	std::vector<int> dest1;
	std::set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(dest1));
	for (const auto &i : dest1) std::cout << i << ' ';	std::cout << '\n';  // 1 2 3 4 5 6 7 8 9 10

	// A - B
	cout << "set_difference" << endl;
	std::vector<int> diff;
	std::set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(diff));
	for (int n : diff) std::cout << n << ' '; cout << endl;  // 1 2 3 4 6 8
	
	// 对称差(symmetric_difference,A并B - A交B)
	cout << "set_symmetric_difference" << endl;
	std::vector<int> v_symDifference;
	std::set_symmetric_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(v_symDifference));
	for (int n : v_symDifference) std::cout << n << ' '; // 1 2 3 4 6 8 9 10
	cout << endl;

	system("pause");
	return 0;
}

// 使用非默认比较函数
int main()
{
	
	std::vector<int> v1{ 1,2,3,4,5,6,7,8 };
	std::vector<int> v2{ 5,  7,  9,10 };
	//std::sort(v1.begin(), v1.end());  // 升序排序,默认使用less<int>比较函数
	//std::sort(v2.begin(), v2.end());

	// 采用非默认比较函数，
	std::sort(v1.begin(), v1.end(), greater<int>());  // 降序排序
	std::sort(v2.begin(), v2.end(), greater<int>());

	// 集合运算必须与集合的排序规则一致，因此最后一个参数需设置比较函数
	// A交B
	cout << "set_intersection:" << endl;
	std::vector<int> v_intersection;
	std::set_intersection(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(v_intersection),greater
	<int>());

	for (int n : v_intersection) std::cout << n << ' '; cout << endl; // 7 5

	// A并B
	cout << "set_union" << endl;
	std::vector<int> dest1;
	std::set_union(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(dest1),greater<int>());
	for (const auto &i : dest1) std::cout << i << ' ';	std::cout << '\n';  // 10 9 8 7 6 5 4 3 2 1 
																			
	// A - B
	cout << "set_difference" << endl;
	std::vector<int> diff;
	std::set_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(diff),greater<int>());
	for (int n : diff) std::cout << n << ' '; cout << endl;  // 8 6 4 3 2 1

	// 对称差(symmetric_difference,A并B - A交B)
	cout << "set_symmetric_difference" << endl;
	std::vector<int> v_symDifference;
	std::set_symmetric_difference(v1.begin(), v1.end(), v2.begin(), v2.end(), std::back_inserter(v_symDifference),greater<int>());
	for (int n : v_symDifference) std::cout << n << ' '; // 10 9 8 6 4 3 2 1
	cout << endl;

	set<string, cmpByKey> set1 = { "P1","P10","P3","P20" };
	set<string, cmpByKey> set2 = { "P10","P2" };
	set<string, cmpByKey> results;
	
    // 编译出错，修改为下句
	//std::set_intersection(set1.begin(), set1.end(), set2.begin(), set2.end(), std::back_inserter(results), cmpByKey());
	std::set_intersection(set1.begin(), set1.end(), set2.begin(), set2.end(), std::inserter(results, results.end()), cmpByKey());
	
    for (string s : results) std::cout << s << ' '; cout << endl;  // P10
	results.clear();
	std::set_union(set1.begin(), set1.end(), set2.begin(), set2.end(), std::inserter(results, results.end()), cmpByKey());
	for (string s : results) std::cout << s << ' '; cout << endl;  // P1 P2 P3 P10 P20
	results.clear();
	std::set_difference(set1.begin(), set1.end(), set2.begin(), set2.end(), std::inserter(results, results.end()), cmpByKey());
	for (string s : results) std::cout << s << ' '; cout << endl;  // P1 P3 P20
	results.clear();
	std::set_symmetric_difference(set1.begin(), set1.end(), set2.begin(), set2.end(), std::inserter(results, results.end()), cmpByKey());
	for (string s : results) std::cout << s << ' '; cout << endl;  // P1 P2 P20

	system("pause");
	return 0;
}

```

************************************************************
##参考##
[C++参考手册 https://zh.cppreference.com/w/cpp/algorithm/](https://zh.cppreference.com/w/cpp/algorithm/)