### DOM相关问题

因为JS包含的内容很广泛，放在一个章节里肯定是不够的。所以单独罗列了DOM问题和BOM问题出来。

#### DOM元素的操作方法

1）创建新节点

createDocumentFragment()    //创建一个DOM片段

createElement()   //创建一个具体的元素

createTextNode()   //创建一个文本节点

2）添加、移除、替换、插入

appendChild()      //添加

removeChild()      //移除

replaceChild()      //替换

insertBefore()      //插入

3）查找

getElementsByTagName()    //通过标签名称

getElementsByName()     //通过元素的Name属性的值

getElementById()        //通过元素Id，唯一性

---

#### DOM元素的类型判断



---

#### 事件类型的种类



---

#### 移动端最小触控区域是多大？



------

#### 移动端的点击事件的有延迟，时间是多久，为什么会有？ 怎么解决这个延时？（click 有 300ms 延迟,为了实现safari的双击事件的设计，浏览器要知道你是不是要双击操作。）

---

1. Is there any difference between window and document?
2. Does document.onload and window.onload fire at the same time?
3. Is attribute similar to property?
4. What are the different ways to get an element from DOM?
5. What is the fastest way to select elements by using css selectors?
6. How come, I can't use forEach or similar array methods on a NodeList?
7. If you need to implement getElementByAttribute, how would you implement it?
8. How would you add a class to an element by query selector?
9. How could I verify whether one element is child of another?
10. What is the best way to create a DOM element? Set innherHTML or use createElement?
11. What is createDocumentFragment and why you might use it?
12. What is reflow? What causes reflow? How could you reduce reflow?
13. What is repaint and when does this happen?
14. How could you make sure to run some JavaScript when DOM is ready like $(document).ready?
15. What is event bubble? How does event flows in DOM?
16. How would you destroy multiple list items with one click handler?
17. Create a button that is destroyed by clicking in it but two new buttons are created in it's place.
18. How could you capture all clicks in a page?
19. How can you get all the texts in a web page?
20. What is defer and async keyword does in a script tag?
21. 10 rapid fire questions

---

### src与href的区别。

　　答案：

　　src用于替换当前元素，href用于在当前文档和引用资源之间确立联系。

　　src是source的缩写，指向外部资源的位置，指向的内容将会嵌入到文档中当前标签所在位置；在请求src资源时会将其指向的资源下载并应用到文档内，例如js脚本，img图片和frame等元素。

　　<script src ="js.js"></script>

　　当浏览器解析到该元素时，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等元素也如此，类似于将所指向资源嵌入当前标签内。这也是为什么将js脚本放在底部而不是头部。

 

　　href是Hypertext Reference的缩写，指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的链接，如果我们在文档中添加

　　<link href="common.css" rel="stylesheet"/>

　　那么浏览器会识别该文档为css文件，就会并行下载资源并且不会停止对当前文档的处理。这也是为什么建议使用link方式来加载css，而不是使用@import方式。

---

#### 下面这个ul，如何点击每一列的时候alert其index?

```HTML
<ul id=”test”>
  <li>这是第一条</li>
  <li>这是第二条</li>
  <li>这是第三条</li>
</ul>
```

---

编写一个JavaScript函数，输入指定类型的选择器(仅需支持id，class，tagName三种简单CSS选择器，无需兼容组合选择器)可以返回匹配的DOM节点，需考虑浏览器兼容性和性能。

```javascript
/** 
 @param selector {String} 传入的CSS选择器。 
 @return {Array}
*/
```

---

请评价以下代码并给出改进意见。

```javascript
if(window.addEventListener){
  var addListener = function(el,type,listener,useCapture){
    el.addEventListener(type,listener,useCapture);
  };
}
else if(document.all){
  addListener = function(el,type,listener){
    el.attachEvent("on"+type,function(){
      listener.apply(el);
    });
  }  
}
```

---

11.原生JS的window.onload与Jquery的$(document).ready(function(){})有什么不同？如何用原生JS实现Jq的ready方法？

window.onload()方法是必须等到页面内包括图片的所有元素加载完毕后才能执行。

$(document).ready()是DOM结构绘制完毕后就执行，不必等到加载完毕。



---

12.（设计题）想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）

回答出概念即可，下面是几个要点

1. 给需要拖拽的节点绑定mousedown, mousemove, mouseup事件
2. mousedown事件触发后，开始拖拽
3. mousemove时，需要通过event.clientX和clientY获取拖拽位置，并实时更新位置
4. mouseup时，拖拽结束
5. 需要注意浏览器边界的情况

---

4.说出以下函数的作用是？空白区域应该填写什么？

```javascript
//define
(function(window) {
  functionfn(str) {
    this.str = str;
  }
  fn.prototype.format = function() {
    vararg = __;
    return this.str.replace(__, function(a,b) {
      return arg[b] || "";
    });
  }
  window.fn = fn;
})(window);

//use
(function() {
  var t = newfn('<p>{1}<span>{2}</span></p>');
  console.log(t.format('http://www.alibaba.com', 'Alibaba', 'Welcome'));
})();
```

---

#### 请写一个简单的幻灯效果页面。

一开始想的方案是，数组存储图片的路径，点击切换`<img>`标签的`src`属性。但是这样用户切换图片时，会有加载延迟。所以比较好的方案应该是：

HTML中定义好slide列表，只有当前显示的图片有显示类：

```HTML
<div class="slide-page">
  <div class="left"></div>
  <div class="right"></div>
  <ul class="slide-list">
    <li class="slide"><img src="XX1"></li>
  	<li class="slide show"><img src="XX2"></li>
  	<li class="slide"><img src="XX3"></li> 
  </ul>
</div>
```

定义样式

```CSS
.slide-page {
  position:fixed;
  top: 0;
  left: 0;
}
.slide {
  display: none;
}
.show {
  display:block;
}
```

为slide框划分左／右方向键区域，分别绑定点击事件，移去当前图片显示类，即将显示的图片添加显示类，另外注意判别边界。

```javascript
const currentSlideIndex = 0;
left.addEventListener('click', () => {
  if(currentSlideIndex > 0) {
    slideList[currentSlideIndex].classList.remove('show');
    currentSlideIndex--;
    slideList[currentSlideIndex].classList.add('show');
  }
});
right.addEventListener('click', () => {
  if(currentSlideIndex < listLength-1) {
    slideList[currentSlideIndex].classList.remove('show');
    currentSlideIndex++;
    slideList[currentSlideIndex].classList.add('show');
  }
});
```

---

#### 评价以下代码并给出改进意见。

```javascript
if(window.addEventListener){
    var addListener = function(el,type,listener,useCapture){
        el.addEventListener(type,listener,useCapture);
  };
}
else if(document.all){
    addListener = function(el,type,listener){
        el.attachEvent("on"+type,function(){
          listener.apply(el);
      });
   }  
}
```



#### 延伸：写一个React轮播组件



---

#### 想实现一个对页面某个节点的拖曳？如何做？（使用原生JS）



---

#### Shadow DOM 是什么？

[ShadowDOM](https://developers.google.cn/web/fundamentals/web-components/shadowdom)主要解决一个文档中可能需要大量交互的多个DOM树建立和维护各自功能边界的问题。最大的用处应该是隔离外部环境用于封装组件。

目前浏览器没有完全支持

---

### 参考

[^1]: https://github.com/allenGKC/Front-end-Interview-questions
[^2]: 

