### 严格模式下 

函数声明提升到代码块的顶部，外部无法访问（如深入理解es6中所说 一旦if代码块执行完毕声明的函数也不复存在了，应该也确实如此因为代码块只执行一次不像定义在函数中 但就结果而言都是一样外部无法访问

与let,const比较类似，只不过它们并不会提升变量到代码块顶部

### 非严格模式下

书中所说是申明函数会被提升到外部函数或者全局环境的顶部
```javascript
console.log(typeof fn) // undefined
if (true) {
  console.log(typeof fn) // function
  function fn() {
    return 1
  }
}
console.log(typeof fn) // function
```
理应说输出的都是三个function,但第一个结果却是undefined

但是变量确实应该是被提升了，只是函数没有被提升，不像在外部直接进行函数声明
```javascript
console.log(fn) //undefined
if (true) {
  function fn() { return 1 }
}
```
如果变量未被提升的话log是会报错,fn is not defined
所以它应该只是提升了一个fn，更像是
```javascript
var fn
console.log(fn)
if (true) {
  //这里不应该是 fn = function() {...} 因为如果在函数定义之前使用该函数的话确实是可以，也就是说在代码块中函数确实是被提升到了顶部
  function fn() { ...}
}
```
大概是这样。。。