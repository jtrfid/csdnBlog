## C++ assert
作用：用于Debug编译环境下的断言，Release下无效。
举例： 
```  C++
#include <iostream>
// 去下行注释则禁用 assert()
// #define NDEBUG
#include <cassert>  // 必须包含
 
int main()
{
    assert(2+2==4);  // 条件正确，正常执行
    std::cout << "Execution continues past the first assert\n";
    assert(2+2==5);  // 条件错误，跳出执行
    std::cout << "Execution continues past the second assert\n";
}
```