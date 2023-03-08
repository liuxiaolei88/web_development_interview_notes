# flex布局面试题总结

### Q1：什么是弹性盒布局？
- 特点：让元素对不同屏幕尺寸和不同显示设备做好适应。在响应式网站表现较好。

## 一、容器属性

### Q2：display:flex和display:inline-flex的作用
- 使容器变成弹性布局，为其子元素生成弹性格式化的上下文

### Q3：flex-direction
- 指定在弹性容器中如何摆放弹性元素
- row（横）、column（竖）
- 加上reverse会反转

### Q4：flex-wrap 换行
- 当主轴不够放的时候
- flex-wrap：nowrap/wrap/warp-reverse（换行放上面还是下面）

### Q5：flex-flow 弹性流
- 用于定义主轴（row（横）、column（竖））和垂轴的方向（wrap/warp-reverse），以及允许弹性元素换行
- 等于flex-direction和flex-wrap的简写，默认就是横着不换行
- 用法：flex-flow：row wrap

### Q6：justify-content
- 控制主轴上如何分布弹性元素

### Q7：align-item
- 控制垂轴上的对齐方式
- 轴线的计算是包括边界的


### Q8：align-self
- 在单个元素的对齐方式上使用align-self进行覆盖
- 默认值auto会继承align-item属性


### Q9：align-content
- 控制垂轴上如何分布弹性元素

## 二、项目属性
### Q10：order
- order属性定义项目的排列顺序。数值越小，排列越靠前，默认为0。

### Q11：flex-grow
- flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。

### Q12： flex-shrink
- flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。负值对该属性无效。如果flex-shrink值为0，表示该项目不收缩。

### Q13：flex-basis
- flex-basis属性定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。

### Q14：flex属性
- flex属性是flex-grow, flex-shrink 和 flex-basis的简写，默认值为0 1 auto。后两个属性可选。


## 参考资料
- https://blog.csdn.net/wwwjjjjj666/article/details/128033184
- https://blog.csdn.net/weixin_45112114/article/details/124366223