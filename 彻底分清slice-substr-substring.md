---
title: '彻底分清slice(),substr(),substring()'
date: 2017-10-24 19:38:26
tags:
---

## 前言

打开笔记本突然看到便签上写着"`substr()`,`substring()`和`slice()`之间有什么区别"，这是我暑假时候刚开始学习`JavaScript`时候记录的一个问题，因为笔记啥的总是记得十分随意，又不整理，几个月了这个问题无意中才又被翻了出来。这个问题我是知道的，当时我也在一个中午查看`W3C`和`MDN`中对这个三个函数的详细介绍，自己也用了很多遍。学习了这么长一段时间总算有点长进了吧，我想。

好，那就在脑子里自我陈述一遍吧，这个三个函数的功能都是**对字符串进行子字符串的截取，`slice`既可以对字符串进行操作也可以对数组进行操作**，因为它既定义在数组的原型上也定义在字符串的原型上。他们的头个参数都是首索引，slice的第二个参数是末尾位置的索引。`substr`和`substring`有一个函数的第二个参数是长度一个末尾位置的索引，至于到底哪个是哪个我已经不敢确定了。还有我记得当时这三个函数在参数的选取上不同结果会有大的出路，这些我也都只是记个大概……

果然还是不行，既然这样那就好记性不如烂笔头好好来整理一番吧。

## `String.prototype.substr()`

> `str.substr(start[,  length])`

`start` 是首个字符的索引位置

- 为负数，start = `str.length` + start，倒数第|start|位。当|start| > `str.length`时，start = 0
- 为非负数, **当 start >= `str.length` 时候返回空字符串**
- 忽略为0

`length`表示要提取的字符串的长度

- **length <= 0返回空字符串**，连长度都没有还有什么好提取的
- length为正数的时候，当 length > `str.length`时，**提取到字符串末尾**
- 忽略也表示提取到末尾

```javascript
var str = '123456';
str.substr(0) // '123456'
str.substr(1) // '23456'

str.substr(-2) //等同于str.substr(4) '56' 倒数第二位开始
str.substr(-7) //等同于str.substr(0) '123456'

str.substr(2, 0) // ''
str.substr(0, 10) // '123456'
```

## `String.prototype.substring()`

> `str.substring(indexStart[, indexEnd])`

`indexStart`   表示首字符的索引值

- 为负数或`NaN`，则`indexStart`为0
- 为非负数，当`indexStart > str.length`时，`indexStart = str.length` 
- 忽略为0 

`indexEnd`  表示末尾字符的索引值, **在提取字符串时候不包含**

- 为负数或`NaN`，则`indexEnd`为0
- 为非负数，当`indexEnd > str.length`时，`indexEnd = str.length` 
- 忽略则提取到字符串末尾

> - 如果`indexStart`等于`indexEnd`，则`substring`返回一个空字符串
> - 如果`indexStart`大于`indexEnd`, 则 `substring` 的执行效果就像两个参数调换了一样。例如，`str.substring(1, 0) ==str.substring(0, 1)`。

```javascript
var str = '123456'

str.substring() //'123456'
str.substring(0) // '123456'
str.substring(-1, 10) // 等同于str.substring(0, 6) '123456'
str.substring(NaN, 5) // 等同于str.substring(0, 5) '12345'

str.substring(3, 3) //''
str.substring(5, 0) //等同于str.substring(0, 5) '12345'
str.substring(6, 8)//等同于str.substring(6,6) ''
```

## `String.prototype.slice()`

> `str.slice(beginSlice[, endSlice])`

`beginSlice` 首个字符的索引

- 为负数，`beginSlice = str.length + beginSlice`, 表示倒数，如果`|beginSlice| > str.length`, `benginSlice =  0`
- 为非负数，**若`beginSlice >= str.length `,`beginSlice = str.length `则函数返回一个空字符串**
- 忽略为0

`endSlice` 末尾字符的索引，提取中不包含

- 为负数，`endSlice = str.length + endSlice` , **如果`|endSlice| >= str.length` ,`endSlice= 0` 此时肯定返回一个空字符串**
- 为非负数，若`endSlice > str.length`，则`endSlice = str.length`
- 忽略表示提取到末尾

**当`benginSlice >= endSlice`时必定返回一个空字符串，因为它不会像substring()函数那样会参数调换**

```javascript
var str = '123456'

str.slice() // '123456'
str.slice(-1, -2) //等同于str.slice(5, 4) 5 > 4 ''
str.slice(0, -6) //等同于str.slice(0,0) ''
```

slice 还可以对数组对象进行操作，因为它也定义在数组原型对象上，而且规则和在字符串上相同

```javascript
var arr = [1,2,3,4,5,6]

arr.slice() // [1,2,3,4,5,6]
arr.slice(-1, -2) // 等同于arr.slice(-5, -4) []
arr.slice(0, -6) //等同于arr.slice(0, 0) []
```

## 小结

> - 除了`substr()`的第二个参数表示长度其余函数中的参数都是索引值
>
> - 三个函数返回的都是新的字符串或者数组，原对象不会改变
>
> - substring()的两个参数表示的都介于0到字符串长度的非负整数（负数会转化成非负数），如果要截取倒数几位可以这样表示
>
>   ```javascript
>   var str = 'abcdef';
>
>   str.substr(-3) // 'def'
>   str.substring(str.length-3) //def
>   ```
>
> - substring()当第一个参数大于第二个参数会发生参数调换
>
> - substring()和slice()的第二个参数所在位置在提取中都是不被包括的
>
> - substring()和slice()中两个参数相等返回空的字符串（数组）
>
> - 另外可以用`Array.prototype.slice.call(arraylike)`来将类数组对象转化成数组，或者`[].slice.call(arraylike)` ,ES6中可以用Array.from()来进行转换



啊写完了，图书馆的好几个小时呐效率真是低，感觉和文档又一点没差啊唉都是照搬照抄啊....还有种感觉就是写的时候自己已经全完会了还到底要不要再写那，算了算了