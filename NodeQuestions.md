### Node.js相关问题

#### Node 开发技能图解

---



#### Node 事件循环机制

- ES6新特性
- javascript高级话题(面向对象，作用域，闭包，设计模式等)
- node核心内置类库(事件，流，文件，网络等)
- node高级话题(异步，部署，性能调优，异常调试等)
- 常用知名第三方类库(Async, Express等)
- 其它相关后端常用技术(MongoDB, Redis, Apache, Nginx等)
- 常用前端技术(Html5, CSS3, JQuery等)



### 基础

- [ 类型判断](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#%E7%B1%BB%E5%9E%8B%E5%88%A4%E6%96%AD)
- [作用域](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#%E4%BD%9C%E7%94%A8%E5%9F%9F)
- [引用传递](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#%E5%BC%95%E7%94%A8%E4%BC%A0%E9%80%92)
- [内存释放](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#%E5%86%85%E5%AD%98%E9%87%8A%E6%94%BE)
- [ ES6 新特性](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#es6-%E6%96%B0%E7%89%B9%E6%80%A7)



- js 中什么类型是引用传递, 什么类型是值传递? 如何将值类型的变量以引用的方式传递? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#q-value)
- js 中， 0.1 + 0.2 === 0.3 是否为 true ? 在不知道浮点数位数时应该怎样判断两个浮点数之和与第三数是否相等？
- const 定义的 Array 中间元素能否被修改? 如果可以, 那 const 修饰对象的意义是? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#q-const)
- JavaScript 中不同类型以及不同环境下变量的内存都是何时释放? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/common.md#q-mem)



### 模块

- [模块机制](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#%E6%A8%A1%E5%9D%97%E6%9C%BA%E5%88%B6)
- [ 热更新](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#%E7%83%AD%E6%9B%B4%E6%96%B0)
- [上下文](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#%E4%B8%8A%E4%B8%8B%E6%96%87)
- [包管理](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#%E5%8C%85%E7%AE%A1%E7%90%86)


- a.js 和 b.js 两个文件互相 require 是否会死循环? 双方是否能导出变量? 如何从设计上避免这种问题? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#q-loop)
- 如果 a.js require 了 b.js, 那么在 b 中定义全局变量 `t = 111` 能否在 a 中直接打印出来? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#q-global)
- 如何在不重启 node 进程的情况下热更新一个 js/json 文件? 这个问题本身是否有问题? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/module.md#q-hot)

### 事件/异步

- [`[Basic\]` Promise](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#promise)
- [`[Doc\]` Events (事件)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#events)
- [`[Doc\]` Timers (定时器)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#timers)
- [`[Point\]` 阻塞/异步](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#%E9%98%BB%E5%A1%9E%E5%BC%82%E6%AD%A5)
- [`[Point\]` 并行/并发](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#%E5%B9%B6%E8%A1%8C%E5%B9%B6%E5%8F%91)


- Promise 中 .then 的第二参数与 .catch 有什么区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#q-1)
- Eventemitter 的 emit 是同步还是异步? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#q-2)
- 如何判断接口是否异步? 是否只要有回调函数就是异步? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#q-3)
- nextTick, setTimeout 以及 setImmediate 三者有什么区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#q-4)
- 如何实现一个 sleep 函数? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#q-5)
- 如何实现一个异步的 reduce? (注:不是异步完了之后同步 reduce) [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/event-async.md#q-6)

### 进程

- [`[Doc\]` Process (进程)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#process)
- [`[Doc\]` Child Processes (子进程)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#child-process)
- [`[Doc\]` Cluster (集群)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#cluster)
- [`[Basic\]` 进程间通信](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1)
- [`[Basic\]` 守护进程](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B)



- 进程的当前工作目录是什么? 有什么作用? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#q-cwd)
- child_process.fork 与 POSIX 的 fork 有什么区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#q-fork)
- 父进程或子进程的死亡是否会影响对方? 什么是孤儿进程? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#q-child)
- cluster 是如何保证负载均衡的? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#how-it-works)
- 什么是守护进程? 如何实现守护进程? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/process.md#%E5%AE%88%E6%8A%A4%E8%BF%9B%E7%A8%8B)



### IO

- [`[Doc\]` Buffer](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#buffer)
- [`[Doc\]` String Decoder (字符串解码)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#string-decoder)
- [`[Doc\]` Stream (流)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#stream)
- [`[Doc\]` Console (控制台)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#console)
- [`[Doc\]` File System (文件系统)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#file)
- [`[Doc\]` Readline](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#readline)
- [`[Doc\]` REPL](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#repl)


- Buffer 一般用于处理什么数据? 其长度能否动态变化? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#buffer)
- Stream 的 highWaterMark 与 drain 事件是什么? 二者之间的关系是? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#%E7%BC%93%E5%86%B2%E5%8C%BA)
- Stream 的 pipe 的作用是? 在 pipe 的过程中数据是引用传递还是拷贝传递? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#pipe)
- 什么是文件描述符? 输入流/输出流/错误流是什么? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#file)
- console.log 是同步还是异步? 如何实现一个 console.log? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#console)
- 如何同步的获取用户的输入? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#%E5%A6%82%E4%BD%95%E5%90%8C%E6%AD%A5%E7%9A%84%E8%8E%B7%E5%8F%96%E7%94%A8%E6%88%B7%E7%9A%84%E8%BE%93%E5%85%A5)
- Readline 是如何实现的? (有思路即可) [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/io.md#readline)

## [util](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md)

- [`[Doc\]` URL](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#url)
- [`[Doc\]` Query Strings (查询字符串)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#query-strings)
- [`[Doc\]` Utilities (实用函数)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#util-1)
- [`[Basic\]` 正则表达式](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#%E6%AD%A3%E5%88%99%E8%A1%A8%E8%BE%BE%E5%BC%8F)

**常见问题**

- HTTP 如何通过 GET 方法 (URL) 传递 let arr = [1,2,3,4] 给服务器? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#get-param)
- Node.js 中继承 (util.inherits) 的实现? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#utilinherits)
- 如何递归获取某个文件夹下所有的文件名? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/util.md#q-traversal)



### 参考

[^1]: https://sunebear.gitbooks.io/frontend-developer-interview-questions-and-answers/content/nodejsmd.html
[^2]: [Node.js 实践教程](https://github.com/ElemeFE/node-practice)
[^3]: []如何通过饿了么 Node.js 面试(https://github.com/ElemeFE/node-interview/tree/master/sections/zh-cn)

 

