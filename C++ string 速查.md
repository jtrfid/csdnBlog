## 头文件和命名空间 ##
    #include <string> 
	using namespace std;
    
## 一. string 常用成员函数 ##

**1. 构造函数(构造string对象)**

```
string s1;  // 构造一个空字符串对象
string s2("example string1");
string s3 = "example string2";
string const s4("const example"); // 字符串常量, 与 s5的声明等效
const string s5("const example"); // 字符串常量, 与 s4的声明等效
string s6 = { 'a','b' };  // "ab"
string s7(s6); // "ab", 以s6对象构造s7对象
s4[0] = 'a'; // 非法
s5[0] = 'a'; // 非法
s1[0] = 'a'; // 合法
```

**2. 使用 at,operator[] 访问指定index（从0开始)对应的字符**

  - at 有越界检查，如果index越界，无论在Debug还是Release编译环境下，程序异常跳出执行；运行exe程序，弹出程序停止工作窗口。

  - operator[ ] 无越界检查，但是如果越界，取到不可预知字符

```
string s("a");
cout << s[0]; // a
cout << s[1]; // 不会发生异常，但是结果不可预知
count << s.at(1); // 异常跳出执行
```

**3. front, bakc 分别访问首尾字符 **

```
string s("abc");
cout << s.front(); // a
cout << s.end(); // c
```

**4. 指向首字符和末尾字符的迭代器（begin,end）**

![图片][begin_end_png]

```
string s("012345");
for (string::iterator it = s.begin(); it != s.end(); it++)
   { cout << *it; } // 012345
```
**5. 指向首字符和末尾字符的逆向迭代器（rbegin,rend） **

![图片][rbegin_rend_png]

```
string s("012345");
for (string::iterator it = s.rbegin(); it != s.rend(); it++)
   { cout << *it;  } // 543210
```
**6. 指向字符串常量的迭代器(cbegin,cend)及逆向迭代器(crbegin,crend) **
```
const string s("012345")
for (string::const_iterator it = s.cbegin(); it != s.cend(); it++)
  { cout << *it; } // 012345, 只读内容,不能修改. 如, *it = 'a'; 非法
```

**7. 应用auto类型，指代以上迭代器更方便 **
```
const string s("012345");
for (auto it = s.cbegin(); it != s.cend(); it++)
  { cout << *it; }  // it自动转换为只读迭代器
```
**8. empty()检查字符串是否为空, clear()清空字符串, size()/length()返回字符串中的字符数 **

```
string s1 = "abc", s2 = "012345";
cout << s1.size() << "\t" << s2.length() << endl; // 3 6
s1.clear();
cout << s1.size << "\t" << s1.empty() << s2.empty() << endl; // 0 1 0
```
**9. insert()在指定index处插入字符或字符串 **
```
string s = "abc";
// insert(size_type index, size_type count, char ch),在index处插入count个ch
s.insert(1, 2, 'D'); // "aDDbc"

s = "abc";
// insert(size_type index, const char* s),在index处插入C语言字符串s
// insert(size_type index, const string& s),在index处插入字符串s
s.insert(1, "123"); // "a123bc"

s = "abc.xt"; 
// insert(const_iterator pos, char ch)
s.insert(s.cbegin() + s.find_last_of('.') + 1, 't'); // "abc.txt" 
```

**10. erase()删除字符 **

// 从index处开始删除count个字符; 不指定count, 则index到末尾的字符全部删除
basic_string& erase( size_type index = 0, size_type count = npos );

// 删除位于position处的字符
iterator erase( iterator position );
iterator erase( const_iterator position );

// 删除first到last的字符
iterator erase( iterator first, iterator last );
iterator erase( const_iterator first, const_iterator last );

```
 string s = "This is an example";
 s.erase(0, 5);     // 删除 "This "
 cout << s << '\n'; // is an example
 s.erase(s.find(' ')); // 从' '到字符串尾裁剪, 不指定count, 则index到末尾的字符全部删除
 cout << s << '\n';    // is
 s.erase(s.begin()); // s, 删除位于position处的字符
 cout << s << endl;
```
**11. replace() 替换 **

