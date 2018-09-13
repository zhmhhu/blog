---
title: javascript语言中的字符编码
entitle: unicode-in-javascript.md
categories:
  - coding
tags:
  - javascript
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws3.sinaimg.cn/large/006tNbRwgy1fuk12apcidj30jg0apabm.jpg
date: 2018-08-23 21:29:21
keywords:
description: JavaScript 在 1995 年诞生的时候，Unicode 已经发布。因此 JavaScript 的作者 Brendan Eich 在设计之初就使用 Unicode 作为 JavaScript 的唯一编码方式。
---

JavaScript 在 1995 年诞生的时候，Unicode 已经发布。因此 JavaScript 的作者 Brendan Eich 在设计之初就使用 Unicode 作为 JavaScript 的唯一编码方式。

事实上，ECMAScript3 要求 JavaScript 必须支持 Unicode2.1 及后续版本，ECMAScript5 则要求支持 Unicode3 及后续版本。所以，我们编写出来的 JavaScript 程序，都是使用 Unicode 字符集编码的。


## JavaScript中的 UTF-16 编码

我们知道 Unicode 是一个字符集，具体的编码方式有 UTF-8,UTF-16,UTF-32 等等。
JavaScript 语言采用 Unicode 字符集，但是只支持一种编码方法，就是UTF-16。所有 JavaScript 代码在计算机中都是以 UTF-16 的 2 或 4 字节方式存储。
```
console.log('\u0062'); // 'b'
console.log('\u62'); // 出错
```
2字节：从0x0 - 0xFFFF的码段(BMP)，编码后的数值和 unicode 对应的码点一致

4字节(两个双字节)：从0x10000 - 0x10FFFF的码点(SP，已经超过了BMP平面)，会根据规则，编码成一对 16 bit 长的码元：如 0x10437 码点会编码成 D801 DC37，它们叫做**代理对(surrogate pair)**

### 4 字节代理对的原理

从上面 D801 DC37 的例子可以发现，这两个码点都落在了 BMP 平面的码点范围之内，**并且都属于U+D800到U+DFFF码段**。没错，就是通过一系列规则，把超出 BMP 平面的码点(U+10437)转换成两个属于 BMP 平面的码点—— U+D800 到 U+DFFF 码段之间(U+D801和U+DC37)。


SMP 平面中的码点范围是从 U+10000 到 U+10FFFF，共计 FFFFF 个，即 2^20=1,048,576 个，需要 20 位来表示。

如果用两个双字节长的码点组成的序列来表示，第一个码点（称为高位代理）要容纳上述 20 位的前 10 位，第二个码点（称为低位代理）容纳上述 20 位的后 10 位。前后分别需要 2^10=1024 个码点来代理。而 BMP 平面的U+D800 到 U+DFFF 码段正好有 2048 个码点，足以满足高位代理与低位代理的需要。

因此需要将 U+D800 - U+DFFF 分为两段

一段为高位代理初始值 U+D800：U+D800 到 U+DBFF 之间的保留码点用于高位代理（leading surrogates）

一段为低位代理初始值 U+DC00：U+DC00 到 U+DFFF 之间的保留码点则用于低位代理（trailing surrogates）

两段之间间隔 2^10=1024，刚好各自能够满足前后10位

例如 U+10437 编码
0x10437<u>减去0x10000</u>，结果为 0x00437，二进制为 0000 0000 0100 0011 0111

分区它的上 10 位值和下 10 位值（使用二进制）:0000000001 and 0000110111

添加 0xD800 到上值，以形成高位：0xD800 + 0x0001 = 0xD801。

添加 0xDC00 到下值，以形成低位：0xDC00 + 0x0037 = 0xDC37。

得出它的 UTF-16 编码为D801 DC37


## Unicode 码点和字符串的转换

JavaScript 的 String 提供了Unicode码点和字符串的转换方法。 

### String.fromCharCode()

fromCharCode() 是 String 对象提供的静态方法（即定义在对象本身，而不是定义在对象实例的方法。该方法的参数是一个或多个数值，代表 Unicode 码点，返回值是这些码点组成的字符串。
```
String.fromCharCode() // ""
String.fromCharCode(97) // "a"
String.fromCharCode(104, 101, 108, 108, 111)
// "hello"
```
上面代码中，String.fromCharCode 方法的参数为空，就返回空字符串；否则，返回参数对应的 Unicode 字符串。

