> 兼容DOM的浏览器会将一个**event对象自动传入到事件处理程序中**，不论指定事件处理程序时使用什么方法（DOM0或者DOM2），都会传入event对象

```javascript
ele.onclick = function() {
  console.log(arguments.length) //1
}
arguments获取是实际传入函数的参数，与定义无关。因此可以说明event对象是被自动传入的

ele.onclick = function(e) {
  // e = event 给event一个别名，指向同一个对象
  console.log(e === event) // true
}
```

另外event对象只能做为仅有的参数传给事件处理程序
```javascript
ele.addEventListener('type', (para1, para2) => {
  console.log(para2) //undefined
})
```

在老的IE中访问event对象有几种不同方式,DOM0级方法中添加事件处理程序时候，event对象是作为window对象的一个属性存在的，因此我们常常见到以下这种方式来处理浏览器兼容问题
```javascript
ele.onclick = function(e) {
  e = event || window.event 
}
```
就现代浏览器而且应该不需要了，但是还是有老版本的ie存在的，因此还是得写

> 只有在事件处理程序执行期间，event对象才会存在；一旦事件处理程序执行完毕，event对象就会被摧毁