以新字符串替换 [pos, pos + count) 或 [first, last) 所指示的 string 部分。
basic_string& replace( size_type pos, size_type count, const basic_string& str );
basic_string& replace( const_iterator first, const_iterator last, const basic_string& str );
basic_string& replace( size_type pos, size_type count, const CharT* cstr );
basic_string& replace( const_iterator first, const_iterator last, const CharT* cstr );

```
 string str("The quick brown fox jumps over the lazy dog.");
 str.replace(10, 5, "red"); // The quick red fox ...
 str.replace(str.begin(), str.begin() + 3, 1, 'A'); // A quick red fox ...
 cout << str << '\n'; // A quick red fox jumps over the lazy dog.
```

**12. append(),operator+=,assign()的使用方法 **

```
string s;
int a = 10;
s.assign("P").append(to_string(a)); // 给s赋值"P",后附"10"
// 或
s += "P" + to_string(a);
```
**13. compare(const string& str)比较两个字符串,返回 >0,0,<0; 分别表示当前字符串大于,等于,小于str **

```
// 逐字符字典序比较，可以用关系运算符（ < 、 <= 、 == 、 > 等）替代
string s1 = "P2",s2="P10";
cout << s1.compare(s2) << endl; // 1,表示s1大于s2
cout << (s1 > s2) << endl;      // 1,表示s1大于s2
```

**14. substr()取子串 **

basic_string substr( size_type pos = 0, size_type count = npos ) const;
返回子串 [pos, pos+count). 若请求的子串越过 string 的结尾, 或若 count == npos, 则返回的子串为 [pos, size()).

```
string s("abcd");
string sub1 = s.substr(1);    // "bcd"
string sub2 = s.substr(1,2);  // "bc" 
```
**15. find()从pos开始，查找首个等于给定字符序列的子串 **

// 从pos开始，查找首个等于给定字符序列的子串,找到返回index,否则返回string::npos
size_type find( const basic_string& str, size_type pos = 0 ) const
size_type find( const CharT* s, size_type pos = 0 ) const;

// 查找单个字符
size_type find( CharT ch, size_type pos = 0 ) const;
```
string::size_type n;
const string s = "This is a string";
// 从 pos = 0 开始搜索
n = s.find("is");
if(n == string::npos) cout << "not found";
else cout << "found: " << s.substr(n); // is is a string
// 从位置 pos = 5 开始搜索
n = s.find("is", 5);
if(n == string::npos) cout << "not found";
else cout << "found: " << s.substr(n); // is a string
// 寻找单个字符
n = s.find('a');
if(n == string::npos) cout << "not found";
else cout << "found: " << s.substr(n); // a string
// 寻找单个字符
n = s.find('q');
if(n == string::npos) cout << "not found"; // not found
else cout << "found: " << s.substr(n); 
```
|      类似的查找函数      |
|------|------|
|rfind|寻找子串的最后一次出现| 
|find_first_of|寻找字符的首次出现| 
|find_first_not_of|寻找字符的首次缺失| 
|find_last_of|寻找字符的最后一次出现| 
|find_last_not_of|寻找字符的最后一次缺失| 

**16. c_str() 返回C语言字符串常量，常用于使用C语言的字符串库函数时使用 **
```
string s1("ABc"), string s2("abc");
int a = _strcmpi(s1.c_str(), s2.c_str()); // 忽略大小写比较两个c语言字符串
cout << a << endl; // 0
```

## 二. 非成员函数 ##
**1. operator+ 连接两个字符串或者一个字符串和一个字符 **
```
string s1 = "Hello", s2 = "world";
cout << s1 + ' ' + s2 + "!\n";
```
**2. 以字典序比较二个字符串 **
operator==, operator!=, operator<, operator>, operator<=, operator>=
```
string s1 = "P1", s2 = "P2";
if(s1 == s2) cout << "equal."
else cout << "not equal!";
```
**3. getline()从I/O流读取数据到字符串 **
```
// 从cin读取字符并将它们放进str, 回车('\n')结束
string str;
while(str.empty()) // 为了防止用户直接回车，而得到一个空串
  getline(cin, str); // str中不包含'\n'

// 从文件读取一行
string line;
ifstream fin("test.txt");  // 添加 #include <fstream>
if (!fin) cout << "Error!! read test.txt.";
else 
{
    while (!fin.eof()) 
    {
		getline(fin, line);
        // ....
    }
    fin.close();
}
```

