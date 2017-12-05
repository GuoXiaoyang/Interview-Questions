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

这篇博文<sup><a href="http://kayosite.com/remove-floating-style-in-detail.html">3</a></sup>总结得非常专业。貌似CSS的每个问题都可以开一篇博文来详细分析啊，可能CSS比较需要`show me the code， show me the result`吧。

浮动引起的问题主要是父元素内容高度不会被撑开，导致高度塌陷。清除浮动主要两种方案：`clear`和BFC元素(上个问题提到的能够包含浮动元素)。

`clear`有几种方法

* 直接对浮动元素的兄弟元素使用`clear:both`，但是这样兄弟元素的外边距会被浮动元素影响，大于浮动元素高度才能生效
* 空`div`元素使用`clear:both`，但是破坏语义和结构分离的原则
* `:after`伪元素，通常该方法弊端最少

BFC元素根据上一个问题有几种方法，比如`overflow`，父元素设置浮动或者改变`display`等，但父元素设置浮动会将问题传递给父元素的父元素，改变`display`则会破坏元素的盒子模型，所以`overflow`方法比较适用。

---

#### 请解释 CSS sprites，以及你要如何在页面或网站中实现它。

概念：将多个小图片拼接到一个图片中。通过`background-position`和元素尺寸调节需要显示的背景图案。

优点：

1. 减少HTTP请求数，极大地提高页面加载速度
2. 增加图片信息重复度，提高压缩比，减少图片大小
3. 更换风格方便，只需在一张或几张图片上修改颜色或样式即可实现

缺点：

1. 图片合并麻烦
2. 维护麻烦，修改一个图片可能需要从新布局整个图片，样式

---

####   页面导入样式时，使用link和@import有什么区别？

* link属于XHTML标签，除了加载CSS外，还能用于定义RSS， 定义rel连接属性等作用；而@import是CSS提供的，只能用于加载CSS;
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

1. visibility: hidden；这个属性只是简单的隐藏某个元素，但是元素占用的空间任然存在。
2. opacity: 0；一个CSS3属性，设置0可以使一个元素完全透明，制作出和visibility一样的效果。与visibility相比，它可以被transition和animate
3. position: absolute；使元素脱离文档流，处于普通文档之上，给它设置一个很大的left负值定位，使元素定位在可见区域之外。
4. display: none；元素会变得不可见，并且不会再占用文档的空间。
5. transform: scale(0)；将一个元素设置为无限小，这个元素将不可见。这个元素原来所在的位置将被保留。
6. HTML5 hidden attribute；hidden属性的效果和display:none;相同，这个属性用于记录一个元素的状态
7. height: 0; overflow: hidden；将元素在垂直方向上收缩为0,使元素消失。只要元素没有可见的边框，该技术就可以正常工作。
8. filter: blur(0)；将一个元素的模糊度设置为0，从而使这个元素“消失”在页面中。

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

#### 请解释 `* { box-sizing: border-box; }` 的作用， 并且说明使用它有什么好处？

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

#### 如何居中div

- 水平居中：给div设置一个宽度，然后添加margin:0 auto属性

  ```css
   div{
   	width:200px;
   	margin:0 auto;
    }

  ```

- 让绝对定位的div居中

  ```css
  div {
    position: absolute;
    width: 300px;
    height: 300px;
    margin: auto;
    top: 0;
    left: 0;
    bottom: 0;
    right: 0;
  }

  ```

- 水平垂直居中一

  ```css
  // 已知确定容器的宽高 宽500 高300
  // 设置层的外边距

  div {
    position: relative;		/* 相对定位或绝对定位均可 */
    width:500px;
    height:300px;
    top: 50%;
    left: 50%;
    margin: -150px 0 0 -250px;     	/* 外边距为自身宽高的一半 */
  }

  ```

- 水平垂直居中二

  ```css
   // 未知容器的宽高，利用transform属性

   div {
   	position: absolute;		/* 相对定位或绝对定位均可 */
   	top: 50%;
   	left: 50%;
   	transform: translate(-50%， -50%);
   }

  ```

- 水平垂直居中三

  ```css
   // 利用 flex 布局
   // 实际使用时应考虑兼容性

   .container {
   	display: flex;
   	align-items: center; 		/* 垂直居中 */
   	justify-content: center;	/* 水平居中 */
   }
   .container div {
   	width: 100px;
   	height: 100px;
   }  
  ```

---

#### CSS3有哪些新特性？

