## 网络相关问题
#### 为什么传统上利用多个域名来提供网站资源会更有效？

浏览器对单个域名请求对资源数量做了限制。

---
#### 请尽可能完整得描述从输入 URL 到整个网页加载完毕及显示在屏幕上的整个流程。

很经典的面试题目。

详细版：
1. 浏览器会开启一个线程来处理这个请求，对 URL 分析判断如果是 http 协议就按照 Web 方式来处理;
2. 调用浏览器内核中的对应方法，比如 WebView 中的 loadUrl 方法;
3. 通过DNS解析获取网址的IP地址，设置 UA 等信息发出第二个GET请求;
4. 进行HTTP协议会话，客户端发送报头(请求报头);
5. 进入到web服务器上的 Web Server，如 Apache、Tomcat、Node.JS 等服务器;
6. 进入部署好的后端应用，如 PHP、Java、JavaScript、Python 等，找到对应的请求处理;
7. 处理结束回馈报头，此处如果浏览器访问过，缓存上有对应资源，会与服务器最后修改时间对比，一致则返回304;
8. 浏览器开始下载html文档(响应报头，状态码200)，同时使用缓存;
9. 文档树建立，根据标记请求所需指定MIME类型的文件（比如css、js）,同时设置了cookie;
10. 页面开始渲染DOM，JS根据DOM API操作DOM,执行事件绑定等，页面显示完成。

详细版2:

HTTP是一个基于请求与响应，无状态的，应用层的协议，常基于TCP/IP协议传输数据。

1.域名解析，查找缓存

- 查找浏览器缓存（DNS缓存）
- 查找操作系统缓存（如果浏览器缓存没有，浏览器会从hosts文件查找是否有DNS信息）
- 查找路由器缓存
- 查找ISP缓存

2.浏览器获得对应的ip地址后，浏览器与远程`Web`服务器通过`TCP`三次握手协商来建立一个`TCP/IP`连接。 3.TCP/IP连接建立起来后，浏览器就可以向服务器发送HTTP请求 4.服务器处理请求，返回资源（MVC设计模式） 5.浏览器处理（加载，解析，渲染）

- HTML页面加载顺序从上而下
- 解析文档为有意义的结构，DOM树；解析css文件为样式表对象
- 渲染。将DOM树进行可视化表示

6.绘制网页

- 浏览器根据HTML和CSS计算得到渲染数，最终绘制到屏幕上

一个完整HTTP请求的过程为： DNS Resolving -> TCP handshake -> HTTP Request -> Server -> HTTP Response -> TCP shutdown



简洁版：

1. 浏览器根据请求的URL交给DNS域名解析，找到真实IP，向服务器发起请求；
2. 服务器交给后台处理完成后返回数据，浏览器接收文件（HTML、JS、CSS、图象等）；
3. 浏览器对加载到的资源（HTML、JS、CSS等）进行语法解析，建立相应的内部数据结构（如HTML的DOM）；
4. 载入解析到的资源文件，渲染页面，完成。

终极版

1. 在浏览器地址栏输入URL
2. 浏览器查看缓存，如果请求资源在缓存中并且新鲜，跳转到转码步骤
   1. 如果资源未缓存，发起新请求
   2. 如果已缓存，检验是否足够新鲜，足够新鲜直接提供给客户端，否则与服务器进行验证。
   3. 检验新鲜通常有两个HTTP头进行控制`Expires` 和 `Cache-Control`：
      - HTTP1.0提供Expires，值为一个绝对时间表示缓存新鲜日期
      - HTTP1.1增加了Cache-Control: max-age=,值为以秒为单位的最大新鲜时间
3. 浏览器**解析URL**获取协议，主机，端口，path
4. 浏览器**组装一个HTTP（GET）请求报文**
5. 浏览器
   获取主机ip地址，过程如下：
   1. 浏览器缓存
   2. 本机缓存
   3. hosts文件
   4. 路由器缓存
   5. ISP DNS缓存
   6. DNS递归查询（可能存在负载均衡导致每次IP不一样）
6. 打开一个socket与目标IP地址，端口建立TCP链接，三次握手如下：

   1. 客户端发送一个TCP的**SYN=1，Seq=X**的包到服务器端口
   2. 服务器发回**SYN=1， ACK=X+1， Seq=Y**的响应包
   3. 客户端发送**ACK=Y+1， Seq=Z**

