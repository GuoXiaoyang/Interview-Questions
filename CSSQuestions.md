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

* 直接对浮动元素的兄弟元素使用`clear:both`，但是这样兄弟元素的外边距会被浮动元素影响，大于浮动元素高度才能生效
* 空`div`元素使用`clear:both`，但是破坏语义和结构分离的原则
* `:after`伪元素，通常该方法弊端最少

BFC元素根据上一个问题有几种方法，比如`overflow`，父元素设置浮动或者改变`display`等，但父元素设置浮动会将问题传递给父元素的父元素，改变`display`则会破坏元素的盒子模型，所以`overflow`方法比较适用。

---

#### 请解释 CSS sprites，以及你要如何在页面或网站中实现它。

雪碧图，合并图片到一张大图，然后通过来`background-position`定位需要的图像；减少http请求次数。

---

####   页面导入样式时，使用link和@import有什么区别？

* link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;
* 页面被加载的时，link导入的样式可以并行加载，而@import会等引用的样式加载完后继续加载后部分样式;
* link方式的样式的权重 高于@import的权重

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

直接使用比较多，特别是矢量图标。大致看过MDN的文档<sup><a href="https://developer.mozilla.org/en-US/docs/Web/SVG">6</a></sup>，通过标签属性中的特定指令进行绘图，与canvas绘图方法差别较大。

---
#### 如何优化网页的打印样式？

这个应该也是在设备查询中优化吧。

```css
@media print {
  ...
}
```

另外，在标签内也可以指定打印样式。

```Html
<link rel=“stylesheet” type="text/css" media="print" href="XXX.css">
```

* 不要使用CSS背景图片，使用`<img>`
* 注意像素的一致性

---
#### 在书写高效 CSS 时会有哪些问题需要考虑？

既然是高效能，当然要让浏览器选择效率提高，id选择效率高于类，但不能牺牲可读性。

* 避免使用通用选择器，尽量避免使用后代选择器
* 规范命名
* 组件化／模块化

---
#### 使用 CSS 预处理器的优缺点有哪些？

优点

* 更像一门编程语言，扩展性好，有函数／变量／混合器
* 可嵌套，书写更方便

缺点：

* 需要预处理器编译成CSS
* 门槛略高

---
#### 请描述你曾经使用过的 CSS 预处理器的优缺点。

使用过sass，优缺点如上吧。

---
#### 如果设计中使用了非标准的字体，你该如何去实现？

如果我自己使用，很可能就在`<head>`标签或者CSS中`@import`导入外部字体了；

但是有可能只有少数文本使用该字体，导入所有字体文件就有点浪费了

* 用图片代替
* 可以将使用的文本打包出自定义的Webfont，使用`@font-face`定义字体类别。

---
#### 请解释浏览器是如何判断元素是否匹配某个 CSS 选择器？

从后往前判断，因为CSS选择器的规则肯定小于元素集合数量，所以遍历选择器规则来过滤元素集合是比较快捷，至于为什么从后往前，主要是元素集合是树形结构，查找兄弟元素和父元素更为方便。

---
#### 请描述伪元素 (pseudo-elements) 及其用途。

伪元素不是真正的元素，所以JS不能操作伪元素，但CSS选择器可以选择并为其设置样式。

- [`::after`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after)
- [`::before`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::before) 这两个伪元素通常用来清除浮动／为元素添加背景或其他样式的美化
- [`::first-letter`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-letter)
- [`::first-line`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::first-line)  这两个伪元素则是选择部分内容进行样式改变
- [`::selection`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::selection)  选择文档中被用户高亮的部分，处于CSS3中第4级伪元素。

另外还有伪类(pseudo-classes)，CSS伪类是添加到选择器的关键字，指定要选择的元素的特殊状态。伪类数量比较多：

