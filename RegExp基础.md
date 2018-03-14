---
title: RegExp基础
date: 2017-10-13 20:34:01
tags: RegExp
---

## 前言

正则表达式(Regular Expression)是使用一个字符串来匹配一系列符合某个语法规则的字符串。**简单来说就是用某种模式或者规则去匹配文本**。

正则表达式的应用十分的广泛，可以是简单的邮箱、电话的匹配，还有现在IDE中也支持了正则使得查找替换之类的功能更加灵活可操作，当然还有超级多我根本不知道的用途，所以我也要来学习一下正则了。

## RegExp对象

> /pattern/flags  
>
> new RegExp(pattern [, flags])

可以用字面量和构造函数的方法来实例化一个正则表达式对象。

- 字面量

  ```javascript
  var reg = /\d/g;
  console.log(reg);  // /\d/g
  ```

- 构造函数

  ```javascript
  var reg = new RegExp('\\d', 'g');
  console.log(reg);  // /\d/g
  ```

上面两种方式创建的正则表达式是一致的，都是用来**全局**(global)地匹配一个数字(\d)。

一般来说更多的还是使用字面的形式，因为更为直观和方便。

### 正则表达式的标志

常用的三个标志：

- **i** : 忽略大小写

  ```javascript
  /mi/.test('Mi'); //false
  /mi/i.test('Mi'); //true
  ```

- **g** : 全局匹配，即找到第一项匹配的字符不会停止还会继续往下匹配

  ```javascript
  'apple is red, grape is purple'.replace(/is/, 'MM');
  //"apple MM red, grape is purple"
  'apple is red, grape is purple'.replace(/is/g, 'MM');
  //"apple MM red, grape MM purple"
  ```

- **m** : 多行模式，可以简单理解为每一行都是一个新的开始

  ```javascript
  var me = 'hello,\nmy name is mxk';
  me;
  //"Aello,
  //Ay name is mxk"
  me.replace(/^\w/, 'A');
  //"Aello,
  //my name is mxk"
  me.replace(/^\w/mg, 'A');
  //"Aello,
  //Ay name is mxk"
  //这里注意我不但加了个m标志还加个g标志，因为我们要全局匹配没有g的话即使设置了m也只能匹配到第一个就结束了
  ```

### 正则表达式中与标志相关的属性

因为上面提到三个标志，那么就来提提正则表达式中几个与之相对应的属性用来反应标志的设置与否

> - **`ignoreCase`** :  设置了i标志访问该属性返回true
> - **`global`** : 设置了g标志访问改属性返回true
> - **`multiline`** : 设置了m标志访问该属性返回true

```javascript
var reg = /aoteman/gi;

reg.ignoreCase; // true
reg.global; // true
reg.multiline; // false
```

**注意: 这个三个属性都是只读的, 并不能通过修改这些属性的值来更改标志**

```javascript
reg.ignoreCase = false;
reg.ignoreCase; // true
```

## 匹配规则

在正则表达式中大部分的字符表达含义都可以从它的字面中看出来，如b就是表示一个字符b，abc就是表示字符abc，这类字符称为**字面量**。然而除此之外还有一部分有特殊含义的符号，如果\b就不是简单的匹配一个反斜杠加上字符b，它表示的单词边界，这一类符号称之为“**元字符**”。

字面量的表示很简单，我们着重要学习的是元字符，以下字符都有特殊含义。

>  **\* + ? $ ^ . | \ ( ) { } [ ]**

> - `\t` 水平制表符
> - `\v` 垂直制表符
> - `\n` 换行符
> - `\r` 回车符
> - `\0` 空字符
> - `\f` 换页符
> - `\cX` 与X对应的控制字符(ctrl + X)

### 字符集合

> - 用元字符`[]`来构建一个字符集合,匹配其中的**一个字符**
> - `-`可以指定范围
> - `^`用来取反

