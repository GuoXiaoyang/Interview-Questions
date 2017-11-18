## CSS相关问题

#### CSS 中类 (classes) 和 ID 的区别。

* ID唯一，CSS用`#`选择
* 类不唯一，CSS用`.`选择，

---

#### 请问 "resetting" 和 "normalizing" CSS 之间的区别？你会如何选择，为什么？

都是解决浏览器默认样式不一致的问题

`resetting`重置所有样式

`normalizing`只是修改一些样式让不同浏览器一致，相比起来更轻量级

---

#### 请解释浮动 (Floats) 及其工作原理。 

浮动使得元素脱离当前文档流，向指定方向移动直至遇到其他浮动元素或者父元素，初始是为了解决图像的文本围绕效果。现在是很多网页布局的一种方式。

---

#### 描述`z-index`和叠加上下文是如何形成的。

这个问题比较有名的CSS博主有篇文章比较详细<sup><a href="http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/">1</a></sup>，我大概总结下：

`z-index`描述元素在z轴的显示顺序，指示元素如何堆叠在屏幕上，大部分元素默认值auto。当元素发生层叠的时候，其覆盖关系遵循下面2个准则：

* 谁大谁上：在同一个层叠上下文中，较高`z-index`值的元素覆盖低`z-index`值的元素。
* 后来居上：当元素的层叠水平一致、层叠顺序相同的时候，在DOM流中处于后面的元素会覆盖前面的元素。



由于父元素嵌套子元素，所以层叠上下文主要描述了元素所处的局部层叠结构，毕竟不同的父元素里同样`z-index`值的元素层叠水平是不一致的。层叠上下文分为根元素层叠上下文／定位元素`z-index`非auto时的层叠上下文／CSS3中的层叠上下文。



---

#### 请描述 BFC(Block Formatting Context) 及其如何工作。

日常JS可能更关注些，对块级格式上下文的确不太了解，参考了这篇博文<sup><a href="http://kayosite.com/block-formatting-contexts-in-detail.html">2</a></sup>。

触发了块级格式上下文的元素可以看作是隔离的独立容器，容器里面的元素不会在布局上影响到外面的元素，并且 BFC 具有普通容器没有的一些特性，例如可以包含浮动元素，防止出现高度塌陷的问题。

触发 BFC 的条件如下：

- 浮动元素，float 除 none 以外的值
- 绝对定位元素，position（absolute，fixed）
- display 为以下其中之一的值 inline-blocks，table-cells，table-captions
- overflow 除了 visible 以外的值（hidden，auto，scroll）

主要有3个特性：

* 可以阻止外边距折叠


* 可以包含浮动元素


* 阻止元素被浮动元素覆盖

---

#### 列举不同的清除浮动的技巧，并指出它们各自适用的使用场景。

这篇博文<sup><a href="http://kayosite.com/remove-floating-style-in-detail.html">3</a></sup>总结得非常专业。貌似CSS的每个问题都可以开一篇博文来详细分析啊，可能CSS比较需要`show me the code, show me the result`吧。

浮动引起的问题主要是父元素内容高度不会被撑开，导致高度塌陷。清除浮动主要两种方案：`clear`和BFC元素(上个问题提到的能够包含浮动元素)。

`clear`有几种方法

* 直接对浮动元素的兄弟元素使用，但是这样兄弟元素的外边距会被浮动元素影响，大于浮动元素高度才能生效
* 空`div`元素使用`clear:float`，但是破坏语义和结构分离的原则
* `:after`伪元素，通常该方法弊端最少

BFC元素根据上一个问题有几种方法，比如`overflow`，父元素设置浮动或者改变`display`等，但父元素设置浮动会将问题传递给父元素的父元素，改变`display`则会破坏元素的盒子模型，所以`overflow`方法比较适用。

---

#### 请解释 CSS sprites，以及你要如何在页面或网站中实现它。

雪碧图，合并图片到一张大图，然后通过来`background-position`定位需要的图像；减少http请求次数。

---

#### 你最喜欢的图片替换方法是什么，你如何选择使用。

这个也没有接触过诶；

图片替换主要是指将文字替换成图片的技术，即在html语句中使用文字，浏览器显示时用对应的图片显示。其意义在于便于做网站优化（SEO），文字才是搜索引擎寻找的主要对象<sup><a href="https://www.cnblogs.com/wmhuang/p/image_change.html">4</a></sup>。

大概查询了下方案，真的不少<sup><a href="https://css-tricks.com/the-image-replacement-museum/">5</a></sup>：

1. **2015 H5BP**替换法

```html
<h3 class="visuallyhidden">
  CSS-Tricks
</h3>
```

```css
h3.visuallyhidden {
  border: 0;
  clip: rect(0 0 0 0);
  height: 1px;
  margin: -1px;
  overflow: hidden;
  padding: 0;
  position: absolute;
  width: 1px;
}
```



该方法并没有真的进行图片替换，只是将文字隐藏起来的同时对屏幕阅读器可见。图片可自行添加其他元素。

* **2012 H5BP替换法**

```html
<h3 class="h5bp">
  CSS-Tricks
</h3>
```

```css
h3.h5bp {
  border: 0;
  font: 0/0 a;
  text-shadow: none;
  color: transparent;
  background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/t-90/test.png);
  width: 300px;
  height: 75px;
}
```