- [`:active`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:active)
- [`:any`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:any)
- [`:checked`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:checked)
- [`:default`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:default)
- [`:dir()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:dir)
- [`:disabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:disabled)
- [`:empty`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:empty)
- [`:enabled`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:enabled)
- [`:first`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first)
- [`:first-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-child)
- [`:first-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:first-of-type)
- [`:fullscreen`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:fullscreen)
- [`:focus`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:focus)
- [`:hover`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:hover)
- [`:indeterminate`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:indeterminate)
- [`:in-range`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:in-range)
- [`:invalid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:invalid)
- [`:lang()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:lang)
- [`:last-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-child)
- [`:last-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:last-of-type)
- [`:left`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:left)
- [`:link`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:link)
- [`:not()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:not)
- [`:nth-child()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-child)
- [`:nth-last-child()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-child)
- [`:nth-last-of-type()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-last-of-type)
- [`:nth-of-type()`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:nth-of-type)
- [`:only-child`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:only-child)
- [`:only-of-type`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:only-of-type)
- [`:optional`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:optional)
- [`:out-of-range`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:out-of-range)
- [`:read-only`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:read-only)
- [`:read-write`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:read-write)
- [`:required`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:required)
- [`:right`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:right)
- [`:root`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:root)
- [`:scope`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:scope)
- [`:target`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:target)
- [`:valid`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:valid)
- [`:visited`](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:visited)

---
#### 请解释你对盒模型的理解，以及如何在 CSS 中告诉浏览器使用不同的盒模型来渲染你的布局。

盒模型<sup><a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model">7</a></sup>是CSS呈现内容样式的核心之一，包括margin-border-padding-content。MDN上标准盒模型的图示：

