箭头函数的this值是由包含它的函数（非箭头函数）来决定的，**与包含的函数的this指向一致**，如果包裹它的不是函数**(直到找到最外层)**则this指向全局对象
并且箭头函数的this是固定的，由**定义**它时所在的环境（以上）所决定，而不是如非箭头函数那样由如何**调用**该函数来决定

```
var name = 'atm'
var person = {
  name: 'mxk',
  getName: () => console.log(this.name)
}
person.getName() // atm 
// 从这里可以看出箭头函数的this并不向非箭头函数的this那样指向person对象
var person = {
  name: 'mixukai',
  getName: function() {
    (() => console.log(this.name))()
  }
}
person.getName() // 'mixukai' 
// 上面这个例子可以看到箭头函数的this成功指向了getName这个函数的this对象
```

因为它的this值是不能改变的，**因此箭头函数也不能利用call(),apply(),bind()这三个方法来改变this**，尽管如此它还是可以使用这三个方法，因为它依旧是函数，是Function构造函数的实例