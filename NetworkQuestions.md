## 网络相关问题
#### 为什么传统上利用多个域名来提供网站资源会更有效？

浏览器对单个域名请求对资源数量做了限制。

---
#### 请尽可能完整得描述从输入 URL 到整个网页加载完毕及显示在屏幕上的整个流程。

很经典的面试题目。



---
#### Long-Polling、Websockets 和 Server-Sent Event 之间有什么区别？



---
#### 请描述以下 request 和 response headers：
* Expires, Date, Age, If-Modified-...之间的不同
* Do Not Track
* Cache-Control
* Transfer-Encoding
* ETag
* X-Frame-Options



---

#### 什么是 HTTP method？请罗列出你所知道的所有 HTTP method，并给出解释。



---
#### 请解释 HTTP status 301 与 302 的区别？



---

- [`[Doc\]` Net (网络)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#net)
- [`[Doc\]` UDP/Datagram](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#udp)
- [`[Doc\]` HTTP](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#http)
- [`[Doc\]` DNS (域名服务器)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#dns)
- [`[Doc\]` ZLIB (压缩)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#zlib)
- [`[Point\]` RPC](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#rpc)


- cookie 与 session 的区别? 服务端如何清除 cookie? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/network.md#q-cookie-session)
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


HTTP request报文结构是怎样的
HTTP response报文结构是怎样的
HTTP状态码及其含义