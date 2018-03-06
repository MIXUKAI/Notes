## 节点层次

- 文档节点是每个文档的根节点
- 文档中其他所有元素都包含在文档元素中，一个文档只能有一个文档元素，HTML页面中始终是html元素
- **JS中所有节点类型都继承自Node类型**
- nodeType属性表示节点类型，最好用数值表示为了兼容IE
- childNodes -> NodeList 类数组

## 节点关系

- parentNode
- nextSibling
- previousSibling
- ownerDocument 指向根节点即文档节点
- firstChild
- lastChild
- hasChildNodes()用来判断是否有子节点，比查询childNodes的length更好

以上属性如若没有可以返回的节点则返回null

任何节点都不能同时存在两个或者多个文档中

## 节点操作

- appendChild(newNode) 返回newNode
- insertBefore(new, old) 返回newNode
- replaceChild(new, old) 返回oldNode
- removeChild(node) 返回node

1. 以上四个方法都是在父节点上操作
2. 并不是所有类型都有子节点，如果在不支持子节点的节点上操作会出出错
3. replace与remove移除的节点依旧是存在着的，只是没有了在文档中的位置

- cloneNode(boolean)

1. true深复制，还包括子节点，false浅复制只复制本身
2. 不复制属性(如事件处理程序）但会复制特性（attribute)

## 查找元素

- getElementById() 只返回第一个符合条件的元素
- getElementsByTagNams() 返回HTMLCollection对象
- getElementsByNams() 返回NodeList对象

DOM扩展添加选择符API

- querySelector()  返回第一个匹配的元素,没有则是null
- querySelectorAll() 返回NodeList对象,没有则是空的NodeList对象

## Document（9）

### 简介

- doucment对象是HTMLDocument（继承Doucment）的实例，表示整个HTML页面
- document是window对象
- 的一个属性
- nodeName = '#document'；nodeVlaue = null
- parentNode = null 因为文档节点已经是根节点了 
- ownerDocument = null而不是本身

### 属性

- **document.body** -> body元素

- **document.documentElement** -> html元素

- document.doctype -> <! DCOTYPE> 不常用

- document.title -> title元素中的文本，**可读可写**

- document.URL 完整的URL

- document.domain 只包含域名（hostname）

- document.referrer 链接到当前页面的url，在没有来源的情况下为空

  以下四个为特殊集合是HTMLCollection对象实时更新


- document.anchors 文档中所有带name特性的a元素
- document.forms 文档中所有form元素
- document.images 文档中所有img元素
- document.links 文档中所有带href的a元素

## Element（1）

### 简介

- nodeValue = null
- nodeName = tagName，HTML中返回大写标签名
- HTMLElement类型继承自Element并添加一些属性
- 所有HTML元素不是直接通过HTMLElement也是通过它的子类型来表示

### 属性

- id 对应特性id
- title 对应特性title
- className 对应特性class
- lang 元素内容的语言代码，很少用
- dir语言的方向，很少用

### 特性(attribute)

- getAttribute(), setAttribute(), removeAttribute()
- 特性是不区分大小写的
- 自定义特性加data-前缀以便验证
- **只有公认的特性才会以属性的形式添加到DOM对象中**，如自定义特性则不会
- **自定义属性不会称为元素的特性**
- 开发人员经常使用对象来的属性来替代getAttribute(),后者常用与自定义特性
- removeAttribute()不只是删除了特性值，实际上是删除了特性

### attributes属性

- Element类型是使用该属性的唯一一个节点类型
- 元素的每个特性节点都保存在NamedNodeMap对象中
- 在attributes上调用removeNamedItem()与在元素上调用removeAttribute()效果一致，只不过前者会返回Attr节点
- 开发人员最常用的还是getAttribute(), setAttribute()和removeAttribute()

### 元素子节点问题

​	因为元素的子节点有可能是元素节点，文本节点，注释等

​	不同浏览器解析时可能会有所差异，因此最好再次用nodeType在区分下来保证在不同的浏览器保持相同的效果

## Attr（2）

### 简介 

- nodeName为特性名，nodeVale为特性值，parentNode=null
- 从技术角度讲，特性就是存在于元素的attributes属性中的节点，每一个特性都由一个Attr节点表示
- 尽管他们也是节点，但特性却不被认为是DOM文档树中的一部分
- **开发人员最常用的还是getAttribute(), setAttribute()和removeAttribute()**，而不是直接引用特性节点
- 可以用通过document.createAttribute来创建节点，并通过setAttributeNode来添加到元素上

### 属性

- name 特性名
- value 特性值
- specified 布尔值，用于区别特性是默认还是特定的

## Text（3）

### 简介

- nodeName='#text'
- nodeValue = data 为文本值
- nodeParent是一个元素节点
- 默认情况下元素最多包含一个文本节点，但可以给元素添加文本节点。在显示上这两个文本会无缝连接

### 方法

- normalize()合并所有文本节点
- splitText(pos)返回是指定值到末尾的新文本节点，原本的被切割，这两个节点是同胞节点，但显示依旧没有变

## Comment（8）

- nodeName = '#comment'
- nodeValue = data 为注释内容
- parentNode可能是Document或者Element
- Comment与Text继承相同的积累，因此它拥有除splitText()之外的所有字符串操作

## CDATASection（4）

- nodeName = '#cdata-section'
- nodeValue为CDATA区域中的内容
- **只针对基于XML的文档**

## DocumentType（10）

- nodeName = doctype的名称 document.doctype.name
- DocumentType对象保存在document.doctype中

### 属性

- name 文档名称
- entities 文档类型描述的实体的NamedNodeMap对象
- notations 文档类型描述的符号的NamedNodeMap对象

name属性比较有用

## DocumentFragment（11）

### 简介

- nodeName = '#document-fragment'


- 所有节点类型中，只有DocumentFragment在文档中没有对应的标记
- 虽然不能把文档片段添加到文档中，但是可以用作仓库
- 文档片段继承了Node的所有方法
- 可以通过节点操作方法将文档片段的内容添加到文档书中
- 文档片段永远不会成为文档书的一部分
- 添加到文档片段的节点同样不会属于文档树

## NodeList

- getElementsByName()， childNodes返回
- querySelectorAll() 返回
- 类数组，动态集合
- item(), 可用[]索引访问

## HTMLCollection

- getElementsByTagName()返回
- document的特殊集合,anchors, forms, links, images
- 类数组，动态集合
- item(), namedItem()，利用[]时后台调用它们

## NamedNodeMap

- 存在于attributes属性中
- document.doctype.entities
- document.doctype.notations
- 动态集合，类数组

### 方法

- getNamedItem(name) 返回nodeName = name的节点，可用[]
- removeNamedItem(name) 从列表中移除nodeName = name的节点
- setNamedItem(name) 向列表中添加节点，nodeName属性为索引
- item(index) 返回index所在处的节点，可用[]



