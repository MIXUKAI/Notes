# Iterator

## 知识点小结

- JS中表示集合的四种**数据结构**：`Array`, `Object`, `Map`, `Set`

- 遍历器(iterator)是一种接口，为不同的数据结构提供统一的访问机制（有序访问）

- iterator只是把接口规格加到数据结构上，因此遍历器与数据结构是分离的

- 凡存在`Symbol.iterator`属性即说明该对象可迭代，具有遍历器接口，可以使用`for...of`方法

- `Symbol.iterator`是**遍历器生成函数**，返回一个遍历器对象

- 遍历器对象本质是一个**指针对象**，具有三个方法:

  1. `next()`(必须),
  2. `return()`(遍历提前退出调用且必须返回一个对象)
  3. `trow()`（可选）

- next()方法返回一个**对象**，具有`value`,`done`两个属性

- 原生具备iterator接口：`Array`, `Map`, `Set`,某些类似数组的对象(`arguments`,`DOMNodeList`), `String`, `Generator`...

- 遍历器是一种**线性处理**，对于任何非线性的数据结构(Object),部署遍历器接口等于部署一种线性转换

- 某些本身不具备遍历器接口的类似数组的对象，部署接口可以直接引用数组的iterator接口

  ```javascript
  arrLike[Symbol.iterator] = Array.prototype[Symbol.iterator];

  // [][Symbol.iterator]
  ```

## 遍历语法比较

### `for`循环

> 最原始且简单，不过有点麻烦

### `forEach()`

> Array, Map, Set内置，写法简单，类似for循环无返回值。
>
> 无法跳出循环，即break，return无效

### `for...in`

- 可以遍历对象及**构造函数原型**中**可枚举的属性**
- 某些情况下**顺序任意**，非线性
- **数组的键名是数字**。而用该方法遍历则是字符串
- 可以跳出循环

> 因此适用于一般对象，不适用遍历数组

### `for...of`

- 需要对象部署遍历器接口(`Symbol.iterator`)
- 可以跳出循环
- 为不同的数据接口提供统一的遍历方式

> ES6新增，相比较其他遍历方式更为方便



### 