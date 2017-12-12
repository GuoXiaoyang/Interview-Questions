### 数据结构与元素相关问题

我只放了一些很常见的问题，个人刷LeetCode的在这里。如果可能的话，希望系统整理一份JavaScript实现常见数据结构与算法的代码。



### 快排

### 判断数字是否为质数

```javascript
function isPrime(number) {
   // If your browser doesn't support the method Number.isInteger of ECMAScript 6,
   // you can implement your own pretty easily
   if (typeof number !== 'number' || !Number.isInteger(number)) {
      // Alternatively you can throw an error.
      return false;
   }
   if (number < 2) {
      return false;
   }
 
   if (number === 2) {
      return true;
   } else if (number % 2 === 0) {
      return false;
   }
   var squareRoot = Math.sqrt(number);
   for(var i = 3; i <= squareRoot; i += 2) {
      if (number % i === 0) {
         return false;
      }
   }
   return true;
```



### 找出数字的所有质数因子

### 获取第n个斐波那契数

### 找出两个数的最大公因子

### 数组去重

### 合并两个有序序列，使重新有序

### 不适应临时变量交换两个数

### 反转字符串

### 反转句子中的单词

### 找出字符串的第一个非重复字符

### 字符串去重

### 判断回文字符串

### 生成5-7之间的随机数

### 找出1-100间，未排序数组的缺失数字

### 给定数组，找出是否存在两个元素的和等于指定数

### 找出数组中最大的两个数

### 从1-n的整数间，一共存在多少个0

### 判断字符串subStr是否是字符串str的子字符串

### 得到字符串的全排列

---

