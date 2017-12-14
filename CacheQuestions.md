### 缓存相关问题

### 网络缓存

#### Cookie相关

```
Set-Cookie: value[; expires=date][; domain=domain][; path=path][; secure]
```

如果想让cookie存在一段时间，就要为expires属性设置为未来的一个用毫秒数表示的过期日期或时间点，expires默认为设置的expires的当前时间。现在已经被max-age属性所取代，max-age用秒来设置cookie的生存期。如果max-age为0，则表示删除该cookie。

cookie的属性：

- HttpOnly属性告之浏览器该 cookie 绝不能通过 JavaScript 的 `document.cookie` 属性访问。
- domain属性可以使多个web服务器共享cookie。
- 只有path属性匹配向服务器发送的路径，Cookie 才会发送。必须是绝对路径
- secure属性用来指定Cookie只能在加密协议HTTPS下发送到服务器。
- max-age属性用来指定Cookie有效期
- expires属性用于指定Cookie过期时间。它的格式采用Date.toUTCString()的格式。

浏览器的同源政策规定，两个网址只要域名相同和端口相同，就可以共享Cookie。

1. 浏览器输入 url 之后敲下回车，刷新 F5 与强制刷新(Ctrl + F5)，又有什么区别？

实际上浏览器输入 url 之后敲下回车就是先看本地 cache-control、expires 的情况，刷新(F5)就是忽略先看本地 cache-control、expires 的情况，带上条件 If-None-Match、If-Modified-Since，强制刷新(Ctrl + F5)就是不带条件的访问。

2. etag，cache-control，last-modified

如果比较粗的说先后顺序应该是这样：

- Cache-Control —— 请求服务器之前
- Expires —— 请求服务器之前
- If-None-Match (Etag) —— 请求服务器
- If-Modified-Since (Last-Modified) —— 请求服务器

需要注意的是 如果同时有 etag 和 last-modified 存在，在发送请求的时候会一次性的发送给服务器，没有优先级，服务器会比较这两个信息.

如果expires和cache-control:max-age同时存在，expires会被cache-control 覆盖。

其中Expires和cache-control属于强缓存，last-modified和etag属于协商缓存 强缓存与协商缓存区别：强缓存不发请求到服务器，协商缓存会发请求到服务器。

![img](images/cache.jpeg)



---

### 本地缓存

#### 缓存，存储相关（cookie，web storage和session）

cookie优点： 1.可以解决HTTP无状态的问题，与服务器进行交互 缺点： 1.数量和长度限制，每个域名最多20条，每个cookie长度不能超过4kb 2.安全性问题。容易被人拦截 3.浪费带宽，每次请求新页面，cookie都会被发送过去

#### cookie和session区别

1.cookie数据存放在客户的浏览器上，session数据放在服务器上。 2.session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能。考虑到减轻服务器性能方面，应当使用COOKIE。

sessionStorage是当前对话的缓存，浏览器窗口关闭即消失，localStorage持久存在，除非清除浏览器缓存。

#### sessionStorage,localStorage,cookie区别

1. 都会在浏览器端保存，有大小限制，同源限制
2. cookie会在请求时发送到服务器，作为会话标识，服务器可修改cookie；web storage不会发送到服务器
3. cookie有path概念，子路径可以访问父路径cookie，父路径不能访问子路径cookie
4. 有效期：cookie在设置的有效期内有效，默认为浏览器关闭；sessionStorage在窗口关闭前有效，localStorage长期有效，直到用户删除
5. 共享：sessionStorage不能共享，localStorage在同源文档之间共享，cookie在同源且符合path规则的文档之间共享
6. localStorage的修改会促发其他文档窗口的update事件
7. cookie有secure属性要求HTTPS传输
8. 浏览器不能保存超过300个cookie，单个服务器不能超过20个，每个cookie不能超过4k。web storage大小支持能达到5M

#### 请描述 cookies、sessionStorage 和 localStorage 的区别。

三者都是浏览器存储用户数据的方式。

- cookies

  大小有限，一般4kb；会过期；每次请求都会被客户端发送给服务端；偏向存储用户与服务端的会话数据

- sessionStorage

  存储空间2.5M+；浏览器不会主动将数据发送到服务端；只与当前页面相关，关闭页面则数据清除

