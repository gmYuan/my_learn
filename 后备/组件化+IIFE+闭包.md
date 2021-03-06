## 组件化

1 什么是组件化
  我的理解是，以项目的一个功能为单位，独立封装JS代码片段。这就是组件化
  通过组件化，我们可以把代码分解成一个个单位，这样组织代码，既逻辑清晰，又易于调试

2 组件化的缺点
  声明的变量很容易变成全局变量，这就导致组件之间的同名变量可能会无意间相互影响和覆盖
  那么该如何解决这个问题呢？

## 立即执行函数(IIFE)

1 在ES6之前，JS只有两种作用域类型: 全局作用域和函数作用域
  换言之，如果我们想把全局变量变成局部变量，就只能把它放在一个函数内部,即
```js
function xxx(){
  .....
}
```

2 通过上一步，全局变量的确变成了局部变量，但是现在那个功能组件却无法使用了（因为函数没有执行）
  所以我们要执行刚刚封装的函数
```js
xxx.call()
```

但这样，又引入的一个新的全局变量xxx(函数名)，所以要用一个匿名函数来封装组件并调用
```js
function (){...}.call()
```

3 但是！这样的语法会报错，所以最后使用一个技巧，来构建立即执行函数:
```js
!function(){...}.call()
```

## 闭包的使用

1 函数解决了变量的封装，但如果我们想在组件之间互相访问某个变量呢？

2 一种方法是：使用`window.xxx`构造全局对象的属性

3 一种方法是：创建一个全局函数，这个函数+私有变量构成闭包，通过这个暴露的全局函数进行通信：
```js
//module 1内容
!function (){
  var person  = {
    'name': 'ygm',
    'age': 18
  }

  window.growUp = function(){
    person.age += 1;
    return person.age
  }
}.call()

//module 2内容
!function(){
  var newAge = window.growUp();
  console.log(newAge)
}.call()
```