```javascript
/[abcdefg123]/; //可以匹配abcdefg123中的任意一个字符
/[a-g1-3]/; //等价/[abcdefg123]/，而且更为简洁
/[^abcdefg123]/; //则表示匹配不为abcdefg123中的任意其他字符
```

### 预定义类

> - `.` 匹配除任意单个字符, 相当于`[^\r\n]`
> - `\d` 匹配0-9之间的任一数字，相当于`[0-9]`
> - `\D` 匹配所有0-9以外的字符，相当于`[^0-9]`
> - `\w` 匹配任意的字母、数字和下划线，相当于`[A-Za-z0-9_]`
> - `\W` 除所有字母、数字和下划线以外的字符，相当于`[^A-Za-z0-9_]`
> - `\s` 匹配空格（包括制表符、空格符、断行符等），相等于`[\t\r\n\v\f]`
> - `\S` 匹配非空格的字符，相当于`[^\t\r\n\v\f]`

### 边界

> - `^`匹配开头
> - `&`匹配结尾
> - `\b`单词边界
> - `\B`非单词边界

### 量词

> - `{}`表示匹配的次数, `{n}`匹配n次, `{n, m}`出现n到m次。
> - `?` 表示出现0或者1次，等价于`{0,1}`
> - `*`表示出现0次或者多次，等价于`{0,}`
> - `+`表示出现1次或者多次，等价于`{1,}`
> - `|`表示或,`/red|green/`匹配red或者green

### 贪婪模式和非贪婪模式

在默认情况下正则表达式的匹配在符合条件的情况会往多的匹配，如`/\d{3,5}/.exec('12345')` ,`123`,`1234`,`12345`均符合规则，但是它不会匹配完`123`就停止，它还会继续往下搜索直到不符合条件为止，这种被称为贪婪模式。

那么怎么把它改成非贪婪模式呢，很简单只要再量词后面加上一个问号即可

```javascript
/\d{3,5}?/.exec('12345'); //非贪婪模式["123", index: 0, input: "12345"]
/a+/.exec('aaabbb');  //贪婪模式["aaa", index: 0, input: "aaabbb"]
/a+?/.exec('aaabbb'); //非贪婪模式["a", index: 0, input: "aaabbb"]
```

### 分组

#### 捕获组

```javascript
/\w\d{3}/.test('a1b2c3'); //false
/(\w\d){3}/.test('a1b2c3') //true
```

从上面的两行代码应该能够看出来，如果不加`()`那么量词对应的只是它前面那单个字符，所以`/w\d{3}/`匹配的是一个word加上三个数字。而如果加了`()`那就相当于对整体进行了分组，那么它就是一个整体，`{3}`量词对象就是那个整体。

```javascript
var date = '1996/09/04';
date.replace(/(\d{4})\/(\d{2})\/(\d{2})/, '$3-$2-$1'); // "04-09-1996"
```

另外在用`()`的同时他还会进行**捕获**, 第一个圆括号里的内容就可以用`$1`表示，第二个用`$2` 。上面的代码中我用三个分组进行匹配并将内容捕获，然后再排列`$1`,`$2`,`$3`的顺序将原字符串更改。

#### 非捕获组

当我们仅仅只是为了将内容进行分组而不需要捕获其内容，那么可以用`(?:)`来操作。

```javascript
'1996/09/04'.match(/(\d{4})\/(\d{2})\/(\d{2})/); 
//["1996/09/04", "1996", "09", "04", index: 0, input: "1996/09/04"]
'1996/09/04'.match(/(?:\d{4})\/(\d{2})\/(?:\d{2})/); 
//["1996/09/04", "09", index: 0, input: "1996/09/04"]
```

从上面的第二行代码中的结果中可以看出来只有`09`被捕获了。

#### 先行断言

`x(?=y)`称为先行断言，就是匹配x的时候还要判断它后面是不是y如果是的就成功匹配，但是要注意这个y并不会被选中。

```javascript
'i am 22 years old'.match(/\d{2}(?=\syears)/);
//["22", index: 5, input: "i am 22 years old"]
```

