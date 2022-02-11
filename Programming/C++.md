简介
===
  [C++](https://www.cplusplus.com/)

[algorithm](https://www.cplusplus.com/reference/algorithm/)
------
#### std::[transform](https://www.cplusplus.com/reference/algorithm/transform/?kw=transform)
```C++
OutputIterator transform (InputIterator first1, InputIterator last1, OutputIterator result, UnaryOperation op);
```
  对first1（包括该元素）到last1（不包括该元素）范围内的输入数据依次执行op指定的一元操作（unary operation），结果输出到result开始的内存。
```C++
OutputIterator transform (InputIterator1 first1, InputIterator1 last1, InputIterator2 first2, OutputIterator result, BinaryOperation binary_op);
```
  对first1（包括该元素）到last1（不包括该元素）范围内的输入数据和first2开始的输入数据（元素个数和前面相同）依次执行binary_op指定的二元操作（binary operation），结果输出到result开始的内存。

[string](https://www.cplusplus.com/reference/string/string/)
------
#### string::[begin](https://www.cplusplus.com/reference/string/string/begin/)
```C++
iterator begin();
```
  返回指向字符串第一个字符的迭代器。

#### string::[end](https://www.cplusplus.com/reference/string/string/end/)
```C++
iterator end();
```
  返回一个指向字符串结束符的迭代器，当字符串为空时，返回值与string::begin相同。

#### string::[find_first_not_of](https://www.cplusplus.com/reference/string/string/find_first_not_of/)
  查找string字符串中第一个与给定字符集中任意一个都不匹配的字符，并返回其位置，如果没找到则返回string::npos。pos指定查找string字符串的起始位置，如果pos为0，表示查找整个字符串；如果pos大于字符串的长度，则永远无法匹配。
```C++
size_t find_first_not_of (const string& str, size_t pos = 0) const;
```
  用string字符串str指定待查找的字符集。

```C++
size_t find_first_not_of (const char* s, size_t pos = 0) const;
```
  用字符数组指定待查找的字符集，以空字符（null）结束，长度为第一个空字符出现的位置。

```C++
size_t find_first_not_of (const char* s, size_t pos, size_t n) const;
```
  用字符数组指定待查找的字符集，范围是前n个字符。

```C++
size_t find_first_not_of (char c, size_t pos = 0) const;
```
  指定待查找的字符。
#### string::[substr](https://www.cplusplus.com/reference/string/string/substr/)
```C++
string substr (size_t pos = 0, size_t len = npos) const;
```
  返回string字符串中从pos开始，长度为len的子字符串。如果pos等于字符串的长度，则返回一个空字符串；如果pos大于字符串的长度，则抛出out_of_range错误。如果未指定len，默认值为npos，表示直到字符串结束；如果len大于字符串的长度，只取有效个数的字符。

### 成员常数
#### string::[npos](https://www.cplusplus.com/reference/string/string/npos/)
```C++
static const size_t npos = -1;
```
  由于size_t表示无符号整形数据，-1即0xFFFFFFFF，表示该类型的最大值。
  在string成员函数中用作长度参数时表示“直到字符串结束”，用作返回值时表示没有匹配到。