**4. stoi(const string& str), stof(const string& str)字符串转整型和浮点型 ** 
```
string str1 = "45";
string str2 = "3.14159";
string str3 = "31337 with words";
string str4 = "words and 2";
int myint1 = std::stoi(str1);
int myint2 = std::stoi(str2);
int myint3 = std::stoi(str3);
// int myint4 = std::stoi(str4); // 错误： 'std::invalid_argument'

cout << myint1 << '\n'; // 45
cout << myint2 << '\n'; // 3
cout << myint3 << '\n'; // 31337
```
**5. to_string()数值转字符串 **
```
double f = 23.43;
double f2 = 1e-9;
double f3 = 1e40;
double f4 = 1e-40;
double f5 = 123456789;

string f_str = std::to_string(f);   // 23.430000
string f_str2 = std::to_string(f2); // 注意：返回 "0.000000"
string f_str3 = std::to_string(f3); // 注意：不返回 "1e+40".
string f_str4 = std::to_string(f4); // 注意：返回 "0.000000"
string f_str5 = std::to_string(f5); // 123456789.000000
```

## 三. 综合应用 ##

```
/*************************************
 * 去除前后缀空格('\n','\r','\t',' ')
 *************************************/
static string& trim(string& s)
{
	if (s.empty()) return s;
	// 去掉s的前缀空格
	string::size_type pos = 0;
	for (auto it = s.begin(); it < s.end(); it++) {
		if (*it == '\n' || *it == '\r' || *it == '\t' || *it == ' ') pos++;
		else break;
	}
	s = s.substr(pos);

	// 删除s的后缀空格
	while (1)
	{
		char end = s.back();
		if (end == '\n' || end == '\r' || end == '\t' || end == ' ')
			s.erase(prev(s.end()));
		else break;
	}
	return s;
}

/*************************************
 * 首先去除s的前后缀空格，
 * 然后提取以指定分隔符隔开的子字符串至sVector.
 * 并且保证各个子串前后无空格
 * 空串或去除前后缀的空格后是空串，直接返回
 *************************************/
static vector<string>& split(const string& s, vector<string>& sVector, char delimit)
{
	if (s.empty()) return sVector;
	string str(s); // 复制s至str
	trim(str); // 去除前后缀空格('\n','\r','\t',' ')
	if (str.empty()) return sVector;
	string::size_type pos1 = 0, pos2 = 0;
	while (1)
	{
		pos2 = str.find(delimit, pos1); // 从pos1开始找分隔符
		if (pos2 == string::npos)  break; // 未找到分隔符
		string sub = str.substr(pos1, pos2 - pos1);
		trim(sub);
		if (sub.empty()) std::cout << "Waring, have empty sub string in " << s << endl;
		sVector.push_back(sub);
		pos1 = pos2 + 1;
	}
	// 添加最后一个
	string sub = str.substr(pos1);
	trim(sub);
	if (sub.empty()) std::cout << "Waring, have empty splited substring in " << s << endl;
	sVector.push_back(sub);

	return sVector;
}

/*************************************
 * 获取文件扩展名（如，".docx")
 * 参数ext返回文件扩展名
 * 无扩展名，函数返回false; 否则返回true
 *************************************/
static bool extFile(const string& file, string& ext)
{
	string::size_type pos;
	pos = file.find_last_of('.');
	if(pos == string::npos) return false;
	ext = file.substr(pos);
	return true;
}

/*************************************
 * 字符串中的小写字母转为大写字母
 *************************************/
static string& toUpper(string &s)
{
	for (string::iterator it = s.begin(); it != s.end(); ++it)
		if (*it >= 'a' && *it <= 'z') *it = *it - ('a' - 'A');
	return s;
}

/*************************************
 * 调用C语言字符串库函数，忽略大小写，比较两个字符串
 * s1 > s2, s1= s2, s1 < s2 分别返回: 大于0,0,小于0的整数 
 *************************************/
static int compareI(const string& s1, const string& s2)
{
	return _strcmpi(s1.c_str(), s2.c_str());
}

```

******************

## 参考: ##

