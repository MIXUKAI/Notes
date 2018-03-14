---
title: JavaScript偏移量
date: 2017-10-10 19:34:56
tags: DOM
---

## 偏移量介绍

>  偏移量( offset demension),  包括**元素**在屏幕上占用的所有**可见**的空间。元素的可见空间大小由其高度、宽度决定，包括所有内边距、滚动条和边框大小。

​	![](http://images2015.cnblogs.com/blog/740839/201609/740839-20160901091826449-1099471755.jpg)

## 四个属性

​	 这个四个属性**都是只读的**，返回值是**数值**，没有单位(其实是以像素来计算的)。

### offsetHeight

```css
offsetHeight = border-top-width + padding-top + height + padding-bottom + border-bottom-width
```

​	注意：当存在垂直的滚动条的时候也要把滚动条的宽度给考虑进去。

### offsetWidth

```css
offsetWidth = border-left-width + padding-left + width + padding-right + border-right-width
```

​	注意：同样的，当存在水平方向的滚动条时要将它考虑进去。

### offsetLeft

​	返回元素左外边框距离包含它最近的**定位元素**左内边框的像素距离。

### offsetTop

​	返回元素上外边框距离包含它最近的**定位元素**上内边框的像素距离。



## 偏移祖先元素

### offsetParent

​	上述提及的定位元素指的是其position值为absolute, fixed, relative, 不能为static，这是默认值。(这是我自己给它取的，并不标准！！)

​	元素offsetParent的几种不同的情况:

```html
<div class="outer">
  <div class="big">
    <div class="small"></div>
  </div>
</div>
```



- 元素自身以fixed定位的时候, offsetParent的结果的为null

  ```javascript
  .small {
    position: fixed;
  }

  document.getElementsByTagName('div')[2].offsetParent; //null
  ```

- 元素自身无fixed定位, 且其父元素或祖先元素的都没定位，即position为static，那么该元素的offsetParent为body

  ```javascript
  document.getElementsByTagName('div')[2].offsetParent; // <body></body>
  ```

- 元素自身没有fixed定位，父元素或祖先元素元素存在定位，该元素的offsetParent指向的就是包含它最近的那个定位元素。

  ```javascript
  .outer {
    position: absolute;
  }
  document.getElementsByTagName('div')[2].offsetParent; 
  //<div class="outer"</div>
  ```

- body元素的offsetParent是null

  ```javascript
  document.body.offsetParent //null
  ```

## 页面偏移

​	页面偏移其实指的就是距离整个页面的偏移量，即距离body的偏移量。

​	所以当某元素的offsetParent为body的时候，该元素的offsetTop就是该元素的距离页面的顶部的偏移量，而offsetLeft就是该元素距离页面左边的偏移量。

​	当元素的offsetParent不为body时，或存在多个嵌套，可以定义如下函数来获取:

```javascript
function getElementLeft(element) {
  var actualLeft = element.offsetLeft;
  var current = element.offsetParent;
    
  while (current !== null) {
    actualLeft += current.offsetLeft + current.clientLeft;
    current = current.offsetParent;
  }
  return actualLeft;
}

function getElementTop(element) {
  var actualTop = element.offsetTop;
  var current = element.offsetParent;
  
  while(current !== null){
    actualTop += current.offsetTop + current.clientTop;
    current = current.offsetParent;
  }
  return actualTop;
} 

// 元素的clientLeft的大小即为元素的左边框的宽度，clientTop为上边框的宽度
//因为offsetTop和offsetLeft都只是从外边框到内边框，因此在叠加的时候还要把边框宽度加上
```

## 注意

> ​	所有的这些偏移量属性都是只读的，而且每次访问他们都需要重新计算。因此，应该尽量避免重复访问这些属性。如果需要重复使用其中的某些属性的值，可以将他们保存在局部变量中，以提高性能。

​	还有在简介中也强调了“可见”两个字，还是要再强调下，如果display为none的时候, 四个偏移属性均为0, offsetParent为null(visibility: hidden时候虽然看不见, 不过属性均正常)。



最后，这算是第一篇学习笔记吧，文章中许多名词称呼也不是很准确，并非官方术语，这里还是要申明下。