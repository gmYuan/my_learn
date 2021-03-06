# React之 Redux相关

Q1 Redux的整体流程是什么
A：
  - 一个集中管理共享数据的 数据仓库：store = createStore(Reducer, initState, middleApply)
  - 可以触发更新数据的 动作申请：store.dispatch(action = {type: xxx, payload: yyy})
  - 根据不同动作类型 ==> 更新并返回新数据的分发处理器：reducer(state, action) => newState对象，reducer是纯函数
  - Redux是单向数据流

<<<<<<< HEAD
用示意图表示为
![](https://gitee.com/ygming/blog-img/raw/master/img/react_redux.png)
=======
用代码表示为：
```js
import { createStore } from 'redux'
// 创建 reducer
const reducer = (state, action) => {
    // 此处是各种样的 state处理逻辑
    return new_state
}
// 基于 reducer 创建 state
const store = createStore(reducer)

// 创建一个 action，这个 action 用 “ADD_ITEM” 来标识 
const action = {
  type: "ADD_ITEM",
  payload: '<li>text</li>'
}
// 使用 dispatch 派发 action，action 会进入到 reducer 里触发对应的更新
store.dispatch(action)
```

>>>>>>> ebfba8c291f434c54065efb64bf12e74ec0da8d9

-----





Q1 React有哪些 组件抽象的方法
A：
  - Mixin
  - HOC

Q2 mixin存在的问题
A：
  - 引入了组件的外部依赖，mixin内部引入了新的state和props
  - mixin存在第三方 命名冲突的问题

Q3 HOC的原理
A：
  - 接收一个React.component A，返回一个增强的 React.component B 的函数
  - 即 const HOC = (wrapper, params) =>newCom

Q4 如何实现HOC
A:
```js
//方法1 属性代理： 操作传入A里的 props
//  作用1： 控制props的增改查内容  
//  作用2： 设置state，抽离组件状态 
//  作用3： 增强传入组件 的DOM结构/样式
const HOC = (wrappedCom) => {
  return class extends React.component {
    constructor(props) {
      super(props)
      ......
    }
    ......
    render() {
      return <wrappedCom {...this.props} data={xxx} />
    }
  }
}


//方法2 反向继承：返回的组件B 继承于 传入的A
// 作用1: 劫持渲染：可以控制 渲染内容/返回结果
const HOC = (wrappedCom) => {
  return class extends wrappedCom {
    render() {
       return super.render()
    }
  }
}
```






## 参考文档

01 [深入React技术栈](/)
02 [React官方文档- 高阶组件](https://zh-hans.reactjs.org/docs/higher-order-components.html)