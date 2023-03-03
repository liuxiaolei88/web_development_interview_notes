# v-if和v-show的总结

## Q1：共同点：
v-if 和 v-show 都能实现元素的显示隐藏

## Q2：区别：
1.v-show 只是简单的控制元素的 display 属性，而 v-if 才是条件渲染（条件为真，元素将会被渲染，条件 为假，元素会被销毁）；
2. v-show 有更高的首次渲染开销，而 v-if 的首次渲染开销要小的多；
3. v-if 有更高的切换开销，v-show 切换开销小；
4. v-if 有配套的 v-else-if 和 v-else，而 v-show 没有
5. v-if 可以搭配 template 使用，而 v-show 不行

## Q3：v-show与v-if的使用场景
1. v-if 与 v-show 都能控制dom元素在页面的显示;
2. v-if 相比 v-show 开销更大的（直接操作dom节点增加与删除）
3. 如果需要非常频繁地切换，则使用 v-show 较好
4. 如果在运行时条件很少改变，则使用 v-if 较好

## 参考链接
- https://blog.csdn.net/weixin_58155690/article/details/125609235