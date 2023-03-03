# BFC 盒子面试题总结

## Q1：BFC盒子是什么？
BFC全称是Block Formatting Context
意思就是**块级格式化上下文**。
可以把BFC看做一个容器，容器里边的元素不会影响到容器外部的元素。

## Q2：如何创建BFC？
1. 根元素：body；
2. 元素设置浮动：float 除 none 以外的值；
3. 元素设置绝对定位：position (absolute、fixed)；
4. display 值为：inline-block、table-cell、table-caption、flex 等；
5. overflow 值为：hidden、auto、scroll； 

### Q3：BFC的作用
1. 解决 margin 的重叠问题：由于BFC是一个独立的区域，内部的元素和外部的元素互不影响，将两个元素变为两个BFC，就解决了margin重叠的问题。
2. 解决高度塌陷的问题：在对子元素设置浮动后，父元素会发生高度塌陷，也就是父元素的高度变为 0。解决这个问题，只需要把父元素变成一个 BFC。常用的办法是给父元素设置overflow:hidden。
3. 创建自适应两栏布局：可以用来创建自适应两栏布局：左边的宽度固定，右边的宽度自适应。 

### Q4:BFC的特点
1. 垂直方向上，自上而下排列，和文档流的排列方式一致。
2. 在 BFC 中上下相邻的两个容器的 margin会重叠
3. 计算 BFC 的高度时，需要计算浮动元素的高度
4. BFC区域不会与浮动的容器发生重叠
5. BFC是独立的容器，容器内部元素不会影响外部元素
6. 每个元素的左 margin 值和容器的左 border 相接触 

## 参考博客
- https://www.bilibili.com/read/cv21217433/
- https://blog.csdn.net/guoao20000915/article/details/125685983
- https://github.com/ljianshu/Blog/issues/15