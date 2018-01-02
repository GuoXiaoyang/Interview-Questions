## 其他问题

#### `git pull`与`git fetch`的区别



#### `git rebase`和`git merge`的区别

merge操作会生成一个新的节点，之前的提交分开显示。而rebase操作不会生成新的节点，是将两个分支融合成一个线性的提交。

---

#### Webpack原理及使用

* loader和plugin区别

  loader用于加载某些资源文件，因为webpack本身只能打包CommonJS规范的js文件，对于其他资源，例如css，图片等，是没有办法加载的，这就需要对应的loader将资源转换 plugin用于扩展webpack的功能，直接作用于webpack，loader只专注于转换文件，而plugin不仅局限于资源加载

  loader只能处理单一类型文件的输入输出，而Plugin则可以对整个打包过程获得更多的灵活性，譬如 ExtractTextPlugin，它可以将所有文件中的css剥离到一个独立的文件中，这样样式就不会随着组件加载而加载了。

* 什么是chunk

  Webpack提供一个功能可以拆分模块，每一个模块称为chunk，这个功能叫做Code Splitting。你可以在你的代码库中定义分割点，调用require.ensure，实现按需加载 

* 如何开发一个loader，原理是啥

  A loader is a node module exporting a function.

  缓存： Webpack Loader 同样可以利用缓存来提高效率，并且只需在一个可缓存的 Loader 上加一句 this.cacheable() 异步：在一个异步的模块中，回传时需要调用 Loader API 提供的回调方法 this.async()

* 打包原理

  webpack打包，最基本的实现方式，是将所有的模块代码放到一个数组里，通过数组ID来引用不同的模块

  ```javascript
  /************************************************************************/
  /******/ ([
    /* 0 */
    /***/ function(module, exports, __webpack_require__) {
      __webpack_require__(1);
      __webpack_require__(2);
      console.log('Hello, world!');
      /***/ },
    /* 1 */
    /***/ function(module, exports) {
      var a = 'a.js';
      console.log("I'm a.js");
      /***/ },
    /* 2 */
    /***/ function(module, exports) {
      var b = 'b.js';
      console.log("I'm b.js");
      /***/ }
    /******/ ]);
  ```

  可以发现入口entry.js的代码是放在数组索引0的位置，其它a.js和b.js的代码分别放在了数组索引1和2的位置，而webpack引用的时候，主要通过`__webpack_require__`的方法引用不同索引的模块。

* webpack和gulp的区别

  webpack是一种模块化打包工具，主要用于模块化方案，预编译模块的方案；gulp是工具链、构建工具，可以配合各种插件做js压缩，css压缩，less编译 替代手工实现自动化工作。

  Grunt/Gulp更多的是一种工作流；提供集成所有服务的一站式平台； gulp可以用来优化前端工作流程。

* 如何写一个plugin

  Compiler在开始打包时就进行实例化，实例对象里面装着与打包相关的环境和参数，包括options、plugins和loaders等。

  compilation对象，它继承于compiler，所以能拿到一切compiler的内容。Compilation表示有关模块资源，已编译资源，Compilation在每次文件变化重新打包时都进行一次实例化

  apply方法：当安装这个插件的时候，这个apply方法就会被webpack compiler调用。

  ```javascript
  function HelloWorldPlugin(options) {
    // Setup the plugin instance with options...
  }
  HelloWorldPlugin.prototype.apply = function(compiler) {
    compiler.plugin('done', function() {
      console.log('Hello World!');
    });
  };
  module.exports = HelloWorldPlugin;
  ```

* webpack打包后文件体积过大怎么办？

  很多方法：异步加载模块（代码分割）；提取第三方库（使用cdn或者vender）；代码压缩；去除不必要的插件；去除devtool选项，dllplugin等等。

---

#### 会话跟踪

#### web开发中会话跟踪的方法有哪些

- cookie
- session
- url重写
- 隐藏input
- ip地址

