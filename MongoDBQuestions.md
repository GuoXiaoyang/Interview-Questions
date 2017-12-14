### MongoDB相关问题

#### MongoDB是什么？与MySQL区别在于？

#### 备份数据库与 M/S, M/M 等部署方式的区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#replication)

#### 索引有什么用，大致原理是什么? 设计索引有什么注意点? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#%E7%B4%A2%E5%BC%95)

#### Monogdb 连接问题(超时/断开等)有可能是什么问题导致的? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#Mongodb)

#### 什么情况下数据会出现脏数据? 如何避免? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#%E6%95%B0%E6%8D%AE%E4%B8%80%E8%87%B4%E6%80%A7)

#### 

---

#### MongoDB原理



#### mongodb有哪些常用优化措施

参考答案: 类似传统数据库，索引和分区．

#### mongoose是什么？有支持哪些特性?

参考答案: mongoose是mongodb的文档映射模型．主要由Schema, Model和Instance三个方面组成．Schema就是定义数据类型，Model就是把Schema和js类绑定到一起，Instance就是一个对象实例．常见mongoose操作有,save, update, find. findOne, findById, static方法等。



---

#### redis支持哪些功能

参考答案: set/get, mset/hset/hmset/hmget/hgetall/hkeys, sadd/smembers, publish/subscribe, expire

#### redis 与 memcached 的区别? [[more\]](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#%E7%BC%93%E5%AD%98)

#### redis最简单的应用

参考答案:

```javascript
var redis = require("redis"),
    client = redis.createClient();

client.set("foo_rand000000000000", "some fantastic value");
client.get("foo_rand000000000000", function (err, reply) {
  console.log(reply.toString());
});
client.end();
```

---

#### 文章

- [`[Point\]` Mysql](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#mysql)
- [`[Point\]` Mongodb](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#mongodb)
- [`[Point\]` Replication](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#replication)
- [`[Point\]` 数据一致性](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#%E6%95%B0%E6%8D%AE%E4%B8%80%E8%87%B4%E6%80%A7)
- [`[Point\]` 缓存](https://github.com/ElemeFE/node-interview/blob/master/sections/zh-cn/storage.md#%E7%BC%93%E5%AD%98)

#### 

---

