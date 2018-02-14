## Set

ES6新的数据结构，**没有重复的值**。

Set加入值**不会发生类型转换，5与'5'不同**

内部判断两值相等采用**类似精确相等运算符`===`**

**因此对象总是不相等，例外是`NaN`等于`NaN`**

#### 两个方法：

- `Set.prototype.constructor` 构造函数Set
- `Set.prototype.size` 返回成员个数

#### 四个操作方法：

- `delete()` 返回布尔值
- `has()` 返回布尔值
- `add()` 返回Set本身，可以采用链式写法
- `clear()` 无返回

#### 四个遍历方法：

- `keys()` 返回键名的遍历器
- `values()` 返回键值的遍历器
- `entries()` 返回键值对的遍历器
- `forEach()` 调用回调函数遍历每个成员

```javascript
var set2 = new Set(['a', 'b', 3]);
undefined
for ( let item of set2.entries() ) {
	console.log(item)
}
// ["a", "a"]
// ["b", "b"]
// [3, 3]
```

Set的键名与键值相同，因此**`values()`与`keys()`对Set来说行为是一样的。**

```javascript
Set.prototype[Symbol.iterator] === Set.prototype.values
//true
```

因此可以直接跳过keys或者values**直接用`for...of`来遍历Set**

由于**扩展运算符...内部使用`for...of`**，因此也适用于Set

```javascript
数组去重
var arr = [1,2,3,3,2,1];
arr = [...new Set(arr)];
arr // [1, 2, 3]
```

## WeakSet

- 成员只能是对象
- 对象都是弱引用
- WeakSet是不可遍历的，也没有`size`属性

#### 作为构造函数的参数：

​	可迭代对象（具有`iterable`接口）

#### 3个方法：

​	**`has()`, `delete()`, `add()`**

WeakSet的对象都是弱引用，可以用来存储DOM节点而无需担心这些节点从文档移除时会引发内存泄漏

## Map

​	**"值-值"，键名可以是各种类型的值**，而对象的键名是字符串

​	**数组作为构造函数的参数**

```javascript
new Map([['name', 'mixukai']])
```

​	**Map的键是跟内存地址绑定的**，只有对一个对象的引用，才将其视为同一个键。

​	Map的键如果是简单类型的值，若严格相等则则视为同一个键。**`+0`与`-0`相同，`NaN`也和`NaN`相同**

#### 属性和方法

​	`size`, `constructor`

​	`set()`(返回Map本身),` get()`, `delete()`,` clear()`

​	`keys()`, `values()`, `entries()`, `forEach()`

#### Map转数组

```javascript
map[Symbol.iterator] === map.entries

[...map.keys()]

[...map.values()]

[...map.entries()]与[...map]效果相同

```



## WeakMap

- 键名必须是对象,`null`除外
- 键名是对象的弱引用
- **没有遍历操作**，无`values()`,`keys()`,`entries()`,也无size
- 无`clear()`


#### 四个方法：

​	`set()`, `get()`, `delete()`, `has()`