7. TCP链接建立后**发送HTTP请求**
8. 服务器接受请求并解析，将请求转发到服务程序，如虚拟主机使用HTTP Host头部判断请求的服务程序
9. 服务器检查**HTTP请求头是否包含缓存验证信息**如果验证缓存新鲜，返回**304**等对应状态码
10. 处理程序读取完整请求并准备HTTP响应，可能需要查询数据库等操作
11. 服务器将**响应报文通过TCP连接发送回浏览器**
12. 浏览器接收HTTP响应，然后根据情况选择关闭TCP连接或者保留重用，关闭TCP连接的四次挥手如下：
    1. 主动方发送**Fin=1， Ack=Z， Seq= X**报文
    2. 被动方发送**ACK=X+1， Seq=Z**报文
    3. 被动方发送**Fin=1， ACK=X， Seq=Y**报文
    4. 主动方发送**ACK=Y， Seq=X**报文
13. 浏览器检查响应状态吗：是否为1XX，3XX， 4XX， 5XX，这些情况处理与2XX不同
14. 如果资源可缓存，**进行缓存**
15. 对响应进行**解码**（例如gzip压缩）
16. 根据资源类型决定如何处理（假设资源为HTML文档）
17. **解析HTML文档，构件DOM树，下载资源，构造CSSOM树，执行js脚本**，这些操作没有严格的先后顺序，以下分别解释
18. 构建DOM树：
    1. **Tokenizing**：根据HTML规范将字符流解析为标记
    2. **Lexing**：词法分析将标记转换为对象并定义属性和规则
    3. **DOM construction**：根据HTML标记关系将对象组成DOM树
19. 解析过程中遇到图片、样式表、js文件，**启动下载**
20. 构建CSSOM树：
    1. **Tokenizing**：字符流转换为标记流
    2. **Node**：根据标记创建节点
    3. **CSSOM**：节点创建CSSOM树
21. [根据DOM树和CSSOM树构建渲染树](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/render-tree-construction):
    1. 从DOM树的根节点遍历所有**可见节点**，不可见节点包括：1）`script`,`meta`这样本身不可见的标签。2)被css隐藏的节点，如`display: none`
    2. 对每一个可见节点，找到恰当的CSSOM规则并应用
    3. 发布可视节点的内容和计算样式
22. js解析如下：
    1. 浏览器创建Document对象并解析HTML，将解析到的元素和文本节点添加到文档中，此时**document.readystate为loading**
    2. HTML解析器遇到**没有async和defer的script时**，将他们添加到文档中，然后执行行内或外部脚本。这些脚本会同步执行，并且在脚本下载和执行时解析器会暂停。这样就可以用document.write()把文本插入到输入流中。**同步脚本经常简单定义函数和注册事件处理程序，他们可以遍历和操作script和他们之前的文档内容**
    3. 当解析器遇到设置了**async**属性的script时，开始下载脚本并继续解析文档。脚本会在它**下载完成后尽快执行**，但是**解析器不会停下来等它下载**。异步脚本**禁止使用document.write()**，它们可以访问自己script和之前的文档元素
    4. 当文档完成解析，document.readState变成interactive
    5. 所有**defer**脚本会**按照在文档出现的顺序执行**，延迟脚本**能访问完整文档树**，禁止使用document.write()
    6. 浏览器**在Document对象上触发DOMContentLoaded事件**
    7. 此时文档完全解析完成，浏览器可能还在等待如图片等内容加载，等这些**内容完成载入并且所有异步脚本完成载入和执行**，document.readState变为complete,window触发load事件
23. **显示页面**（HTML解析过程中会逐步显示页面）

---
#### Long-Polling、Websockets 和 Server-Sent Event 之间有什么区别？



---
#### HTTP缓存相关字段

* If-Modified-*

