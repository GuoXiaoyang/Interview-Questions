### 移动端问题



#### 你用过媒体查询，或针对移动端的布局/CSS 吗？

移动端适配的话都需要媒体查询吧。

Bootstrap的适配属于渐进增强方式，优先适配小屏设备。

```css
@media screen and {max-width:800px} {
  ...
}
```

针对移动端的布局就是将移动端样式通过设备查询还有图像查询进行调整，使网页内容可读性更强。

---

#### 说说你知道的移动端web的兼容性bug

1. 一些情况下对非可点击元素如(label,span)监听click事件，ios下不会触发，css增加cursor:pointer就搞定了。

2. position 在Safari下的两个定位需要都写，只写一个容易发生错乱

3. Input 的placeholder会出现文本位置偏上的情况

   input 的placeholder会出现文本位置偏上的情况：PC端设置line-height等于height能够对齐，而移动端仍然是偏上，解决是设置line-height：normal

4. zepto点击穿透问题

   引入fastclick解决；event.preventDefault

5. 当输入框在最底部的时候，弹起的虚拟键盘会把输入框挡住。

   Element.scrollIntoViewIfNeeded(opt_center)

#### 移动端最小触控区域是多大？



------

#### 移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时？（click 有 300ms 延迟,为了实现safari的双击事件的设计，浏览器要知道你是不是要双击操作。）