#### 什么是web语义化,有什么好处

- 用正确的标签做正确的事情。
- html语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析;
- 即使在没有样式CSS情况下也以一种文档格式显示，并且是容易阅读的;
- 搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，利于SEO;
- 使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。

---

### 工程问题

#### 应用开发的整体步骤

1. 需求第一
2. 根据需求确定框架与架构
3. 选择开发语言，构建工具
4. 享受开发自动构建带来的便利，快速做好一个不包含逻辑的应用
5. 写公用的 UI 库，Util 库，Component 业务逻辑，单元测试
6. 优化性能与体验
7. 编译，打包，静态文件部署，上内线
8. 内线测试，修复 Bug
9. 正式上外线

接下来的内容，根据整理的步骤来展开

#### 需求与框架选择

首先并不是所有的网站都适合做成单页应用的形式，侧重交互的网站更适合，比如：

- 公司内部的 ERP，HR，金融系统等软件
- Bug 追踪，TODO 管理等需要用户长时间停留的软件
- 最常见的是信息和商业系统，这些系统的用户经常在页面上停留数个小时
- 所有 Hybird App
- 侧重交互或反馈要求及时

#### 首屏优化

再回到前端渲染遇到首屏渲染问题，除了同构就没有其它解法了吗？总结以下可以通过以下三步解决

分拆打包 现在流行的路由库如 react-router 对分拆打包都有很好的支持。可以按照页面对包进行分拆，并在页面切换时加上一些 loading 和 transition 效果。

1.首屏内容最好做到静态缓存 2.首屏内联css渲染 3.图片懒加载 4.服务端渲染，首屏渲染速度更快（重点），无需等待js文件下载执行的过程 5.交互优化（使用加载占位器，在白屏无法避免的时候，为了解决等待加载过程中白屏或者界面闪烁） 6.图片尺寸大小控制

#### 前端渲染的优势

- 局部刷新。无需每次都进行完整页面请求
- 懒加载。如在页面初始时只加载可视区域内的数据，滚动后rp加载其它数据，可以通过 react-lazyload 实现
- 富交互。使用 JS 实现各种酷炫效果
- 节约服务器成本。省电省钱，JS 支持 CDN 部署，且部署极其简单，只需要服务器支持静态文件即可
- 天生的关注分离设计。服务器来访问数据库提供接口，JS 只关注数据获取和展现
- JS 一次学习，到处使用。可以用来开发 Web、Serve、Mobile、Desktop 类型的应用

#### 服务端渲染的优势

- 更好的 SEO，由于搜索引擎爬虫抓取工具可以直接查看完全渲染的页面。
- 服务端渲染不需要先下载一堆 js 和 css 后才能看到页面（首屏性能）
- 服务端渲染不用关心浏览器兼容性问题（随意浏览器发展，这个优点逐渐消失）
- 对于电量不给力的手机或平板，减少在客户端的电量消耗很重要

#### 谈谈你对组件的看法

一个组件应该有以下特征：

- 可组合（Composeable）：一个组件易于和其它组件一起使用，或者嵌套在另一个组件内部。如果一个组件内部创建了另一个组件，那么说父组件拥有（own）它创建的子组件，通过这个特性，一个复杂的 UI 可以拆分成多个简单的 UI 组件；
- 可重用（Reusable）：每个组件都是具有独立功能的，它可以被使用在多个 UI 场景；
- 可维护（Maintainable）：每个小的组件仅仅包含自身的逻辑，更容易被理解和维护；
- 可测试（Testable）：因为每个组件都是独立的，那么对于各个组件分别测试显然要比对于整个 UI 进行测试容易的多。

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

#### 雪碧图的自动化方案

---

#### 编码常识：文件编码、URL 编码、Unicode编码 什么含义。一个gbk编码的页面如何正确引用一个utf8的的资源





---

#### Docker是什么？原理



#### 适用场景



---

### 正则

#### 正则匹配邮箱



### 正则匹配css hex color