* Etag

  当发送一个服务器请求时，浏览器首先会进行缓存过期判断。浏览器根据缓存过期时间判断缓存文件是否过期。

  情景一：若没有过期，则不向服务器发送请求，直接使用缓存中的结果，此时我们在浏览器控制台中可以看到 `200 OK`(from cache) ，此时的情况就是完全使用缓存，浏览器和服务器没有任何交互的。

  情景二：若已过期，则向服务器发送请求，此时请求中会带上①中设置的文件修改时间，和`Etag`

  然后，进行资源更新判断。服务器根据浏览器传过来的文件修改时间，判断自浏览器上一次请求之后，文件是不是没有被修改过；根据`Etag`，判断文件内容自上一次请求之后，有没有发生变化

  情形一：若两种判断的结论都是文件没有被修改过，则服务器就不给浏览器发`index.html`的内容了，直接告诉它，文件没有被修改过，你用你那边的缓存吧—— `304 Not Modified`，此时浏览器就会从本地缓存中获取`index.html`的内容。此时的情况叫协议缓存，浏览器和服务器之间有一次请求交互。

  情形二：若修改时间和文件内容判断有任意一个没有通过，则服务器会受理此次请求，之后的操作同①

  ① 只有get请求会被缓存，post请求不会

  `Etag`由服务器端生成，客户端通过`If-Match`或者说`If-None-Match`这个条件判断请求来验证资源是否修改。常见的是使用`If-None-Match`。请求一个文件的流程可能如下：

  ====第一次请求===

  ```
  1.客户端发起 HTTP GET 请求一个文件；
  2.服务器处理请求，返回文件内容和一堆Header，当然包括Etag(例如"2e681a-6-5d044840")(假设服务器支持Etag生成和已经开启了Etag).状态码200
  ```

  ====第二次请求===

  ```
  客户端发起 HTTP GET 请求一个文件，注意这个时候客户端同时发送一个If-None-Match头，这个头的内容就是第一次请求时服务器返回的Etag：2e681a-6-5d0448402.服务器判断发送过来的Etag和计算出来的Etag匹配，因此If-None-Match为False，不返回200，返回304，客户端继续使用本地缓存；流程很简单，问题是，如果服务器又设置了Cache-Control:max-age和Expires呢，怎么办

  ```

  答案是同时使用，也就是说在完全匹配`If-Modified-Since`和`If-None-Match`即检查完修改时间和`Etag`之后，

  服务器才能返回304.(不要陷入到底使用谁的问题怪圈)

  为什么使用Etag请求头?

  +

  Etag 主要为了解决 `Last-Modified` 无法解决的一些问题。

* Do Not Track


* Expires 头部字段提供一个日期和时间，响应在该日期和时间后被认为失效。允许客户端在这个时间之前不去检查（发请求），等同max-age的效果。但是如果同时存在，则被Cache-Control的max-age覆盖。

  Expires = "Expires" ":" HTTP-date
  例如 Expires: Thu, 01 Dec 1994 16:00:00 GMT （必须是GMT格式）

    如果把它设置为-1，则表示立即过期

* Cache-Control

  网页的缓存是由HTTP消息头中的“Cache-control”来控制的，常见的取值有private、no-cache、max-age、must-revalidate等，默认为private。`Expires`要求客户端和服务端的时钟严格同步。`HTTP1.1`引入`Cache-Control`来克服Expires头的限制。如果max-age和Expires同时出现，则max-age有更高的优先级。

  Expires和max-age都可以用来指定文档的过期时间，但是二者有一些细微差别

      1. Expires在HTTP/1.0中已经定义，Cache-Control:max-age在HTTP/1.1中才有定义，为了向下兼容，仅使用max-age不够；
      2. Expires指定一个绝对的过期时间(GMT格式),这么做会导致至少2个问题：1)客户端和服务器时间不同步导致Expires的配置出现问题。 2）很容易在配置后忘记具体的过期时间，导致过期来临出现浪涌现象；
      3. max-age 指定的是从文档被访问后的存活时间，这个时间是个相对值(比如:3600s),相对的是文档第一次被请求时服务器记录的Request_time(请求时间)
      4. Expires指定的时间可以是相对文件的最后访问时间(Atime)或者修改时间(MTime),而max-age相对对的是文档的请求时间(Atime)
       如果值为no-cache,那么每次都会访问服务器。如果值为max-age,则在过期之前不会重复访问服务器。

* Transfer-Encoding
* X-Frame-Options


---

#### 什么是 HTTP method？请罗列出你所知道的所有 HTTP method，并给出解释。