- localStorage

  基本与sessionStorage一致，但数据与网站相关，同源页面共享数据，关闭页面不会清除数据



#### 客户端存储localStorage和sessionStorage

- localStorage有效期为永久，sessionStorage有效期为顶层窗口关闭前
- 同源文档可以读取并修改localStorage数据，sessionStorage只允许同一个窗口下的文档访问，如通过iframe引入的同源文档。
- Storage对象通常被当做普通javascript对象使用：**通过设置属性来存取字符串值**，也可以通过**setItem(key, value)设置**，**getItem(key)读取**，**removeItem(key)删除**，**clear()删除所有数据**，**length表示已存储的数据项数目**，**key(index)返回对应索引的key**

```
localStorage.setItem('x', 1); // storge x->1
localStorage.getItem('x); // return value of x

// 枚举所有存储的键值对
for (var i = 0, len = localStorage.length; i < len; ++i ) {
    var name = localStorage.key(i);
    var value = localStorage.getItem(name);
}

localStorage.removeItem('x'); // remove x
localStorage.clear();  // remove all data

```

------

#### 一次js请求一般情况下有哪些地方会有缓存处理？

1. 浏览器端存储
2. 浏览器端文件缓存
3. HTTP缓存304
4. 服务器端文件类型缓存
5. 表现层&DOM缓存

参考《[一次HTTP请求中有哪些地方可以缓存](http://www.nowamagic.net/librarys/veda/detail/162)》

------

#### cookie及其操作

- cookie是web浏览器存储的少量数据，最早设计为服务器端使用，作为HTTP协议的扩展实现。cookie数据会自动在浏览器和服务器之间传输。
- 通过读写cookie检测是否支持
- cookie属性有**名**，**值**，**max-age**，**path**, **domain**，**secure**；
- cookie默认有效期为浏览器会话，一旦用户关闭浏览器，数据就丢失，通过设置**max-age=seconds**属性告诉浏览器cookie有效期
- cookie作用域通过**文档源**和**文档路径**来确定，通过**path**和**domain**进行配置，web页面同目录或子目录文档都可访问
- 通过cookie保存数据的方法为：为document.cookie设置一个符合目标的字符串如下
- 读取document.cookie获得'; '分隔的字符串，key=value,解析得到结果

```
document.cookie = 'name=qiu; max-age=9999; path=/; domain=domain; secure';

document.cookie = 'name=aaa; path=/; domain=domain; secure';
// 要改变cookie的值，需要使用相同的名字、路径和域，新的值
// 来设置cookie，同样的方法可以用来改变有效期

// 设置max-age为0可以删除指定cookie

//读取cookie，访问document.cookie返回键值对组成的字符串，
//不同键值对之间用'; '分隔。通过解析获得需要的值

```

#### 页面缓存原理

页面缓存状态是由http header决定的，一个浏览器请求信息，一个是服务器响应信息。主要包括Pragma: no-cache、Cache-Control、 Expires、 Last-Modified、If-Modified-Since。

#### 应用程序存储和离线web应用

HTML5新增应用程序缓存，允许web应用将应用程序自身保存到用户浏览器中，用户离线状态也能访问。 1.为html元素设置manifest属性:`<html manifest="myapp.appcache">`，其中后缀名只是一个约定，真正识别方式是通过`text/cache-manifest`作为MIME类型。所以需要配置服务器保证设置正确 2.manifest文件首行为`CACHE MANIFEST`，其余就是要缓存的URL列表，每个一行，相对路径都相对于manifest文件的url。注释以#开头 3.url分为三种类型：`CACHE`:为默认类型。`NETWORK`：表示资源从不缓存。 `FALLBACK`:每行包含两个url，第二个URL是指需要加载和存储在缓存中的资源， 第一个URL是一个前缀。任何匹配该前缀的URL都不会缓存，如果从网络中载入这样的URL失败的话，就会用第二个URL指定的缓存资源来替代。以下是一个文件例子：

```son
CACHE MANIFEST

CACHE:
myapp.html
myapp.css
myapp.js

FALLBACK:
videos/ offline_help.html

NETWORK:
cgi/

```



---