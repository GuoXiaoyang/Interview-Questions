### BOM相关问题

#### HTML5新特性及API

新增了关于图像，位置，存储，多任务等API。

* 拖拽释放(Drag and drop) API   
* 语义化标签（`header,nav,footer,aside,article,section`）  
* 音频、视频API(audio,video)  
* 画布(Canvas) API  地理(Geolocation) API  
* 本地离线存储   localStorage 长期存储数据，浏览器关闭后数据不丢失；  sessionStorage 的数据在浏览器关闭后自动删除 
* 表单控件，calendar、date、time、email、url、search    
* 新的技术 webworker, websocket, Geolocation等



---

#### BOM中有哪些对象

问题比较大哈

---

#### 浏览器内核

- IE：Triden，Trident与W3C标准基本脱节，造成IE的兼容性难题。
- Firefox：Gecko，基本只用于Firefox浏览器
- Chrome/Opera：Blink内核，Blink是Google在Webkit基础上开发的开源内核，Chrome最初是基于Webkit的，后来越来越觉得Webkit过于保守，因此单独拉取了Blink分支。Blink在多进程架构，JavaScript引擎方面都与Webkit有比较大的差别。但二者都是遵守W3C标准的，因此开发者不用考虑二者兼容性问题。
- Safari：Webkit，苹果开发的浏览器内核，并于2005年开放源代码。Chrome的Blink也是基于Webkit开发的，差异性不是特别大，也可以认为是Webkit内核浏览器。
- 移动端：ios和Android 4.4以前平台使用Webkit内核，Android4.4及以后版本使用Blink。

---

#### 浏览器工作原理



---

#### 页面可见性（Page Visibility API） 可以有哪些用途？

  通过 visibilityState 的值检测页面当前是否可见，以及打开网页的时间等;
  在页面被切换到其他后台进程的时候，自动暂停音乐或视频的播放；



---

#### 参考

[^1]: https://github.com/markyun/My-blog/tree/master/Front-end-Developer-Questions/Questions-and-Answers