1. 一台服务器要与HTTP1.1兼容，只要为资源实现**GET**和**HEAD**方法即可
2. **GET**是最常用的方法，通常用于**请求服务器发送某个资源**。
3. **HEAD**与GET类似，但**服务器在响应中值返回首部，不返回实体的主体部分**
4. **PUT**让服务器**用请求的主体部分来创建一个由所请求的URL命名的新文档，或者，如果那个URL已经存在的话，就用干这个主体替代它**
5. **POST**起初是用来向服务器输入数据的。实际上，通常会用它来支持HTML的表单。表单中填好的数据通常会被送给服务器，然后由服务器将其发送到要去的地方。
6. **TRACE**会在目的服务器端发起一个环回诊断，最后一站的服务器会弹回一个TRACE响应并在响应主体中携带它收到的原始请求报文。TRACE方法主要用于诊断，用于验证请求是否如愿穿过了请求/响应链。
7. **OPTIONS**方法请求web服务器告知其支持的各种功能。可以查询服务器支持哪些方法或者对某些特殊资源支持哪些方法。
8. **DELETE**请求服务器删除请求URL指定的资源

#### GET,POST,PUT,DELETE

1. GET请求会向数据库获取信息，只是用来查询数据，不会修改，增加数据。使用URL传递参数，对所发送的数量有限制，一般在2000字符
2. POST向服务器发送数据，会改变数据的种类等资源，就像insert操作一样，会创建新的内容，大小一般没有限制，POST安全性高，POST不会被缓存
3. PUT请求就像数据库的update操作一样，用来修改数据内容，不会增加数据种类
4. Delete用来删除操作

#### GET和POST的区别

1. GET使用URL或Cookie传参，而POST将数据放在BODY中，这个是因为HTTP协议用法的约定。并非它们的本身区别。
2. GET方式提交的数据有长度限制，则POST的数据则可以非常大，这个是因为它们使用的操作系统和浏览器设置的不同引起的区别。也不是GET和POST本身的区别。
3. POST比GET安全，因为数据在地址栏上不可见，这个说法没毛病，但依然不是GET和POST本身的区别。

GET和POST最大的区别主要是GET请求是幂等性的，POST请求不是。（幂等性：对同一URL的多个请求应该返回同样的结果。）因为get请求是幂等的，在网络不好的隧道中会尝试重试。如果用get请求增数据，会有重复操作的风险，而这种重复操作可能会导致副作用

---

#### Web应用从服务器主动推送Data到客户端有哪些方式

 html5提供的Websocket
  不可见的iframe
  WebSocket通过Flash
  XHR长时间连接
  XHR Multipart Streaming
  `<script>`标签的长时间连接(可跨域)

---

#### 如何实现浏览器内多个标签页之间的通信

* 同域，websocket 通过服务器作为中介进行通信
* 同域，localStorage／cookie本地存储数据来通信
* 同域，SharedWorker
* 跨域，window.postMessage()

---

#### webSocket如何兼容低浏览器

-   Adobe Flash Socket 
-   ActiveX HTMLFile (IE) 
-   基于 multipart 编码发送 XHR 
-   基于长轮询的 XHR

---

#### Web Worker 

#### worker主线程:

```
1.通过 worker = new Worker( url ) 加载一个JS文件来创建一个worker，同时返回一个worker实例。
2.通过worker.postMessage( data ) 方法来向worker发送数据。
3.绑定worker.onmessage方法来接收worker发送过来的数据。
4.可以使用 worker.terminate() 来终止一个worker的执行。
```

---

#### WebSocket

`WebSocket`是`Web`应用程序的传输协议，它提供了双向的，按序到达的数据流。他是一个`HTML5`协议，`WebSocket`的连接是持久的，他通过在客户端和服务器之间保持双工连接，服务器的更新可以被及时推送给客户端，而不需要客户端以一定时间间隔去轮询。

WebSocket 使用ws或wss协议，是一个持久化的协议，相对于HTTP这种非持久的协议来说。WebSocket API最伟大之处在于服务器和客户端可以在给定的时间范围内的任意时刻，相互推送信息。WebSocket并不限于以Ajax(或XHR)方式通信，因为Ajax技术需要客户端发起请求，而WebSocket服务器和客户端可以彼此相互推送信息；XHR受到域的限制，而WebSocket允许跨域通信。