[C++ 参考手册 https://zh.cppreference.com/w/cpp/string/basic_string](https://zh.cppreference.com/w/cpp/string/basic_string)



[begin_end_png]:data:image/png;base64,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L15sFAAAAED9yJ04WeG5e/duc6R0NGmT9sADD6j+/fsbBQAAANAwhx12mNq4caNRpaNJmzQ7L+2tt94yRwAAAADqRxYerlmzxqjS0aRNmtCtWzf1y1/+0igAAACA+jn33HPVqlWrjCodTd6kzZ07V910001GAQAAANTPypUr1bx584wqHU3epMlS2kmTJhkFAAAAUD8/+tGP1PDhw40qHU3epD3//PP6OZ4AAAAAPoh3OPLII40qHU3epMlzPPfff3+1detWcwQAAACgbt5//329Dccbb7xhjpSGJm/ShN69e7N4AAAAALw56qij1G9/+1ujSkOzMGkVFRXqq1/9qlEAAAAA9SNz0mRuWilpFiZt+fLlasGCBUYBAAAA1M/s2bPVrbfealRpaBYm7Qc/+IEaO3asUQAAAAD1c8MNN6gLL7zQqNLQLEya3FNu27atUQAAAAD1893vfrfkF3iahUl7++239SoNiQAAAAAN8dRTT6mOHTsaVRqahUkT2rVrV/JVGgAAANA4kK27DjjgALV3715zJD7NxqSVlZXpuWkAAAAADbFnzx617777qm3btpkj8Wk2Ju2yyy7TqzwBAAAAfGjfvr165plnjIpPszFpX//619XMmTONAgAAAKifESNGqB/+8IdGxafZmLS1a9eqPn36GAUAAABQP7NmzVJ33HGHUfFpNiZty5YtqkWLFkYBAAAA1M+yZcvUkiVLjIpPszFpO3bs0Ntw7Nq1yxwBAAAAqJs777xTX00rFc3GpH388cfapG3fvt0cAQAAAKgbmY82evRoo+LTbEya0Lp1a/XCCy8YBQAAAFA3Tz75pDr++OONik+zMmndunVTjz/+uFEAAAAAdfPiiy+qli1bGhWfZmXShg8frh588EHmpQEAAECDvPnmm3qqlEyZevnll83ReDQLk3bzzTerOXPmqClTpqh77rlHnX/++eqss84yvQAAAAA1mTZtmrr33nv1Uwd+97vfqWHDhqn777/f9MahWZg0mYd29NFH633SxKjJpctf/epXphcAAACgJl/60pfUEUccoQ499FA1duxY1blzZ/X666+b3jg0C5MmjBkzRh144IH6ze7Zs6d+cCoAAACAC7nA07ZtW7XffvvpW56luAPXbEzaV77yFf0mSxs1apQ5CgAAAFCbvXv3ar9gvcN9991neuLRbEyaOOJWrVrp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

[rbegin_rend_png]:data:image/png;base64,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ay1x3nnnyfmquCufEJIcuFIydOhQefPhlClTxL///W9x0EEHiRdeeEF5EEIqkb333lteSSRhKk5sQnjttddeYsyYMWpLeYOh+9mzZ4szzjhDTlD+17/+JW666Sbx888/Kw9CSFL8+OOPYuLEifIJZUcddZT44IMPVIkQw4YNExtssIGcpvLyyy+rrYSQSuLJJ58Ua6yxhvjiiy/UFgIqTmwCXPrC5XQsi1Su4Nms7du3l5fId9ppJ9GvXz8+NouQhEGH8dBDD8mb6o499lg5knn22WeLuXPtzxSFGB0wYIBo3Lix2HbbbUWbNm3kgs+YwsMpLIRUBsccc4zo3LmzsgioSLEJcHf6mWeeqazyAaOYWKi+QYMG4rLLLhPPPfecKiGE1BbcOfrJJ5+Ixx57TEyaNEle/sJVAohFTEVZbbXVxKGHHip69uwpnnjiiVo9jWzRokXyKsP5558vheff/vY3sfXWW8sVIC6//HIxZMgQcdddd8kb+L777jv1LkJIufP000/LP6bpP72w7lKxYvOnn34Sm2++uTwo6jq49H/rrbeKRo0aiW222UaMHTtW/PLLL6qUEGKC+ZQYkXzrrbfk73v69Ony0ve1114rRxpPOeUUsf/++4stt9xSCkAsAbbOOuuIZs2aidatW4vRo0fLP3W4RJ7kMiZY5xdzOnHuxM17uNy+4447ihVWWEHGAHG78847SzEKgdqjRw8xcuRIuXLEww8/LN/74YcfykfkcR42IXWbf/7zn+Luu+9WFqlYsQkmT54sl0Oqq/8+cNkNa2DioN1tt93EtGnT+E+JeAHiBccb/oR9//33csF/jMxh5QasbACR8+6774o333xTvPrqq+LFF1+Uo+pz5syRI364Ee3BBx+Uixrfd9994t5775WjdrfddpsUS7iUjN8bRN/48ePlHyYsEYK16TDCN3DgQDkVpHfv3qKqqkp06dJFXn343//+J+/0Pu644+QI4wEHHCAn4O+6667yd4E7wTfeeGOx7rrrynlSK664olhppZXEhhtuKJch2W+//eTcynPOOUcKzerqavmZiOOOO+4QjzzyiBzZLCX44/jss8/KdkNb4UELaI+uXbuKSy65RJx88snikEMOEXvuuafYaqut5PJlyy+/vFh55ZXFmmuuKa9yYDoNRk0hXrHEGUZmcaNg8+bNxYknnij3/9JLLxXt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Rdpss83kIFUlQrFpgBX+sc6mPjDatWunSmp4+eWXxVZbbSUF6WqrrSZHNk855RTxxRdfKA9CCCGEECHFJQamoCn22msv8c0336iSyoJiM0LHjh2zYvPee+9VW2sYNWpUtnyjjTYSw4cPVyWEEEIIIWGwwg00Q4sWLdSWyoNiM8IzzzwjVl99dbHeeutZH1vZtGlTedBgzubkyZPVVkIIIYSQXMaOHSt1A24UqlQoNiP88ccfcjIv5mziAfom7777rpzgixFNzO8khBBCCCnEW2+9JdZdd11x5513qi2VR8WIzblz54rzzz9ffP311/LGn7/++kvefY6H43fu3Fncc8894sYbbxQDBgyQ2zfYYAM5twK+WKgd2y666CI54jl06FA5qglfjIRiQXfTF4+8xMGFeu++++6s78033ywOOOAA6YcEYQtfbWtfbaPe7777Lmsjvfnmm/Lfkbb79+8vnn322ZAP6j355JOz9l133SWXaMJrHPCYZ7rOOuvIhEdsYukmbbdv317up7bxTNdPP/00ayNhekGzZs2y9hFHHCEmTZoU8sGao9F6cYc/Xq+wwgpihx12yMb322+/yXmv2sYPEo/20va5554r59NqGwmjzt26dcvamISN9cxMH+zbqaeemrXvuOOObL2YQ4P5M6YvnvKgbdwxOG3atKyNNdKwHJa2kV599VW5MoG2sd6qLYbTTz89a992223yOMBr/GnB+qy6DHcu5vNFOvvss+UfIG0jvfLKK6Jnz55Zu7q6Wrz44oshH3wXZ5xxRta+9dZbxS233CJfR2Mo5It01llniR9//DFrI2Eec1VVVdbu1auXeOmll0I+P/30k1z2Q9s4fyDhdTQG+OJztI3P175IqAc+2kbC5+FztY14EJfpg7ij9WL/8DoaA9o56ovvQ9v4ntBW2kZCu6P9tY3vBceI6YNjCN+jtvH96noRwx577BHyxXGnbRyP0RhwfGkbCccfHrer7e7du1tjwG9K2/hN4HjH62gM+N1FffE70jZ+X/j9ahtJryuobfxOX3/99ZDPkiVLcurF7x6vozHA97zzzsvaU6ZMyfoi4dzx+++/Z22k5557TvTr1y9rd+3aVa4aYvrg3Pqf//wna+MciXMlXiOG3Xff3ckXCedbnHe1jYTzMs7P2sZ5G+dv0+fbb7+V53lt4/yPfgCvozHYfNFvaRt9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