将字体大小设为0来隐藏文字，貌似是一个错误的语法但可以生效？

- **2012 Scott Kellum替换法**

```html
<h3 class="skm">
  CSS-Tricks
</h3>
```

```css
h3.skm {
  width: 300px;
  height: 75px;
  background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/t-90/test.png);
  text-indent: 100%;
  white-space: nowrap;
  overflow: hidden;
}
```

将文字缩进到不可见。

- **2003 Phark替换法**

```html
<h3 class="phark">
  CSS-Tricks
</h3>
```

```css
h3.phark {
  width: 300px;
  height: 75px;
  background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/t-90/test.png);
  text-indent: -9999px;
}
```

同样将文字缩进到不可见。在浏览器不能显示图片时，丢失文字。

- **2003 Fahrner替换法（FIR）**

```html
<h3 class="fir">
  <span>Fahrner Image Replacement</span>
</h3>
```

```css
h3.fir {
  width: 300px;
  height: 300px;
  background: url(fir.jpg) no-repeat;
}
h3.fir span {
  display: none;
}

```

​	用`<span>`标签的display属性把文字隐藏起来，最后指定`<h1>`的背景图片。

​	优点：使用CSS而不是标记语法提供图片，更改图片只需更改CSS。

​	缺点：需要一组不具备任何语义的`<span>`标签；`display`属性影响屏幕阅读器使用者；浏览器不能显示图片时，文字不可显示。

- **2003 Glider/Levin 替换法**

```html
<h3 class="levin">
  <span></span>CSS-Tricks
</h3>
```

```css
h3.levin {
  width: 300px;
  height: 75px;
  position: relative;
}
h3.levin span {
  background: url(https://s3-us-west-2.amazonaws.com/s.cdpn.io/t-90/test.png);
  position: absolute;
  width: 100%;
  height: 100%;
}
```

使用空`<span>`标签显示图片，覆盖文字，在不显示图像时能够显示文字；但该方法要求图片不能透明，否则会显示出文字。



---

#### 你会如何解决特定浏览器的样式问题？

自己可能简单粗暴使用noramlize或者reset，如果针对特定浏览器，可以检测浏览器来发送与该浏览器相关的样式文件。

---

#### 如何为有功能限制的浏览器提供网页？

* 提示用户开启某些功能
* 使用Polyfill
* 优雅降级，针对可能失效的功能采取备选方案

---

#### 你会使用哪些技术和处理方法？

这个问题有些摸不着头脑啊，自由发挥？

仅指CSS的话，可以使用预处理css以及添加后缀、压缩合并等打包技术。

---
#### 有哪些的隐藏内容的方法 (如果同时还要保证屏幕阅读器可用呢)？

这个和图像替换的方法类似吧，隐藏内容但保证阅读器可用，需要元素可见(不能设置`display:none`)。

隐藏的方法包括但不限于：缩进或位置不可见／被其他内容覆盖／`display:hidden`／`overflow:hidden`隐藏溢出的内容

---
#### 你用过栅格系统 (grid system) 吗？如果使用过，你最喜欢哪种？

栅格系统主要规范了页面的布局，对于标准化开发比较友好；有过Bootstrap和Materialize-CSS的使用经验。对喜欢哪种目前没概念。

---
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
#### 你熟悉 SVG 样式的书写吗？

直接使用比较多，特别是矢量图标。大致看过MDN的文档，通过标签属性中的特定指令进行绘图，与canvas绘图方法差别较大。

---
#### 如何优化网页的打印样式？



---
#### 在书写高效 CSS 时会有哪些问题需要考虑？

---
#### 使用 CSS 预处理器的优缺点有哪些？

---
#### 请描述你曾经使用过的 CSS 预处理器的优缺点。

---
#### 如果设计中使用了非标准的字体，你该如何去实现？

---
#### 请解释浏览器是如何判断元素是否匹配某个 CSS 选择器？

---
#### 请描述伪元素 (pseudo-elements) 及其用途。

---
#### 请解释你对盒模型的理解，以及如何在 CSS 中告诉浏览器使用不同的盒模型来渲染你的布局。

---
#### 请解释 `* { box-sizing: border-box; }` 的作用, 并且说明使用它有什么好处？

---
#### 请罗列出你所知道的 display 属性的全部值

---
#### 请解释 inline 和 inline-block 的区别？

---
#### 请解释 relative、fixed、absolute 和 static 元素的区别

---
#### CSS 中字母 'C' 的意思是叠层 (Cascading)。请问在确定样式的过程中优先级是如何决定的 (请举例)？如何有效使用此系统？

---
#### 你在开发或生产环境中使用过哪些 CSS 框架？你觉得应该如何改善他们？

---
#### 请问你有尝试过 CSS Flexbox 或者 Grid 标准规格吗？

---
#### 为什么响应式设计 (responsive design) 和自适应设计 (adaptive design) 不同？

---
#### 你有兼容 retina 屏幕的经历吗？如果有，在什么地方使用了何种技术？

---
#### 请问为何要使用 `translate()` 而非 absolute positioning，或反之的理由？为什么？

---

#### 参考文献

[1] http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/

[2] http://kayosite.com/block-formatting-contexts-in-detail.html

[3] http://kayosite.com/remove-floating-style-in-detail.html

[4] https://www.cnblogs.com/wmhuang/p/image_change.html

[5] https://css-tricks.com/the-image-replacement-museum/