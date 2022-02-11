简介
===
  JavaScript[官方教程](https://www.javascript.com/)太简洁了，本文参考菜鸟[JavaScript教程](https://www.runoob.com/js/js-tutorial.html)。

[对象]()
------
### [string对象](https://www.runoob.com/jsref/jsref-obj-string.html)
#### [charCodeAt](https://www.runoob.com/jsref/jsref-charcodeat.html)
```C++
string.charCodeAt(index);
```
  返回字符串指定位置的字符的Unicode编码（UTF-16），范围是0~65535。

```JavaScript
var str = "HELLO WORLD";
var n = str.charCodeAt(0); // 返回第一个字符的Unicode编码：72
var n = str.charCodeAt(str.length - 1); // 返回最后一个字符的Unicode编码：68
```

#### [fromCharCode](https://www.runoob.com/jsref/jsref-fromcharcode.html)
```C++
String.fromCharCode(n1, n2, ..., nX);
```
  将一个Unicode编码转换成一个字符，或将多个Unicode编码转换成一个字符串。

```JavaScript
var n = String.fromCharCode(72,69,76,76,79); // n = “HELLO"
```