```Javascript
// 创建一个Socket实例
var socket = new WebSocket('ws://localhost:8080');
// 打开Socket
socket.onopen = function(event) {
  // 发送一个初始化消息
  socket.send('I am the client and I\'m listening!');
  // 监听消息
  socket.onmessage = function(event) {
    console.log('Client received a message',event);
  };
  // 监听Socket的关闭
  socket.onclose = function(event) {
    console.log('Client notified socket has closed',event);
  };
  // 关闭Socket....
  //socket.close()
};
```



---

- [`[Doc\]` Net (网络)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#net)
- [`[Doc\]` UDP/Datagram](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#udp)
- [`[Doc\]` HTTP](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#http)
- [`[Doc\]` DNS (域名服务器)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#dns)
- [`[Doc\]` ZLIB (压缩)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#zlib)
- [`[Point\]` RPC](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#rpc)


- cookie 与 session 的区别? 服务端如何清除 cookie? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#q-cookie-session)


---

#### 什么是Cookie 隔离？（或者说：请求资源的时候不要让它带cookie怎么做）

  如果静态文件都放在主域名下，那静态文件请求的时候都带有的cookie的数据提交给server的，非常浪费流量，
  所以不如隔离开。

  因为cookie有域的限制，因此不能跨域提交请求，故使用非主要域名的时候，请求头中就不会带有cookie数据，
  这样可以降低请求头的大小，降低请求时间，从而达到降低整体请求延时的目的。

  同时这种方式不会将cookie传入Web Server，也减少了Web Server对cookie的处理分析环节，
  提高了webserver的http请求的解析速度。

---

- HTTP 协议中的 POST 和 PUT 有什么区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#q-post-put)
- 什么是跨域请求? 如何允许跨域? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#q-cors)
- TCP/UDP 的区别? TCP 粘包是怎么回事，如何处理? UDP 有粘包吗? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#q-tcp-udp)
- `TIME_WAIT` 是什么情况? 出现过多的 `TIME_WAIT` 可能是什么原因? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#q-time-wait)
- ECONNRESET 是什么错误? 如何复现这个错误?
- socket hang up 是什么意思? 可能在什么情况下出现? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#socket-hang-up)
- hosts 文件是什么? 什么叫 DNS 本地解析?
- 列举几个提高网络传输速度的办法?


---

- [`[Doc\]` TTY](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#tty)
- [`[Doc\]` OS (操作系统)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#os-1)
- [`[Doc\]` Path](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#path)
- [`[Doc\]` 命令行参数](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#%E5%91%BD%E4%BB%A4%E8%A1%8C%E5%8F%82%E6%95%B0)
- [`[Basic\]` 负载](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#%E8%B4%9F%E8%BD%BD)
- [`[Point\]` CheckList](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#checklist)

**常见问题**

- 什么是 TTY? 如何判断是否处于 TTY 环境? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#tty)
- 不同操作系统的换行符 (EOL) 有什么区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#os)
- 服务器负载是什么概念? 如何查看负载? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/os.md#%E8%B4%9F%E8%BD%BD)
- ulimit 是用来干什么的? [[more\]


---

#### HTTP和HTTPS

`HTTP`协议通常承载于TCP协议之上，有时也承载于`TLS`或`SSL`协议层之上，这个时候，就成了我们常说的HTTPS。

默认HTTP的端口号为80，`HTTPS`的端口号为443。

### 为什么`HTTPS`安全

因为网络请求需要中间有很多的服务器路由器的转发。中间的节点都可能篡改信息，而如果使用`HTTPS`，密钥在你和终点站才有。`https`之所以比`http`安全，是因为他利用`ssl/tls`协议传输。它包含证书，卸载，流量转发，负载均衡，页面适配，浏览器适配，refer传递等。保障了传输过程的安全性

#### 关于Http 2.0 你知道多少？

`HTTP/2`引入了“服务端推（server push）”的概念，它允许服务端在客户端需要数据之前就主动地将数据发送到客户端缓存中，从而提高性能。

`HTTP/2`提供更多的加密支持

`HTTP/2`使用多路技术，允许多个消息在一个连接上同时交差。

它增加了头压缩（header compression），因此即使非常小的请求，其请求和响应的`header`都只会占用很小比例的带宽。

#### http2.0和https

与HTTP/1相比，主要区别包括

- HTTP/2采用二进制格式而非文本格式（二进制协议解析起来更高效）
- HTTP/2是完全多路复用的，即一个TCP连接上同时跑多个HTTP请求
- 使用报头压缩，HTTP/2降低了开销
- HTTP/2让服务器可以将响应主动“推送”到客户端缓存中，支持服务端推送（就是服务器可以对一个客户端请求发送多个响应）

HTTPS协议是由SSL+HTTP协议构建的可进行加密传输、身份认证的网络协议，TLS/SSL中使用 了非对称加密，对称加密以及HASH算法。比http协议安全。

- HTTPS的工作原理

HTTPS 在传输数据之前需要客户端（浏览器）与服务端（网站）之间进行一次握手，在握手过程中将确立双方加密传输数据的密码信息

- 什么是keep-alive模式 （持久连接，连接重用）

keep-alive使客户端到服务端的连接持久有效，当出现对服务器的后继请求时，keep-alive功能避免了建立或者重新连接

不需要重新建立tcp的三次握手，就是说不释放连接

http1.0默认关闭，http1.1默认启用

优点：更高效，性能更高。因为避免了建立/释放连接的开销

3.http1.0和http1.1区别：

- 缓存处理，在HTTP1.0中主要使用header里的If-Modified-Since，Expires来做为缓存判断的标准，HTTP1.1则引入更多缓存控制策略，例如Entity tag,If-Match,If-None-Match等
- Http1.1支持长连接和请求的流水线（pipeline）处理，在一个TCP连接上可以传送多个HTTP请求和响应，减少了建立和关闭连接的消耗和延迟，默认开启Connection:keep-alive

---

#### HTTP request报文结构是怎样的

[rfc2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html)中进行了定义：

1. 首行是**Request-Line**包括：**请求方法**，**请求URI**，**协议版本**，**CRLF**
2. 首行之后是若干行**请求头**，包括**general-header**，**request-header**或者**entity-header**，每个一行以CRLF结束
3. 请求头和消息实体之间有一个**CRLF分隔**
4. 根据实际请求需要可能包含一个**消息实体** 一个请求报文例子如下：

```
GET /Protocols/rfc2616/rfc2616-sec5.html HTTP/1.1
Host: www.w3.org
Connection: keep-alive
Cache-Control: max-age=0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
User-Agent: Mozilla/5.0 (Windows NT 6.1; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/35.0.1916.153 Safari/537.36
Referer: https://www.google.com.hk/
Accept-Encoding: gzip,deflate,sdch
Accept-Language: zh-CN,zh;q=0.8,en;q=0.6
Cookie: authorstyle=yes
If-None-Match: "2cc8-3e3073913b100"
If-Modified-Since: Wed, 01 Sep 2004 13:24:52 GMT

name=qiu&age=25

```

---

#### HTTP response报文结构是怎样的

[rfc2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html)中进行了定义：

1. 首行是状态行包括：**HTTP版本，状态码，状态描述**，后面跟一个CRLF
2. 首行之后是**若干行响应头**，包括：**通用头部，响应头部，实体头部**
3. 响应头部和响应实体之间用**一个CRLF空行**分隔
4. 最后是一个可能的**消息实体** 响应报文例子如下：

```
HTTP/1.1 200 OK
Date: Tue, 08 Jul 2014 05:28:43 GMT
Server: Apache/2
Last-Modified: Wed, 01 Sep 2004 13:24:52 GMT
ETag: "40d7-3e3073913b100"
Accept-Ranges: bytes
Content-Length: 16599
Cache-Control: max-age=21600
Expires: Tue, 08 Jul 2014 11:28:43 GMT
P3P: policyref="http://www.w3.org/2001/05/P3P/p3p.xml"
Content-Type: text/html; charset=iso-8859-1

{"name": "qiu", "age": 25}
```

---

#### HTTP状态码及其含义

 简单版

100  Continue	继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
200  OK 		正常返回信息
201  Created  	请求成功并且服务器创建了新的资源
202  Accepted 	服务器已接受请求，但尚未处理
301  Moved Permanently  请求的网页已永久移动到新位置。
302 Found  		临时性重定向。
303 See Other  	临时性重定向，且总是使用 GET 请求新的 URI。
304  Not Modified 自从上次请求后，请求的网页未修改过。
400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
401 Unauthorized 请求未授权。
403 Forbidden  	禁止访问。
404 Not Found  	找不到如何与 URI 相匹配的资源。
500 Internal Server Error  最常见的服务器端错误。
503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。

 

完整版
1**(信息类)：表示接收到请求并且继续处理
100——客户必须继续发出请求
101——客户要求服务器根据请求转换HTTP协议版本

2**(响应成功)：表示动作被成功接收、理解和接受
200——表明该请求被成功地完成，所请求的资源发送回客户端
201——提示知道新文件的URL
202——接受和处理、但处理未完成
203——返回信息不确定或不完整
204——请求收到，但返回信息为空
205——服务器完成了请求，用户代理必须复位当前已经浏览过的文件
206——服务器已经完成了部分用户的GET请求

3**(重定向类)：为了完成指定的动作，必须接受进一步处理
300——请求的资源可在多处得到
301——本网页被永久性转移到另一个URL
302——请求的网页被转移到一个新的地址，但客户访问仍继续通过原始URL地址，重定向，新的URL会在response中的Location中返回，浏览器将会使用新的URL发出新的Request。
303——建议客户访问其他URL或访问方式
304——自从上次请求后，请求的网页未修改过，服务器返回此响应时，不会返回网页内容，代表上次的文档已经被缓存了，还可以继续使用
305——请求的资源必须从服务器指定的地址得到
306——前一版本HTTP中使用的代码，现行版本中不再使用
307——申明请求的资源临时性删除

4**(客户端错误类)：请求包含错误语法或不能正确执行
400——客户端请求有语法错误，不能被服务器所理解
401——请求未经授权，这个状态代码必须和WWW-Authenticate报头域一起使用
HTTP 401.1 - 未授权：登录失败
HTTP 401.2 - 未授权：服务器配置问题导致登录失败
HTTP 401.3 - ACL 禁止访问资源
HTTP 401.4 - 未授权：授权被筛选器拒绝
HTTP 401.5 - 未授权：ISAPI 或 CGI 授权失败
402——保留有效ChargeTo头响应
403——禁止访问，服务器收到请求，但是拒绝提供服务
HTTP 403.1 禁止访问：禁止可执行访问
HTTP 403.2 - 禁止访问：禁止读访问
HTTP 403.3 - 禁止访问：禁止写访问
HTTP 403.4 - 禁止访问：要求 SSL
HTTP 403.5 - 禁止访问：要求 SSL 128
HTTP 403.6 - 禁止访问：IP 地址被拒绝
HTTP 403.7 - 禁止访问：要求客户证书
HTTP 403.8 - 禁止访问：禁止站点访问
HTTP 403.9 - 禁止访问：连接的用户过多
HTTP 403.10 - 禁止访问：配置无效
HTTP 403.11 - 禁止访问：密码更改
HTTP 403.12 - 禁止访问：映射器拒绝访问
HTTP 403.13 - 禁止访问：客户证书已被吊销
HTTP 403.15 - 禁止访问：客户访问许可过多
HTTP 403.16 - 禁止访问：客户证书不可信或者无效
HTTP 403.17 - 禁止访问：客户证书已经到期或者尚未生效
404——一个404错误表明可连接服务器，但服务器无法取得所请求的网页，请求资源不存在。eg：输入了错误的URL
405——用户在Request-Line字段定义的方法不允许
406——根据用户发送的Accept拖，请求资源不可访问
407——类似401，用户必须首先在代理服务器上得到授权
408——客户端没有在用户指定的饿时间内完成请求
409——对当前资源状态，请求不能完成
410——服务器上不再有此资源且无进一步的参考地址
411——服务器拒绝用户定义的Content-Length属性请求
412——一个或多个请求头字段在当前请求中错误
413——请求的资源大于服务器允许的大小
414——请求的资源URL长于服务器允许的长度
415——请求资源不支持请求项目格式
416——请求中包含Range请求头字段，在当前请求资源范围内没有range指示值，请求也不包含If-Range请求头字段
417——服务器不满足请求Expect头字段指定的期望值，如果是代理服务器，可能是下一级服务器不能满足请求长。

5**(服务端错误类)：服务器不能正确执行一个正确的请求
HTTP 500 - 服务器遇到错误，无法完成请求
HTTP 500.100 - 内部服务器错误 - ASP 错误
HTTP 500-11 服务器关闭
HTTP 500-12 应用程序重新启动
HTTP 500-13 - 服务器太忙
HTTP 500-14 - 应用程序无效
HTTP 500-15 - 不允许请求 global.asa
Error 501 - 未实现
HTTP 502 - 网关错误
HTTP 503：由于超载或停机维护，服务器目前无法使用，一段时间后可能恢复正常

---

#### ​Restful

REST（Representational State Transfer） REST的意思是表征状态转移，是一种基于HTTP协议的网络应用接口风格，充分利用HTTP的方法实现统一风格接口的服务，HTTP定义了以下8种标准的方法：

GET：请求获取指定资源
HEAD：请求指定资源的响应头
PUT ：请求服务器存储一个资源 根据REST设计模式，这四种方法通常分别用于实现以下功能： GET（获取），POST（新增），PUT（更新），DELETE（删除）

---

#### 代理和反向代理

正向代理： 用浏览器访问时，被残忍的block，于是你可以在国外搭建一台代理服务器，让代理帮我去请求google.com，代理把请求返回的相应结构再返回给我。

反向代理：反向代理服务器会帮我们把请求转发到真实的服务器那里去。Nginx就是性能非常好的反向代理服务器，用来做负载均衡。 正向代理的对象是客户端，反向代理的对象是服务端

#### CDN工作原理

CDN做了两件事，一是让用户访问最近的节点，二是从缓存或者源站获取资源

CDN的工作原理：通过dns服务器来实现优质节点的选择，通过缓存来减少源站的压力。

#### 网络优化/性能优化

使用CDN，让用户访问最近的资源，减少来回传输时间 合并压缩CSS、js、图片、静态资源，服务器开启GZIP css放顶部，js放底部（css可以并行下载，而js加载之后会造成阻塞） 图片预加载和首屏图片之外的做懒加载 做HTTP缓存（添加Expires头和配置Etag）用户可以重复使用本地缓存，减少对服务器压力 大小超过 10KB 的 css/img 建议外联引用，以细化缓存粒度 小于 10k 的图片 base64 DNS 预解析 DNS-Prefetch 预连接 Preconnect

- 代码层面优化

少用全局变量，减少作用域链查找，缓存DOM查找结果，避免使用with（with会创建自己的作用域，会增加作用域链长度）；多个变量声明合并；减少DOM操作次数；尽量避免在HTML标签中写style属性

避免使用css3渐变阴影效果，尽量使用css3动画，开启硬件加速，不滥用float；避免使用CSS表达式；使用`<link>`来代替`@import`

- 图片预加载原理

提前加载图片，当用户需要查看时可直接从本地缓存中渲染

```
var imgArr=["1.jpg","2.jpg"];
loadImage(imgArr,callback);
function loadImage(imgArr, callback) {
    var imgNum=imgArr.length,count=0;
    for(var i=0;i<imgNum;i++){
      var img = new Image(); //创建一个Image对象，实现图片的预下载
      img.src = imgArr[i];
      if (img.complete) { // 如果图片已经存在于浏览器缓存，直接调用回调函数
          if(count==imgNum){
          callback();// 直接返回，不用再处理onload事件
         }
 } count++; img.onload=function () { if(count==imgNum){ callback(); } } }//for循环结束}
```

#### [说说TCP传输的三次握手四次挥手策略](http://hawx1993.github.io/Front-end-Interview-Questions/#/?id=%e8%af%b4%e8%af%b4tcp%e4%bc%a0%e8%be%93%e7%9a%84%e4%b8%89%e6%ac%a1%e6%8f%a1%e6%89%8b%e5%9b%9b%e6%ac%a1%e6%8c%a5%e6%89%8b%e7%ad%96%e7%95%a5)

为了准确无误地把数据送达目标处，TCP协议采用了三次握手策略。用TCP协议把数据包送出去后，TCP不会对传送 后的情况置之不理，它一定会向对方确认是否成功送达。握手过程中使用了TCP的标志：SYN和ACK

发送端首先发送一个带SYN标志的数据包给对方。接收端收到后，回传一个带有SYN/ACK标志的数据包以示传达确认信息。 最后，发送端再回传一个带ACK标志的数据包，代表“握手”结束。 若在握手过程中某个阶段莫名中断，TCP协议会再次以相同的顺序发送相同的数据包。

---

#### 参考

[^1]: https://github.com/qiu-deqing/FE-interview