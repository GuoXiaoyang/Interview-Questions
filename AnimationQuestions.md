

### 动画相关问题

#### 请解释 CSS 动画和 JavaScript 动画的优缺点。

动画这块目前也写得很少，大概总结下：

- CSS动画

  优点在于使用GPU计算，不占用JS资源；缺点是相对难掌控，因为逻辑性要差一些，旧版浏览器不支持

- JavaScript动画

  与CSS动画相反，占用主线程资源但能够精细控制

---

用js来实现动画，我们一般是借助setTimeout或setInterval这两个函数，以及新的requestAnimationFrame

```javascript
<div id="demo" style="position:absolute; width:100px; height:100px; background:#ccc; left:0; top:0;"></div>

<script>
  var demo = document.getElementById('demo');
  function rander(){
    demo.style.left = parseInt(demo.style.left) + 1 + 'px'; //每一帧向右移动1px
  }
  requestAnimationFrame(function(){
    rander();
    //当超过300px后才停止
    if(parseInt(demo.style.left)<=300) requestAnimationFrame(arguments.callee);
  });
</script>
```

CSS3使用

- @keyframes 结合animation
- transition：property duration timing-function delay

---

#### 实现某个动画？

---

#### 视差滚动效果，如何给每页做不同的动画？（回到顶部，向下滑动要再次出现，和只出现一次分别怎么做？）



---

#### 如果需要手动写动画，你认为最小时间间隔是多久，为什么？（阿里）

 多数显示器默认频率是60Hz，即1秒刷新60次，所以理论上最小间隔为1/60＊1000ms ＝ 16.7ms

---

#### Web Animation 的十几年

在 2000 年，《[How Web Animation Works](http://computer.howstuffworks.com/web-animation.htm)》在谈 Gifs、 Flash、DHTML

在 2001 年，《[SMIL Animation](http://www.w3.org/TR/smil-animation/)》在谈 SVG 等标记语言

在 2009 年，CSS3 在谈动画类模块，HTML5[第一版已发布一年](https://www.pinterest.com/pin/425590233517909178/)

在 2015 年，可以[大胆的使用](http://caniuse.com/)各种动画实现技术

从[The Evolution of the Web](http://www.evolutionoftheweb.com/) 可以看出 2008 年开始 Web 发展非常快

---

#### 按应用场景对 Web Animation 进行分类

1. 静态与应用类：CSS3, DOM4
2. 三维系：WebGL API
3. 游戏类：Canvas API
4. 图表类：SVG，Canvas API

有一些会混合着用，不过本质上就是 CSS + JS

---

### 好食材需要好工具

1.[Tumult Hype 3](http://tumult.com/hype) 面向设计师的动画软甲，**强烈推荐**

2.[Adobe Edge Animate CC](https://www.adobe.com/products/edge-animate.html) 面向设计师的动画软件2，配合全家桶一起食用更好

3.[Animatron](https://www.animatron.com/) 在线制作动画，特长是 Banner

4.[Ceaser](http://matthewlein.com/ceaser/) CSS 贝塞尔曲线演示与调整

还有很多没什么◎用的工具就不放上了～

反正我是跪着看完了[Give ’n’ Go](http://give-n-go.co/)，怎样才能复制 Dribbble Shot？

。。。当然我觉得最主要的工具还是 GUI 代码编辑器

---

#### 教程

很多人推荐 W3C，但我很少看的，总觉得  W3C 会降低视力

推荐 [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS) 的人值得深交！！！

不太有印象看过那种醍醐灌顶的文章

所以就列一下网站首页，实践还是 Google 好

1.[CSS-Tricks](https://css-tricks.com/) 站如其名，善于使人变机智

2.[CSS: Animations with Val Head](http://www.lynda.com/CSS-tutorials/CSS-Animations/115434-2.html?utm_medium=ldc-partner&utm_source=SSPRC&utm_content=524&utm_campaign=CD14926&bid=524&aid=CD14926&opt=) Lynda 视频教程，感觉人跟头像对不上～

3.《[CSS animations](http://www.fivesimplesteps.com/products/css-animations)》 BY VAL HEAD 她还有一本小小书，可收藏她的[个人网站](http://valhead.com/)

4.《[Foundation HTML5 Animation with JavaScript](http://lamberta.github.io/html5-animation)》最优秀的 Canvas 动画书

还有 SitePoint，Tuts+，David Walsh，Impressive Webs 等都是很棒的综合文章网站

---

### 在动画库这块就多了，首先可以分为是否依赖 jQuery...

jQuery+生态圈，[Velocity.js](https://github.com/julianshapiro/velocity)，[Transit.js](https://github.com/rstacruz/jquery.transit/)，[GSAP.js](https://github.com/greensock/GreenSock-JS/)

以上几个库能满足绝大部分静态站动画需求的

有些时候需求很明确，如视差滚动，页面切换，图片变换，粒子特效，加载进度条等

这时就需要去找满足具体需求的动画库

[Awesomes](http://awesomes.cn/repos/Dom/animation) 整理得不错，自己也需要建一个分类来收藏

---

#### 代码片段与素材也好多的说，还是先挑几个好一点的

1.[Codepen](http://codepen.io/) 最佳最全的动画实例，[搜一次](http://codepen.io/search/pens?q=Doge&limit=all&type=type-pens)就停不下来

2.[CSSDesk](http://cssdeck.com/) 别被名字骗了，有些特效还是有 JS，可以当作 Codepen 的备胎～

3.[Envato](http://market.envato.com/) 各种收费网页素材，我就看看没带钱！

4.[Codrops](http://tympanus.net/codrops/) 颜值最高的成品素材



### 参考

[^1]: [Node.js面试问题及答案](https://sunebear.gitbooks.io/frontend-developer-interview-questions-and-answers/content/animation.html)
[^2]: https://sunebear.gitbooks.io/frontend-developer-interview-questions-and-answers/animation.html