注意，该方法不支持 Unicode 码点大于 0xFFFF （即超出基本平面）的字符，即传入的参数不能大于 0xFFFF（即十进制的 65535）。
```
String.fromCharCode(0x20BB7)
// "ஷ"
String.fromCharCode(0x20BB7) === String.fromCharCode(0x0BB7)
// true
```
上面代码中，String.fromCharCode 参数 0x20BB7 大于 0xFFFF，导致返回结果出错。0x20BB7 对应的字符是汉字𠮷，但是返回结果却是另一个字符（码点 0x0BB7）。这是因为 String.fromCharCode 发现参数值大于0xFFFF，就会忽略多出的位（即忽略 0x20BB7 里面的 2）。

这种现象的根本原因在于，码点大于 0xFFFF 的字符占用四个字节，而 JavaScript 默认支持两个字节的字符。这种情况下，必须把 0x20BB7 拆成两个字符表示。
```
String.fromCharCode(0xD842, 0xDFB7)
// "𠮷"
```
上面代码中，0x20BB7 拆成两个字符 0xD842 和 0xDFB7（即两个两字节字符，合成一个四字节字符），就能得到正确的结果。码点大于 0xFFFF的字符的四字节表示法，由 UTF-16 编码方法决定。

### String.prototype.charCodeAt()

charCodeAt 方法返回字符串指定位置的 **十进制表示** Unicode 码点，相当于String.fromCharCode() 的逆操作。
```
'abc'.charCodeAt(1) // 98
```
上面代码中，abc 的 1 号位置的字符是 b，它的 Unicode 码点是 98。

如果没有任何参数，charCodeAt 返回首字符的 Unicode 码点。
```
'abc'.charCodeAt() // 97
```
如果参数为负数，或大于等于字符串的长度，charCodeAt 返回 NaN。
```
'abc'.charCodeAt(-1) // NaN
'abc'.charCodeAt(4) // NaN
```
注意，charCodeAt 方法返回的 Unicode 码点不会大于 65536（0xFFFF），也就是说，只返回两个字节的字符的码点。如果遇到码点大于 65536 的字符（四个字节的字符），必需连续使用两次 charCodeAt，不仅读入charCodeAt(i)，还要读入 charCodeAt(i+1)，将两个值放在一起，才能得到准确的字符。


## 字符串长度
读到上面那句时，你会发现，JavaScript 有可能会把原本只是一个的字符，识别成其长度是2！

没错，你说对了！

JavaScript 引擎会把所有源码当做是一连串的 UTF16 码元，也就是内部是以 UTF-16 进行编码的。
```
var f\u006F\u006F = 123;
console.log(foo); // 123
console.log(\u0066\u006F\u006F); // 123
var foo = '12\u0033'; // 123

// 中文
var 腾 = '123';
console.log(\u817e); // 123

// 4字节字符
var bar = '𠮷';
console.log('\uD842\uDFB7'); // '𠮷'
```

上面的例子可以看到，无论是字符串还是变量，无论是 BMP 还是 SMP上的字符，都可以使用 UTF-16 码元来表示。

在 ES6 中可以直接使用大括号包裹码点。

```
'\u{20BB7}' === '\uD842\uDFB7' // 竟然全等
```
但实际上只是语法糖，而这个语法糖很赞，ES6 内部对大括号内的码点进行了 UTF-16 编码，不需要自己换算成代理对。

其实了解了上面的知识以后，对于字符串的 length 就不难理解了。

对于 JavaScript 引擎来说，所有的字符串都是一系列的 UTF-16 码元，**length 指的是码元的个数**（也可以理解为两个字节等于 1 个 length），而不是字符个数。当某个字符是 4 个字节的 UTF-16 编码时，这时一个字符的 length 就为 2。但是中文的 length 却始终为 1，这是因为中文的码点范围 U+4E00 - U+9FA5，都在 BMP 平面内，UTF-16 编码只需要 2 个字节。

来看例子
```
// 还是拿这个字
'𠮷' === '\uD842\uDFB7';
'\uD842\uDFB7' === '\u{20BB7}';
var foo = '𠮷';
var bar = '\uD842\uDFB7';
var baz = '\u{20BB7}';
console.log(foo.length, bar.length, baz.length); // 2 2 2
```
可以看出， 4 个字节 UTF-16 编码的字符串的 length 是 2。

而我们需要获取“正确”的 length 值该怎么办？

ES6 轻松解决：Array.from(str).length

如果不是 ES6，那也有办法，只要识别两个相邻的码元是否形成代理对的关系，是的话把它们视为一个整体。

```
function getRealLen(str) {
    var reg = /[\ud800-\udbff][\udc00-\udfff]/g; // /[高位代理][低位代理]/g
    return str.replace(reg, 'i').length;
}

getRealLen('𠮷'); // 1
getRealLen('赵钱孙李'); // 4
```
这样就可以正确识别字符串的 length 了。
