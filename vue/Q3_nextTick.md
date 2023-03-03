
## Q1：什么时候使用$nextTick
1. Vue⽣命周期的created()钩⼦函数进⾏的DOM操作⼀定要放在Vue.nextTick()的回调函数中
原因是在created()钩⼦函数执⾏的时候,DOM 其实并未进⾏任何渲染，⽽此时进⾏DOM操作⽆异于徒劳，
所以此处⼀定要将DOM操作的js代码放进Vue.nextTick()的回调函数中。
2. 当项⽬中改变data函数的数据，想基于新的dom做点什么，对新DOM⼀系列的js操作都需要放进Vue.nextTick()的回调函数中

## Q2：为什么使用nextTick？
Vue在更新data之后并不会立即更新DOM上的数据，直接获取dom值只能获取到旧值

## 参考链接
- https://www.jianshu.com/p/8efa5cba7d07