#### 先行否定断言

`x(?!y)`称为先行否定断言，意思是x的后面得不是y这样x才能被匹配到，同样的在先行否定断言中括号中的内容不会被返回。

```javascript
/a(?!b)/.test('abc'); // false
'acd'.match(/a(?!b)/); //["a", index: 0, input: "acd"]
```

## 正则表达式的其他属性

> - **`lastIndex`** 是一个可读可写的整型属性，用来指定下一次匹配的起始索引
> - **`source`** 只返回正则表达式文本的字符串的形式，不含斜杠和标志，只读

```javascript
var str = 'a1b2c3d'
var reg = /[a-z]/g;
console.log(reg.lastIndex) //0
var rt;
while (rt = reg.exec(str)) {
	console.log('lastIndex: ' + reg.lastIndex + '\n' + 'result: ' + rt.toString());
}
/*lastIndex: 1
**result: a
**lastIndex: 3
**result: b
**lastIndex: 5
**result: c
**lastIndex: 7
**result: d*/

console.log(reg.lastIndex, rt); //0, null
```

分析下这段代码，首先正则表达式的`lastIndex`属性是0表示从索引0开始匹配，匹配到`a`，此时`lastIndex`被设置为最近一次成功匹配的下一个位置，它的索引是1，再进行匹配，匹配到`b`，它的下个位置索引是3,以此类推。到最后匹配到`d`,它的索引是6，因此`lastIndex`被设置为7。因为字符串后面已经没得可以匹配了所以结果为null, lastIndex被重置为了0。

> **注意：只有正则表达式使用了全局检索的‘g’标志是，该属性才会起作用。**

```javascript
/apple/gi.source; //"apple"
/\w\d(red|green){3,4}/m.source; //"\w\d(red|green){3,4}"
var reg = /abc/g;
reg.source; // "abc"
reg.source = '1243';
reg.source; // "abc" 
```

**因此本文中介绍的五个属性中只有lastIndex是可写的。**

## 正则表达式的方法

### RegExp.prototype.test()

> **`test()`**方法执行一个检索，用来查看正则表达式与指定的字符串是否匹配。返回 `true` 或 `false`。 

语法： `regObj.test(str)`

这个方法和`String.prototype.search()`很像，都是用来检验嫩能否在字符串中匹配到内容。

**注意：**当使用**全局匹配**时候多次调用有可能会返回不同的结果，因此每次匹配成功正则表达式的lastIndex都在变化, 所以每次调用都是在越过之前的匹配开始的。

```javascript
var str = 'abc';
var reg = /\w/g;
reg.test(str) //true
reg.test(str) //true
reg.test(str) //true
reg.test(str) //false

/\w/g.test(str) //如果是这样的话那就一直是true了,因为每次都是新的正则了，这样太耗内存了
```

但是既然只是为了检查能够匹配到，那设置全局匹配也就没有意义了。

### RegExp.prototype.exec()

> **`exec()`** 方法在一个指定字符串中执行一个搜索匹配。返回一个结果数组或 `null`

语法：`regObj.exec(str)`

匹配成功的话返回一个数组，一般在数组中显示的有三项

- 匹配到的结果，以字符串返回
- `index`，匹配的第一个字符的索引
- `input`, 原字符串

在含有捕获分组的情况下，在结果和index中间还有会捕获结果也是以字符串返回。

**匹配到的结果和捕获结果才是数组的内容**，index, input, length都是数组的属性。

```javascript
var str = 'abc';
var reg = /\w/g;
var rt = reg.exec(str);
Object.prototype.toString.call(rt) //"[object Array]"
rt; //["a", index: 0, input: "abc"]
rt.length; //1 
reg.exec(str); //["b", index: 1, input: "abc"]
reg.exec(str); //["c", index: 2, input: "abc"]
reg.exec(str); //null
reg.exec(str); //["a", index: 0, input: "abc"]

var reg2 = /(\w)/g;
var rt2 = reg2.exec(str);
rt //["a", "a", index: 0, input: "abc"]
rt.length; //2

reg2.exec(str); //["b", "b", index: 1, input: "abc"]
reg2.exec(str); //["c", "c", index: 2, input: "abc"]
```

