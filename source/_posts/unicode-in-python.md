---
title: python语言中的字符编码问题
entitle: unicode-in-python
categories:
  - coding
tags:
  - python
author: 赵小生
authorAbout: 'https://github.com/zhmhhu'
avatar: /images/userpic.jpg
authorLink: 'https://github.com/zhmhhu'
authorDesc: 不会讲故事的程序员不是好的水利工程师
photos: https://ws2.sinaimg.cn/large/006tNbRwgy1fu4ys17t69j30f007qdfx.jpg
date: 2018-08-09 15:53:49
keywords:
description: 在具体的编程语言中，字符编码会存在具体的问题，比如，Python中的编码问题一度让开发者非常头疼。
---

上一篇文章讲述了计算机中的字符编码问题，在具体的编程语言中，字符编码会存在具体的问题，比如，Python 中的编码问题一度让开发者非常头疼。

## python 的默认编码
Python 的默认编码是 ASCII，一直到 python2.X 都是如此，如果开发者不指定具体的字符编码方式，那么默认就是 ASCII。为什么不使用更好的 Unicode？这一点，跟 python 的诞生背景有关。

Python 的诞生时间是 1989 年，Unicode 于 1994 年才正式公布，在Python 诞生之初并无 Unicode 可用，只能选择 ASCII。后来为了适用于非英语系的用户，做了多方改进。

如果不做修改，Python将使用 ASCII 为所有代码编码，包括注释。
```
>>> import sys
>>> sys.getdefaultencoding()
'ascii'
```
在编写 python 代码时如果不指定文件的编码方式，将默认使用 ASCII 编码。所以如果在代码中出现中文（包括注释），将会报错。
```
#stringtest.py

print '你好'

C:\Python27\python.exe D:/MyGit/demo/test/test.py
File "D:/MyGit/demo/test/test.py", line 1
SyntaxError: Non-ASCII character '\xe4' in file D:/MyGit/demo/test/test.py on line 1, but no encoding declared; see http://python.org/dev/peps/pep-0263/ for details
```
 

如果想在代码中使用中文，则一定要在代码开头（第一行或第二行）声明此文件的编码方式，比如编码方式设为 UTF-8
```
# -*- coding: utf-8 -*-
```
或者
```
#!/usr/bin/python
# -*- coding: utf-8 -*-
```
其中第一行注释表示这是一个可以在 Unix/Linux/Mac 直接运行的程序，Windows 系统会忽略这个注释。

这样，在代码中就可以使用中文了。

## python中的编码解码

### str和unicode

python 语言中有两个表示字符串的对象，str，unicode。他们都是basestring 的子类，可见 str 与 unicode 是两种不同类型的字符串对象。

同样都是字符串对象，为什么 Python 要搞两套的。这其实都是历史遗留问题。

Python 的创始人 Guido van Rossum（人称龟叔）在开发初期并未预料到 Python 能发展成一个全球流行的语言，因此他一直都没有把支持全球各国语言当做重要的事情来做，所以就轻佻的把ASCII当做了默认编码。并且，他为了处理起来简单，居然把字节和字符串当做同一种东西来对待，即 `bytes == str`，任何字符串默认按照 ASCII 编码后以字节形式存储在计算机中。这可要了命了。

当后来大家对支持汉字、日文、法语等语言的呼声越来越高时，Python 于是准备引入 Unicode，但若直接把默认编码改成 Unicode 的话很不现实， 因为很多软件就是基于之前的默认编码 ASCII 开发的，编码一换，那些软件的编码就都乱了。所以 Python 2 就直接 搞了一个新的字符类型，就叫 Unicode类型。于是，就有了 str 和 Unicode 并存的局面。

str 是普通字符串（存储时其实就是字节串），使用默认方式编码，使用时一般需要解码成 unicode。Unicode 是 Unicode 字符串，默认使用 Unicode方式编码，使用时可以解码成需要的格式。

这里提一下 python 开发中非常重要的两处设置。

一个是编解码器的默认设置defaultencoding
```
>>> import sys
>>> sys.getdefaultencoding()
'ascii'
```
另一个是声明在python文件头部的代码编码方式coding
```
# -*- coding: utf-8 -*-
```
这两处设置在python的 str，unicode对象的encode和decode方法中，有非常重要的作用，直接影响到结果。**下面的代码按照目前的设置进行，即defaultencoding为ascii，coding为utf-8**

使用str()方法创建字符串。
```
s = str('你们')  #存储时按utf-8存储
print repr(s)   #打印结果为'\xe4\xbd\xa0\xe4\xbb\xac'
```
使用unicode()方法创建的是unicode字符串，也可使用 u 标记。创建unicode字符串时，需指定字符串解码方式，否则，在显示时，按defaultencoding对字符串进行解码。
```
u1 = u'你们'
u2 = unicode('你们')  
print u1  #输出正确
print u2  #存储时按utf-8存储，再按ascii解码，出错
#UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128)
```
```
u3 = unicode('你们','utf-8')  #先按utf-8编码存储，再按utf-8解码
print u3 #输出正确
print repr(u3) #打印结果是 u'\u4f60\u4eec'
```
```
u4 = unicode('你们','gbk')  #先按utf-8编码存储，再按gbk解码
print u4 #打印结果是 浣犱滑
print repr(u4) #打印结果是 u'\u6d63\u72b1\u6ed1' 是 浣犱滑 三个字的unicode编码
```
出现上述结果的原因是，字符串“你们”，按照 uft-8 编码存储时，在内存中，是`'\xe4\xbd\xa0\xe4\xbb\xac'`,这一点通过第一个例子可以看到。在使用gbk解码时，其中 `e4bd` 对应的是字符“浣”，`a0e4` 对应的是字符“犱”，`bbac` 对应的是字符“滑”。于是，解码之后的结果变成了“浣犱滑”。

