# React之 组件通信

Q1 组件通信方法有哪些
A：
  - parentA 通信 childB：通过props 传递值
  - B 通信 A：A 通过props 传入函数 给B  & 该函数内部可以访问A的上下文环境 + B调用传入的props函数
  - 多层嵌套组件通信：通过 context
  - 无嵌套关系的组件通信：发布/订阅模式 ==> 会带来逻辑混乱，加入流程约束 ==> Redux

-----
Q2 如何实现一个 发布-订阅模式
A：
```js
class EventEmitter {
  constructor() {
     //S1 eventMap 用来存储 事件和监听函数之间的关系
    this.eventMap = {}
  }
  //S2 订阅事件
  on(type, cb) {
    if (! (cb instanceof function) ) {
      throw new Error('cb 要是一个函数')
    }
    // 判断 type 事件对应的队列是否存在
    if ( !this.eventMap[type] ) {
      // 若不存在，新建该队列
      this.eventMap[type] = []
    }
    // 若存在，直接往队列里推入回调
    this.eventMap[type].push(cb)
  }
  //S3 发布事件
  emit(type, params) {
    if ( this.eventMap[type] ) {
      this.eventMap[type].forEach( (cb, index) => {
        cb(params)
      })
    }
  }
  // 取消订阅
  off(type, cb) {
    if ( this.eventMap[type] ) {
      // indexOf不存在时会返回-1 ==> splice(-1, 1) 就会误删掉数组最后一个元素 ==> -1 >>> 0 无符号右移，值为1*10e32
      this.eventMap[type].splice(this.eventMap[type].indexOf(cb) >>> 0, 1);
    }
  }
}
```

------
Q3 context的使用方法是什么

A：
```js
//S1 创建一个 context 对象
comContext = React.createContext(defaultValue)

// 特点：即使B组件的 shouldComponentUpdate返回false, value发生变化时C也能自动接收到最新值
// 而旧版本的API 则不会接收到最新值
<comContext.Provider value={xxx}>
  <B />
</comContext>

class C extends React.Component {
  //S2 contextType属性值为 一个由 React.createContext() 创建的 Context 对象
  static contextType = comContext

 //S3 xxx值为 Prover中的 value值
  const xxx = this.context
}
```

用图表示为
![context示意图](https://gitee.com/ygming/blog-img/raw/master/img/react_context.png)

## 参考文档

01 [深入浅出React](/)
02 [React官方文档- Context](https://zh-hans.reactjs.org/docs/context.html)