我们注意到无论是`test()`方法还是`exec()`在多次调用的情况下会产生不同的结果，不过我们得清楚这是**全局匹配**的情况才会有的，因为这些都是因为正则表达式`lastIndex`的变化而来的。

## 字符串的方法

### String.prototype.search()

> `search()` 方法执行正则表达式和 `String`对象之间的一个搜索匹配。

语法：`str.search(regexp)`

如果传入的参数不是正则表达式，则会使用`new RexExp(obj)`将其隐性转为正则表达式对象。

匹配成功，返回首次匹配项的第一个字符的索引，无关全局检索

匹配失败，返回-1

```javascript
var str = '1abc';
var reg2 = /[a-z]/g;
str.search(reg2) //1
str.search(reg2) //1
str.search(reg2) //1

'1234'.search(3) //2
```

### String.prototype.match()

> 当一个字符串与一个正则表达式匹配时， **match()**方法检索匹配项。

语法：`str.match(regexp)`

如果传入的参数不是正则表达式，则会使用`new RexExp(obj)`将其隐性转为正则表达式对象。未提供任何参数，直接使用`match()`, 那么你会得到一个包含空字符串的 `Array`：[""] 

- 正则表达式没有设置g标志, `str.match(regexp)`和`regObj.exec(str)`结果一样
- 设置了`g`标志了，返回一个数组, 包含所有的匹配项，没有`input`和`index`属性
- 设置了`g`标志，正则表达式即使有分组，捕获组也不会被返回还是只有所有匹配项

```javascript
var str = 'a1b2c3d4'
var reg = /[a-z]\d/
str.match(reg) //(2) ["a1", "a1", index: 0, input: "a1b2c3d4"]
reg.exec(str) //(2) ["a1", "a1", index: 0, input: "a1b2c3d4"]

var reg2 = /[a-z]\d/g
str.match(reg2) //(4) ["a1", "b2", "c3", "d4"]

var reg3 = /[a-z](\d)/g
str.match(reg3) //(4) ["a1", "b2", "c3", "d4"]
```

### String.prototype.replace()

> **replace() **方法返回一个由替换值替换一些或所有匹配的模式后的新字符串。模式可以是一个字符串或者一个正则表达式, 替换值可以是一个字符串或者一个每次匹配都要调用的函数。

语法：`str.replace(regexp|substr, newSubStr|function)`

- 第一个参数会被第二个参数的返回值替换
- 第一个参数是字符串，则仅仅是第一个匹配会被替换

**该方法返回的是一个新的替换后的字符串不改变原字符串。**

`function`会在每次匹配替换的时候调用,**替换的是函数的返回值**，有四个参数

1. 匹配字符串
2. 正则表达式分组内容, 没有分组则没有该参数
3. 匹配项在字符串中的index
4. 原字符串input(or origin)

```javascript
'a1b2c3d4e5'.replace(/\d/g, function(match, index, origin) {
	console.log(index);
	return parseInt(match) + 1;
});
/*1
**3
**5
**7
**9
**"a2b3c4d5e6"*/

'a1b2c3d4e5'.replace(/(\d)(\w)(\d)/g, function(match, group1, group2, group3, index, origin) {
  console.log(match);
  return group1 + group2;
});
/*1b2
**3d4
**"a1bc3de5"*/
```

## 参考链接

- [阮一峰Javascript标准参考教程——标准库RegExp对象](http://javascript.ruanyifeng.com/stdlib/regexp.html)
- [MDN JavaScript标准库 RegExp对象](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/RegExp)
- [慕课网 JavaScript正则表达式](http://www.imooc.com/learn/706)

