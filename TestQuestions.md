## 测试相关问题

#### 对代码进行测试的有什么优缺点？

优点：

* 质量控制，为代码的提交、上线及维护都提供了质量标准
* 提前发现错误
* 前期测试的完备，后期开发成本会降低

缺点：

* 完备的测试，单测／功能测试／端到端测试等需要一定的成本

---
#### 你会用什么工具测试你的代码功能？

* Enzyme：React组件的测试工具，模拟组件的渲染和加载
* Jest：基于 `Jasmine` 框架的 JavaScript 单元测试工具
* 当然还有一些第三方测试库提供的Mock
* 后端 Mocha，还没有使用经验

---
#### 单元测试与功能/集成测试的区别是什么？

* 单元测试

  最小粒度的测试，一般是开发人员对函数、模块内部进行测试，偏向白盒测试

* 功能测试

  测试业务功能

* 集成测试

  比单元测试粒度大，比如测试已经经过单测的模块间的接口

---
#### 代码风格 linting 工具的作用是什么？

* 提高代码可读性
* 团队风格一致，便于维护开发
* 消除代码的歧义

---

知道BDD, TDD, Unit Test么? 知道怎么测试你的前端工程么(mocha, sinon, jasmin, qUnit..)?



---

## [错误处理/调试](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md)

- [`[Doc\]` Errors (异常)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#errors)
- [`[Doc\]` Domain (域)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#domain)
- [`[Doc\]` Debugger (调试器)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#debugger)
- [`[Doc\]` C/C++ 插件](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#c-c++-addon)
- [`[Doc\]` V8](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#v8)
- [`[Point\]` 内存快照](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#%E5%86%85%E5%AD%98%E5%BF%AB%E7%85%A7)
- [`[Point\]` CPU profiling](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#cpu-profiling)

**常见问题**

- 怎么处理未预料的出错? 用 try/catch ，domains 还是其它什么? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#q-handle-error)
- 什么是 `uncaughtException` 事件? 一般在什么情况下使用该事件? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#uncaughtexception)
- domain 的原理是? 为什么要弃用 domain? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#domain)
- 什么是防御性编程? 与其相对的 let it crash 又是什么?
- 为什么要在 cb 的第一参数传 error? 为什么有的 cb 第一个参数不是 error, 例如 http.createServer?
- 为什么有些异常没法根据报错信息定位到代码调用? 如何准确的定位一个异常? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#%E9%94%99%E8%AF%AF%E6%A0%88%E4%B8%A2%E5%A4%B1)
- 内存泄漏通常由哪些原因导致? 如何分析以及定位内存泄漏? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md#%E5%86%85%E5%AD%98%E5%BF%AB%E7%85%A7)

[阅读更多](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/error.md)

## [测试](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md)

- [`[Basic\]` 测试方法](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#%E6%B5%8B%E8%AF%95%E6%96%B9%E6%B3%95)
- [`[Basic\]` 单元测试](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
- [`[Basic\]` 集成测试](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#%E9%9B%86%E6%88%90%E6%B5%8B%E8%AF%95)
- [`[Basic\]` 基准测试](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#%E5%9F%BA%E5%87%86%E6%B5%8B%E8%AF%95)
- [`[Basic\]` 压力测试](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#%E5%8E%8B%E5%8A%9B%E6%B5%8B%E8%AF%95)
- [`[Doc\]` Assert (断言)](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#assert)

**常见问题**

- 为什么要写测试? 写测试是否会拖累开发进度?[[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#q-why-write-test)
- 单元测试的单元是指什么? 什么是覆盖率?[[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95)
- 测试是如何保证业务逻辑中不会出现死循环的?[[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#q-death-loop)
- mock 是什么? 一般在什么情况下 mock?[[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md#mock)

[阅读更多](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/test.md)

---