![标准盒模型](https://developer.mozilla.org/files/72/boxmodel%20(1).png)

标准盒模型下，宽高不包括padding和border，元素的背景会延伸到padding和border；外边距在同一BFC中会发生折叠。

但IE的怪异模式下，盒模型宽高会包括padding和borde。

通过`box-sizing`能够更改用于计算元素宽度和高度的默认的 CSS 盒子模型。

* `box-sizing: content-box;` 默认值，元素宽高等于内容区宽高。
* `box-sizing: border-box;` 元素宽高包括padding、border和content，

---

#### 请解释 `* { box-sizing: border-box; }` 的作用, 并且说明使用它有什么好处？

参见该文<sup><a href=”https://css-tricks.com/international-box-sizing-awareness-day/“>8</a></sup>

将所有元素的盒模型设置为`border-box`，能够固定实际的宽高，不根据设置的内边距和边框改变，比较适用于固定的列布局。

另一方面，也能解决和IE不一致的问题吧。

---
#### 请罗列出你所知道的 display 属性的全部值

如果说我知道的：

* none
* block
* inline
* inline-block
* table
* table-cell
* flex
* grid
* inherit

我不知道的：

有很多，就不列举了<sup><a href="https://developer.mozilla.org/en-US/docs/Web/CSS/display">9</a></sup>。

---
#### 请解释 inline 、 inline-block和block 的区别？

* 块元素

  默认情况宽度与父元素一致，独占一行，前后块元素会换行

  可以设置宽高，内边距和外边距

* 行内元素

  宽度与内容一致，前后没有换行符

  不能设置元素的宽高

  水平方向可以设置外边距和内边距，竖直方向不可以

* 行内块元素

  前后没有换行符，多个行内块在一行显示

  可以设置元素宽高

---
#### 请解释 relative、fixed、absolute 和 static 元素的区别

不同定位的区别。

* static

  默认定位，按照文档流的位置来定位

* relative 相对定位

  元素按照本来static定位的位置进行相对位移，位移值为left与top的值

  不脱离文档流，原来的占位存在

* absolute 绝对定位

  元素脱离文档流，相对最近的非static定位父元素进行定位

* fixed 固定定位

  元素脱离文档流，相对整个页面窗口进行定位

  ​

---
#### CSS 中字母 'C' 的意思是叠层 (Cascading)。请问在确定样式的过程中优先级是如何决定的 (请举例)？如何有效使用此系统？

* 选择器的优先级<sup><a href="https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity">10</a></sup>，按以下位数计数法的权重规则排列：
  * 0 类型选择器
  * 1 类选择器／属性选择器／伪类
  * 2 ID选择器
  * 通用选择器(`*`)／组合选择(`+ > ~ ` )对选择器优先级没有影响
* 优先级相同时后声明样式优先级高于先声明样式
* 内联样式优先级高于外部样式
* `!important` 样式优先级高于任何样式，尽量避免使用

---
#### 你在开发或生产环境中使用过哪些 CSS 框架？你觉得应该如何改善他们？

使用过Bootsrtap框架和Materialize-CSS框架。

觉得框架使用浮动实现栅格系统没有flex灵活，毕竟需要清除浮动，容易引入一些布局问题。

---
#### 请问你有尝试过 CSS Flexbox 或者 Grid 标准规格吗？

尝试过flex布局，了解过grid布局；

这两个布局都是CSS3引入的display属性，目前还不是全平台兼容性。

曾经引入flex解决固定底部Footer的问题。

---
#### 为什么响应式设计 (responsive design) 和自适应设计 (adaptive design) 不同？

* 自适应设计，不同设备／屏幕布局不同，但在每个布局中元素不随窗口大小的调整发生变化
* 流动式设计，页面元素宽度会随窗口的大小而改变
* 响应式设计，结合上两者，同时对屏幕进行自适应布局和流式布局

---
#### 你有兼容 retina 屏幕的经历吗？如果有，在什么地方使用了何种技术？

没有。参考这篇博文<sup><a href="https://www.cnblogs.com/cench/p/5314044.html">11</a></sup>吧。

retina屏幕属于一种高像素密度的高分屏，在苹果设备上，一个独立的像素会包含几个物理像素，所以网页的像素适配不一样。

对于字体的渲染，苹果设备会使用矢量字体，不会造成像素的模糊或者失真；对于图像，就需要适配大几倍的像素图。

对于普通屏幕与retina屏幕，通过设备查询来适配。

---
#### 请问为何要使用 `translate()` 而非 absolute positioning，或反之的理由？为什么？

这个主要涉及浏览器的渲染性能<sup><a href="https://developers.google.com/web/fundamentals/performance/rendering/">12</a></sup>。

浏览器进行页面渲染的步骤是

![浏览器渲染步骤图](BrowserRendering.svg)

* JavaScrit    实现元素的添加／删除／移动等动画
* 样式计算     根据CSS选择器规则获取所有元素的最终样式
* 布局    得到元素的样式后开始计算元素的位置和占据的空间大小，最终得到页面的整体布局
* 绘制   浏览器开始绘制元素的文本、背景、边框和阴影等
* 合成    由于页面的各部分可能被绘制到多层，由此它们需要按正确顺序绘制到屏幕上，以便正确渲染页面。

渲染的每个步骤都会影响到页面的渲染性能，但是触发的步骤越靠前，后边的步骤都要重新计算。因此，属性的赋值要尽量改变元素靠后步骤的属性，比如改变元素的颜色就要比改变布局计算量小。

也就是说，布局要在页面渲染的时候尽量一次性完成，后边的动画部分用`translate()`性能要优于absolute positioning。

---



---

多种方法实现两列布局



---

常见的CSS hack



---

哪些属性可继承？哪些不可？



---

Flex布局与Grid布局



---

**9.rgba()和opacity的透明效果有什么不同？**

　　答案：

　　rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，

　　而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

---

#### 参考文献

[1] http://www.zhangxinxu.com/wordpress/2016/01/understand-css-stacking-context-order-z-index/

[2] http://kayosite.com/block-formatting-contexts-in-detail.html

[3] http://kayosite.com/remove-floating-style-in-detail.html

[4] https://www.cnblogs.com/wmhuang/p/image_change.html

[5] https://css-tricks.com/the-image-replacement-museum/

[6] https://developer.mozilla.org/en-US/docs/Web/SVG

[7] https://developer.mozilla.org/zh-CN/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model

[8] https://css-tricks.com/international-box-sizing-awareness-day/

[9] https://developer.mozilla.org/en-US/docs/Web/CSS/display

[10] https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity

[11] https://www.cnblogs.com/cench/p/5314044.html

[12] https://developers.google.com/web/fundamentals/performance/rendering/