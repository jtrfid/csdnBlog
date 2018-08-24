## C++ 唯一键集合map的安全可控插入修改数据 ##
**map<key,value>是key-value键值对集合，唯一key对应一个value值，key是唯一的，value可重复。插入或修改键值对时注意一下事项：**
1. operator[key]读取对应的value值，但是当无key时，自动插入了一对key-value键值对，value是默认值，不可控。
2. operator[key] = value; 插入kye-value键值对，当key存在时，修改它对应的value值为新值
3. insert(make_pair(key,value));用于插入key-value键值对，当key存在时，此语句无效，不会引起对原value的修改。
4. 安全可控的插入或修改key-value键值对，应该是首先查找key是否存在，如果存在，用operator[key] = value修改原值，否则insert(make_pair(key,value))插入新的键值对。

```
#include <string> 
#include <iostream>
#include <map>
#include <set>
using namespace std;

int main()
{
  map<string,int> map1; // key-value键值对，唯一key对应一个value值，key是唯一的，value可重复
  map1["key1"] = 1;
  map1["key2"] = 2;
  map1["key1"] = 3; // 修改key="key1"的值为3
  map1.insert(make_pair("key1", 4));  // key="key1"已经存在，此插入操作无效
  // 无意引起的插入键值对：
  cout << "test: " << map1["key3"] << endl; // 造成插入了key="key3"，value是默认值，不可控
		
  // 安全可控插入或修改键值对：
  if (map1.find("key3") == map1.end()) // 不存在key-value
	  map1.insert(make_pair("key3", 3));
  else // 存在key修改value
	  map1["key3"] = 3;

  for (auto it = map1.begin(); it != map1.end(); it++)
	cout << it->first << "," << it->second << "," << map1[it->first] << "\t";
  cout << endl;
}
```