总之，任何字符串在存储是按照用户设定的 coding 值选择对应的编码方式进行存储，unicode 字符串按照 unicode 方式存储，在解码时如不指定解码方式，将按照 defaultencoding 对字符串进行解码。

### decode()和encode()

str.decode() 用于将字符串解码成指定的格式，如果不指定解码方式，将使用默认的ascii进行解码。

unicode.encode() 用于将标准的 unicode 编码成指定的格式，如果不指定编码方式，将使用默认的 defaultencoding 进行编码。

这里的解码可以理解为把二进制字节码码转换成可以显示的字符串，编码可以理解为把字符串按照约定的方式转换成二进制字节码。

看下面两个例子
```
s = str('你们')
print s.decode('utf-8') #输出正确
print s.decode() #出错
#UnicodeDecodeError: 'ascii' codec can't decode byte 0xe4 in position 0: ordinal not in range(128)
```

```
u=u'你们'
print u.encode('utf-8')  #输出正确
print u.encode('gbk')  #输出乱码，编码时为utf-8,解码时为gbk
print u.encode() #出错，编码时为utf-8,解码时为ascii
#UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
```
总结，str 字符串存储和 unicode 字符串存储时，按照 coding 设置编码，str.decode() 把本身解码成指定格式，unicode.encode()把本身编码成指定格式，如果不指定编码格式，将使用 defaultencoding 进行编码。如果编码解码方式不一致，将会出错或乱码。

另有str.encode() ，unicode.decode()，这两个方法没什么用。因为str是已经编码了的字符串，无需再次编码。unicode 本身已经解码成 unicode了，无需再次解码。但 python的就是这么好玩，为了保持对称，设计了这两个方法。

官方文档如此描述：str.encode(e) is the same as unicode(str).encode(e). This is useful since code that expects Unicode strings should also work when it is passed ASCII-encoded 8-bit strings(from Guido van Rossum) .

这段话大概意思是说 encode 方法本来是被 unicode 调用的，但如果不小心被作为 str 对象的方法调用时，并且这个 str 对象正好是 ASCII 编码的（ASCII 这一段和 unicode 是一样的），也应该让他成功。这就是str.encode 方法的一个用处。

类似地，把光用 ASCII 组成的 unicode 再 decode 是一样的道理，因为好像几乎任何编码里 ASCII 都没变。因此这样的操作等于没做。 这些方法没什么用，实际使用中往往会出错，比如下面两个。
```
str.encode() ，首先使用默认的编码方式将str进行编码，然后使用指定的方式将对象解码

s.encode("utf-8") #等价于 s.decode(defaultencoding).encode("utf-8")

unicode.decode()，首先使用默认的编码方式将对象进行解码，再用指定的方式将对象编码

u.decode("utf-8") #等价于 u.encode(defaultencoding).decode("utf-8")
```
### 关于requests库

requests 库在写网络爬虫的时候经常用到，使用起来非常方便。

Request 对象在访问服务器后会返回一个 Response 对象，这个对象将返回的Http 响应字节码保存到 content 属性中。content 本身是未编码的二进制数据，如果是文本文件，则自动按 str 的默认方式编码。

Response 对象还有一个属性 text，它是一个 unicode 字符串对象。如果不加处理直接访问，常常会发生乱码。因为 Response 对象会通过另一个属性 encoding 来将 content 字节码编码成 unicode，而这个 encoding 属性居然是 responses 自己猜出来的。

官方文档：

text
Content of the response, in unicode.

If Response.encoding is None, encoding will be guessed using chardet.

The encoding of the response content is determined based solely on HTTP headers, following RFC 2616 to the letter. If you can take advantage of non-HTTP knowledge to make a better guess at the encoding, you should set r.encoding appropriately before accessing this property.

responses 根据 http 头文件获取 encoding 设置，大多数情况是正确的，但有时也有例外。所以要么你使用 content 然后重新解码，要么把 encoding 设置正确。
```
# -*- coding: UTF-8 -*-
import requests
url = "http://xxx.xxx.xxx" #gbk格式编码的网页
response = requests.get(url)
response.encoding = 'gbk'
```
结果如下
```
print response.content   #未编码的二进制数据，如果是文本文件，则自动按str的默认方式解码,此处按 ASCII 解码，结果显示乱码,
print response.content.decode('gbk') #按“gbk”解码之后，显示正确
print response.text #显示正确
```

在 python 语言中，存储在磁盘中的二进制文本，以字符串的形式显示时，一定要指定其编码方式，默认的编码方式就是 defaultencoding 所对应的值，为了方便起见，一开始就把 defaultencoding 设置为 utf-8 形式
```
import sys
reload(sys)
sys.setdefaultencoding('utf8')
```
以免使用默认的编码方式，造成中文乱码。