- 新增各种CSS选择器	（: not(.input)：所有 class 不是“input”的节点）
- 圆角		    （border-radius:8px）
- 多列布局	    （multi-column layout）
- 阴影和反射	（Shadow\Reflect）
- 文字特效		（text-shadow）
- 文字渲染		（text-decoration）
- 线性渐变		（gradient）
- 变形如旋转、缩放、移动、倾斜等	（transform）

---

#### 多种方法实现两列布局

---

#### 品字布局

上面的div宽100%，
下面的两个div分别宽50%，
然后用float或者inline使其不换行即可

---

#### 常见的CSS hack

三角形

```css
#demo {
  width: 0;
  height: 0;
  border-width: 20px;
  border-style: solid;
  border-color: transparent transparent red transparent;
}
```





---

#### 哪些属性可继承？哪些不可？



---

#### css多列等高如何实现？

* 利用padding-bottom|margin-bottom正负值相抵；

设置父容器设置超出隐藏（overflow:hidden），这样子父容器的高度就还是它里面的列没有设定padding-bottom时的高度，当它里面的任 一列高度增加了，则父容器的高度被撑到里面最高那列的高度，其他比这列矮的列会用它们的padding-bottom补偿这部分高度差。

```css
.container{
  margin:0 auto; 
  width:600px; 
  border:3px solid #00C;
  overflow:hidden;
 /*这个超出隐藏的声明在IE6里不写也是可以的*/
}
.left{
  float:left; 
  width:150px; 
  background:#B0B0B0;
  padding-bottom:2000px;
  margin-bottom:-2000px;
}
.right{
  float:left; 
  width:450px; 
  background:#6CC;
  padding-bottom:2000px;
  margin-bottom:-2000px;
}
```

- 假等高列：使用背景图片，在列的父元素上使用这个背景图进行Y轴的铺放，从而实现一种等高列的假像
- 给容器div使用单独的背景色（[固定布局](http://codepen.io/strick/pen/ZbZYoW)）（[流体布局](http://codepen.io/strick/pen/WQWOPK)）：用元素中的最大高度撑大其他的容器高度
- 创建[带边框的两列](http://codepen.io/strick/pen/bVJRQv)等高布局：用border-left来做，只能使用两列。
- 使用[正padding和负margin](http://codepen.io/strick/pen/qOwXEN)对冲实现多列布局方法：在所有列中使用正的上、下padding和负的上、下margin，并在所有列外面加上一个容器，设置overflow:hiden把溢出背景切掉
- 使用[边框和定位模拟](http://codepen.io/strick/pen/XmQabJ)列等高：但不能使用在多列
- [模仿表格布局](http://codepen.io/strick/pen/ZbZJGg)等高列效果：兼容性不好，在ie6-7无法正常运行

---

#### Flex布局与Grid布局



---

#### rgba()和opacity的透明效果有什么不同

答案：

rgba()和opacity都能实现透明效果，但最大的不同是opacity作用于元素，以及元素内的所有内容的透明度，

而rgba()只作用于元素的颜色或其背景色。（设置rgba透明的元素的子元素不会继承透明效果！）

---

### 如何确定一个元素的包含块(containing block)

1. 根元素的包含块叫做初始包含块，在连续媒体中他的尺寸与viewport相同并且anchored at the canvas origin；对于paged media，它的尺寸等于page area。初始包含块的direction属性与根元素相同。

2. `position`为`relative`或者`static`的元素，它的包含块由最近的块级（`display`为`block`,`list-item`, `table`）祖先元素的**内容框**组成

3. 如果元素`position`为`fixed`。对于连续媒体，它的包含块为viewport；对于paged media，包含块为page area

4. 如果元素`position`为`absolute`，它的包含块由祖先元素中最近一个`position`为`relative`,`absolute`或者`fixed`的元素产生，规则如下：

   - 如果祖先元素为行内元素，the containing block is the bounding box around the **padding boxes** of the first and the last inline boxes generated for that element.
   - 其他情况下包含块由祖先节点的**padding edge**组成

   如果找不到定位的祖先元素，包含块为**初始包含块**

---

#### absolute的containing block(容器块)计算方式跟正常流有什么不同

  无论属于哪种，都要先找到其祖先元素中最近的 position 值不为 static 的元素，然后再判断：
  1、若此元素为 inline 元素，则 containing block 为能够包含这个元素生成的第一个和最后一个 inline box 的 padding box (除 margin， border 外的区域) 的最小矩形；
  2、否则，则由这个祖先元素的 padding box 构成。
  如果都找不到，则为 initial containing block。

  补充：
    1. static(默认的)/relative：简单说就是它的父元素的内容框（即去掉padding的部分）
    2. absolute: 向上找最近的定位为absolute/relative的元素
    3. fixed: 它的containing block一律为根元素(html/body)，根元素也是initial containing block

---

#### CSS里的visibility属性有个collapse属性值是干嘛用的？在不同浏览器下以后什么区别

对于普通元素`visibility:collapse`会将元素完全隐藏，不占据页面布局空间，与`display:none`表现相同。如果目标元素为table，`visibility:collapse`将table隐藏，但是会占据页面布局空间。 仅在Firefox下起作用，IE会显示元素，Chrome会将元素隐藏，但是占据空间。

---

#### position跟display、float这些特性相互叠加后会怎么样？

如果元素`display:none`，那么元素不被渲染，`position，float`不起作用

如果元素拥有`position:absolute`或者`position:fixed`属性那么元素将为绝对定位，float不起作用。

如果元素float属性不是none，元素会脱离文档流，根据float属性值来显示。有浮动，绝对定位，inline-block属性的元素，margin不会和垂直方向上的其他元素margin折叠。

---

#### `display: none;`与`visibility: hidden;`的区别

联系：它们都能让元素不可见

区别：

1. display:none;会让元素完全从渲染树中消失，渲染的时候不占据任何空间；visibility: hidden;不会让元素从渲染树消失，渲染师元素继续占据空间，只是内容不可见
2. display: none;是非继承属性，子孙节点消失由于元素从渲染树消失造成，通过修改子孙节点属性无法显示；visibility: hidden;是继承属性，子孙节点消失由于继承了hidden，通过设置visibility: visible;可以让子孙节点显式
3. 修改常规流中元素的display通常会造成文档重排。修改visibility属性只会造成本元素的重绘。
4. 读屏器不会读取display: none;元素内容；会读取visibility: hidden;元素内容

---

#### specified value,computed value,used value计算方法

- specified value: 计算方法如下：
  1. 如果样式表设置了一个值，使用这个值
  2. 如果没有设置值，这个属性是继承属性，从父元素继承
  3. 如果没设置，并且不是继承属性，使用css规范指定的初始值
- computed value: 以specified value根据规范定义的行为进行计算，通常将相对值计算为绝对值，例如em根据font-size进行计算。一些使用百分数并且需要布局来决定最终值的属性，如width，margin。百分数就直接作为computed value。line-height的无单位值也直接作为computed value。这些值将在计算used value时得到绝对值。**computed value的主要作用是用于继承**
- used value：属性计算后的最终值，对于大多数属性可以通过window.getComputedStyle获得，尺寸值单位为像素。以下属性依赖于布局，
  - background-position
  - bottom, left, right, top
  - height, width
  - margin-bottom, margin-left, margin-right, margin-top
  - min-height, min-width
  - padding-bottom, padding-left, padding-right, padding-top
  - text-indent

---

#### CSS有哪些继承属性

- 关于文字排版的属性如：
  - [font](https://developer.mozilla.org/en-US/docs/Web/CSS/font)
  - [word-break](https://developer.mozilla.org/en-US/docs/Web/CSS/word-break)
  - [letter-spacing](https://developer.mozilla.org/en-US/docs/Web/CSS/letter-spacing)
  - [text-align](https://developer.mozilla.org/en-US/docs/Web/CSS/text-align)
  - [text-rendering](https://developer.mozilla.org/en-US/docs/Web/CSS/text-rendering)
  - [word-spacing](https://developer.mozilla.org/en-US/docs/Web/CSS/word-spacing)
  - [white-space](https://developer.mozilla.org/en-US/docs/Web/CSS/white-space)
  - [text-indent](https://developer.mozilla.org/en-US/docs/Web/CSS/text-indent)
  - [text-transform](https://developer.mozilla.org/en-US/docs/Web/CSS/text-transform)
  - [text-shadow](https://developer.mozilla.org/en-US/docs/Web/CSS/text-shadow)
- [line-height](https://developer.mozilla.org/en-US/docs/Web/CSS/line-height)
- [color](https://developer.mozilla.org/en-US/docs/Web/CSS/color)
- [visibility](https://developer.mozilla.org/en-US/docs/Web/CSS/visibility)
- [cursor](https://developer.mozilla.org/en-US/docs/Web/CSS/cursor)
- 列表相关 `list-style-image，list-style-position，list-style-type， list-style`

---

#### 抽离样式模块怎么写

---

#### css属性content有什么作用？有什么应用？

css的content属性专门应用在 before/after 伪元素上，用于来插入生成内容。

可以配合自定义字体显示特殊符号。

---

#### 元素竖向的百分比设定是相对于容器的高度吗



---

#### 在CSS样式中常使用px、em、rem，各有什么优劣，在表现上有什么区别？

px是相对长度单位，相对于显示器屏幕分辨率而言的。

em是相对长度单位，相对于当前对象内文本的字体尺寸。

px定义的字体，无法用浏览器字体放大功能。

em的值并不是固定的，会继承父级元素的字体大小，1 ÷ 父元素的font-size × 需要转换的像素值 = em值。

rem

---

#### 全屏滚动的原理是什么，用到了CSS的那些属性



---

#### 让页面里的字体变清晰，变细用CSS怎么做？

  `-webkit-font-smoothing: antialiased;`

---

#### overflow: scroll时不能平滑滚动的问题怎么处理



---

#### 有一个高度自适应的div，里面有两个div，一个高度100px，希望另一个填满剩下的高度。



---

#### png、jpg、gif 这些图片格式解释一下，分别什么时候用。有没有了解过webp

**GIF**:

1. 8位像素，256色
2. 无损压缩
3. 支持简单动画
4. 支持boolean透明
5. 适合简单动画

**JPEG**：

1. 颜色限于256
2. 有损压缩
3. 可控制压缩质量
4. 不支持透明
5. 适合照片

**PNG**：

1. 有PNG8和truecolor PNG
2. PNG8类似GIF颜色上限为256，文件小，支持alpha透明度，无动画
3. 适合图标、背景、按钮

**Webp**：

谷歌（google）开发的一种旨在加快图片加载速度的图片格式。图片压缩体积大约只有JPEG的2/3，并能节省大量的服务器带宽资源和数据空间。Facebook Ebay等知名网站已经开始测试并使用WebP格式。

**Apng**：

全称是“Animated Portable Network Graphics”, 是PNG的位图动画扩展，可以实现png格式的动态图片效果。04年诞生，但一直得不到各大浏览器厂商的支持，直到日前得到 iOS safari 8的支持，有望代替GIF成为下一代动态图标准。

---

#### style标签写在body后与body前有什么区别？



---

### CSS选择器有哪些

1. ***通用选择器**：选择所有元素，**不参与计算优先级**，兼容性IE6+
2. **#X id选择器**：选择id值为X的元素，兼容性：IE6+
3. **.X 类选择器**： 选择class包含X的元素，兼容性：IE6+
4. **X Y后代选择器**： 选择满足X选择器的后代节点中满足Y选择器的元素，兼容性：IE6+
5. **X 元素选择器**： 选择标所有签为X的元素，兼容性：IE6+
6. **:link，：visited，：focus，：hover，：active链接状态**： 选择特定状态的链接元素，顺序LoVe HAte，兼容性: IE4+
7. **X + Y直接兄弟选择器**：在**X之后第一个兄弟节点**中选择满足Y选择器的元素，兼容性： IE7+
8. **X > Y子选择器**： 选择X的子元素中满足Y选择器的元素，兼容性： IE7+
9. **X ~ Y兄弟**： 选择**X之后所有兄弟节点**中满足Y选择器的元素，兼容性： IE7+
10. **[attr]**：选择所有设置了attr属性的元素，兼容性IE7+
11. **[attr=value]**：选择属性值刚好为value的元素
12. **[attr~=value]**：选择属性值为空白符分隔，其中一个的值刚好是value的元素
13. **[attr|=value]**：选择属性值刚好为value或者value-开头的元素
14. **[attr^=value]**：选择属性值以value开头的元素
15. **[attr$=value]**：选择属性值以value结尾的元素
16. *[attr=value]**：选择属性值中包含value的元素
17. **[:checked]**：选择单选框，复选框，下拉框中选中状态下的元素，兼容性：IE9+
18. **X:after, X::after**：after伪元素，选择元素虚拟子元素（元素的最后一个子元素），CSS3中::表示伪元素。兼容性:after为IE8+，::after为IE9+
19. **:hover**：鼠标移入状态的元素，兼容性a标签IE4+， 所有元素IE7+
20. **:not(selector)**：选择不符合selector的元素。**不参与计算优先级**，兼容性：IE9+
21. **::first-letter**：伪元素，选择块元素第一行的第一个字母，兼容性IE5.5+
22. **::first-line**：伪元素，选择块元素的第一行，兼容性IE5.5+
23. **:nth-child(an + b)**：伪类，选择前面有an + b - 1个兄弟节点的元素，其中n >= 0， 兼容性IE9+
24. **:nth-last-child(an + b)**：伪类，选择后面有an + b - 1个兄弟节点的元素 其中n >= 0，兼容性IE9+
25. **X:nth-of-type(an+b)**：伪类，X为选择器，**解析得到元素标签**，选择**前面**有an + b - 1个**相同标签**兄弟节点的元素。兼容性IE9+
26. **X:nth-last-of-type(an+b)**：伪类，X为选择器，解析得到元素标签，选择**后面**有an+b-1个相同**标签**兄弟节点的元素。兼容性IE9+
27. **X:first-child**：伪类，选择满足X选择器的元素，且这个元素是其父节点的第一个子元素。兼容性IE7+
28. **X:last-child**：伪类，选择满足X选择器的元素，且这个元素是其父节点的最后一个子元素。兼容性IE9+
29. **X:only-child**：伪类，选择满足X选择器的元素，且这个元素是其父元素的唯一子元素。兼容性IE9+
30. **X:only-of-type**：伪类，选择X选择的元素，**解析得到元素标签**，如果该元素没有相同类型的兄弟节点时选中它。兼容性IE9+
31. **X:first-of-type**：伪类，选择X选择的元素，**解析得到元素标签**，如果该元素 是此此类型元素的第一个兄弟。选中它。兼容性IE9+

---

#### **获得一个DOM元素的绝对位置**

[offsetTop](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetTop)：返回当前元素相对于其 [offsetParent](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) 元素的顶部的距离

[offsetLeft](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetLeft)：返回当前元素相对于其 [offsetParent](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/offsetParent) 元素的左边的距离

[getBoundingClientRect()](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getBoundingClientRect)：返回值是一个[DOMRect](https://developer.mozilla.org/zh-CN/docs/Mozilla/Tech/XPCOM/Reference/Interface/nsIDOMClientRect)对象，它包含了一组用于描述边框的只读属性——left、top、right和bottom，属性单位为像素

参考《[JavaScript中尺寸、坐标](http://www.cnblogs.com/strick/p/4826273.html)》，[查看在线代码](http://codepen.io/strick/pen/XmQaaX)。

---

#### 如何利用JS生成一个table

首先是用[createElement](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createElement)创建一个table，再用[setAttribute](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/setAttribute)设置table的属性，

然后用for循环设置tr和td的内容，用[appendChild](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/appendChild)拼接内容，设置td的时候还用到[innerHTML](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML)和[style](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference).padding。

[查看在线代码](http://codepen.io/strick/pen/wKZqpR)。参考《[JavaScript要点归档：DOM表格](http://myweb.jowai.info/javascript-main-points-archive-dom-table/)》《[JavaScript要点归档：DOM](http://myweb.jowai.info/javascript-main-points-archive-dom/)》

---

#### 实现预加载一张图片，加载完成后显示在网页中并设定其高度为50px，宽度为50px

先new [Image](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLImageElement/Image)()获取一个图片对象，然后在图片对象的onload中设置宽度和高度。[查看在线代码](http://codepen.io/strick/pen/vNMJVr)。

#### 假设有一个4行tr的table，将table里面tr顺序颠倒

先是通过table.tBodies[0].rows获取到当前tbody中的行，接下来是两种方法处理。获取到的行没有[reverse](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)这个方法。

第一种是将这些行push到另外一个数组中

第二种是用Array.prototype.[slice](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/slice).call()将那些行变成数组，

接着用reverse倒叙，table再appendChild。[查看在线代码](http://codepen.io/strick/pen/VvNzqX)。

这里我有个疑问，就是在appendChild的时候，并不是在最后把列加上，而是做了替换操作？

#### 模拟一个HashTable类，一个类上注册四个方法：包含有add、remove、contains、length方法

先是在构造函数中定义一个数组，然后用push模拟add，splice模拟remove。

四个方法都放在了[prototype](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/prototype)上面。[查看在线代码](http://codepen.io/strick/pen/VvNBom)。

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