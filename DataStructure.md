### 数据结构与元素相关问题

我只放了一些很常见的问题，个人刷LeetCode的在这里。如果可能的话，希望系统整理一份JavaScript实现常见数据结构与算法的代码。



#### 快排

#### 判断数字是否为质数

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



#### 找出数字的所有质数因子

```javascript
function primeFactors(n){
  var factors = [], 
      divisor = 2;
  
  while(n>2){
    if(n % divisor == 0){
       factors.push(divisor); 
       n= n/ divisor;
    }
    else{
      divisor++;
    }     
  }
  return factors;
}
```

复杂度O(n)，优化：除数每次+2，范围缩小到n/2

#### 获取第n个斐波那契数

```javascript
function fibonacci(n){
  var fibo = [0, 1];
  
  if (n <= 2) return 1;

  for (var i = 2; i <=n; i++ ){
   fibo[i] = fibo[i-1]+fibo[i-2];
  }

 return fibo[n];
} 
```

复杂度O(n)

#### 找出两个数的最大公因子

```javascript
unction greatestCommonDivisor(a, b){
  var divisor = 2, 
      greatestDivisor = 1;

  //if u pass a -ve number this will not work. fix it dude!!
  if (a < 2 || b < 2)
     return 1;
  
  while(a >= divisor && b >= divisor){
   if(a %divisor == 0 && b% divisor ==0){
      greatestDivisor = divisor;      
    }
   divisor++;
  }
  return greatestDivisor;
}
```

```javascript
function greatestCommonDivisor(a, b){
   if(b == 0)
     return a;
   else 
     return greatestCommonDivisor(b, a%b);
}
```



#### 数组去重

```javascript
function removeDuplicate(arr){
  var exists ={},
      outArr = [], 
      elm;

  for(var i =0; i<arr.length; i++){
    elm = arr[i];
    if(!exists[elm]){
      outArr.push(elm);
      exists[elm] = true;
   }
  }
  return outArr;
}
```



#### 合并两个有序序列，使重新有序

```javascript
function mergeSortedArray(a, b){
  var merged = [], 
      aElm = a[0],
      bElm = b[0],
      i = 1,
      j = 1;
  
  if(a.length ==0)
    return b;
  if(b.length ==0)
    return a;
  /* 
  if aElm or bElm exists we will insert to merged array
  (will go inside while loop)
   to insert: aElm exists and bElm doesn't exists
             or both exists and aElm < bElm
    this is the critical part of the example            
  */
  while(aElm || bElm){
   if((aElm && !bElm) || aElm < bElm){
     merged.push(aElm);
     aElm = a[i++];
   }   
   else {
     merged.push(bElm);
     bElm = b[j++];
   }
  }
  return merged;
}
```



#### 不使用临时变量交换两个数

```javascript
function swapNumb(a, b){
  console.log('before swap: ','a: ', a, 'b: ', b);
  b = b -a;
  a = a+ b;
  b = a-b;
  console.log('after swap: ','a: ', a, 'b: ', b);  
}

```

```javascript
function swapNumb(a, b){
  console.log("a: " + a + " and b: " + b);
  a = a ^ b;
  b = a ^ b;
  a = a ^ b;
  console.log("a: " + a + " and b: " + b);
}
```



#### 反转字符串

```javascript
function reverse(str){
  var rtnStr = '';
  for(var i = str.length-1; i>=0;i--){
    rtnStr +=str[i];
  }
  return rtnStr;
}
```

```javascript
function reverse(str){
  var rtnStr = [];
  if(!str || typeof str != 'string' || str.length < 2 ) return str;
  
  for(var i = str.length-1; i>=0;i--){
    rtnStr.push(str[i]);
  }
  return rtnStr.join('');
}
```

```javascript
function reverse(str) {
  str = str.split('');
  var len = str.length,
      halfIndex = Math.floor(len / 2) - 1,
      revStr;
  for (var i = 0; i <= halfIndex; i++) {
    revStr = str[len - i - 1];
    str[len - i - 1] = str[i];
    str[i] = revStr;
  }
  return str.join('');
}
```

```javascript
function reverse (str) {
    if (str === "") {
        return "";
    } else {
        return reverse(str.substr(1)) + str.charAt(0);
    }
}
```

```javascript
function reverse(str){
  if(!str || str.length <2) return str;
  
  return str.split('').reverse().join('');
}
```

```javascript
String.prototype.reverse = function (){
  if(!this || this.length <2) return this;
  
  return this.split('').reverse().join('');
}
```



#### 反转句子中的单词

```javascript
function reverseInPlace(str){
  return str.split(' ').reverse().join(' ').split('').reverse().join('');
}

```



#### 找出字符串的第一个非重复字符

```javascript

function firstNonRepeatChar(str){
  var len = str.length,
      char, 
      charCount = {};
  for(var i =0; i<len; i++){
    char = str[i];
    if(charCount[char]){
      charCount[char]++;
    }
    else
      charCount[char] = 1;
  }
  for (var j in charCount){
    if (charCount[j]==1)
       return j;
  }
}  
```



#### 字符串去重

```javascript
function removeDuplicateChar(str){
  var len = str.length,
      char, 
      charCount = {}, 
      newStr = [];
  for(var i =0; i<len; i++){
    char = str[i];
    if(charCount[char]){
      charCount[char]++;
    }
    else
      charCount[char] = 1;
  }
  for (var j in charCount){
    if (charCount[j]==1)
       newStr.push(j);
  }
  return newStr.join('');
}
```



#### 有一个大数组,var a = ['1', '2', '3', ...];a的长度是100,内容填充随机整数的字符串.请先构造此数组a,然后设计一个算法将其内容去重

```javascript
/**
    * 数组去重
    **/
function normalize(arr) {
  if (arr && Array.isArray(arr)) {
    var i, len, map = {};
    for (i = arr.length; i >= 0; --i) {
      if (arr[i] in map) {
        arr.splice(i, 1);
      } else {
        map[arr[i]] = true;
      }
    }
  }
  return arr;
}

/**
    * 用100个随机整数对应的字符串填充数组。
    **/
function fillArray(arr, start, end) {
  start = start == undefined ? 1 : start;
  end = end == undefined ?  100 : end;

  if (end <= start) {
    end = start + 100;
  }

  var width = end - start;
  var i;
  for (i = 100; i >= 1; --i) {
    arr.push('' + (Math.floor(Math.random() * width) + start));
  }
  return arr;
}

var input = [];
fillArray(input, 1, 100);
input.sort(function (a, b) {
  return a - b;
});
console.log(input);

normalize(input);
console.log(input);


```



#### 判断回文字符串

```javascript
function isPalindrome(str){
  var i, len = str.length;
  for(i =0; i<len/2; i++){
    if (str[i]!== str[len -1 -i])
       return false;
  }
  return true;
}
```

```javascript
function checkPalindrom(str) {
    return str == str.split('').reverse().join('');
}
```



#### 生成5-7之间的随机数

```javascript

function rand5(){
   return 1 + Math.random() * 4;
}

function rand7(){
  return 5 + rand5() / 5 * 2;
}
```



#### 找出1-100间，未排序数组的一个缺失数字

```javascript
function missingNumber(arr){
  var n = arr.length+1, 
  sum = 0,
  expectedSum = n* (n+1)/2;
  
  for(var i = 0, len = arr.length; i < len; i++){
    sum += arr[i];
  }
  
  return expectedSum - sum;
}

```



#### 给定数组，找出是否存在两个元素的和等于指定数

```javascript

function sumFinder(arr, sum){
  var len = arr.length;
  
  for(var i =0; i<len-1; i++){  
     for(var j = i+1;j<len; j++){
        if (arr[i] + arr[j] == sum)
            return true;
     }
  }
  
  return false;
}
```

```javascript
function sumFinder(arr, sum){
  var differ = {}, 
      len = arr.length,
      substract;
  
  for(var i =0; i<len; i++){
     substract = sum - arr[i];

     if(differ[substract])
       return true;       
     else
       differ[arr[i]] = true;
  }
  
  return false;
}
```


#### 找出数组中最大的两个数

```javascript
function topSum(arr){
  
  var biggest = arr[0], 
      second = arr[1], 
      len = arr.length, 
      i = 2;

  if (len<2) return null;
  
  if (biggest<second){
    biggest = arr[1];
    second = arr[0];
  } 
  
  for(; i<len; i++){

   if(arr[i] > biggest){
      second = biggest;
      biggest = arr[i];
    }
   else if (arr[i]>second){
      second = arr[i];
   }
    
 }
 return biggest + second;
}
```



#### 从1-n的整数间，一共存在多少个0

```javascript
function countZero(n){
  var count = 0;
  while(n>0){
   count += Math.floor(n/10);
   n = n/10;
  }
  return count;
}
```



#### 判断字符串subStr是否是字符串str的子字符串

```javascript
function subStringFinder(str, subStr){
  var idx = 0,
      i = 0,
      j = 0,
      len = str.length,
      subLen = subStr.length;

   for(; i<len; i++){
   
      if(str[i] == subStr[j])
         j++;
      else
         j = 0;
      
      //check starting point or a match   
      if(j == 0)
        idx = i;
      else if (j == subLen)
        return idx;
  }

  return -1;
}
```



#### 得到字符串的全排列

```javascript
function permutations(str){
var arr = str.split(''),
    len = arr.length, 
    perms = [],
    rest,
    picked,
    restPerms,
    next;

    if (len == 0)
        return [str];

    for (var i=0; i<len; i++)
    {
        rest = Object.create(arr);
        picked = rest.splice(i, 1);

        restPerms = permutations(rest.join(''));

       for (var j=0, jLen = restPerms.length; j< jLen; j++)
       {
           next = picked.concat(restPerms[j]);
           perms.push(next.join(''));
       }
    }
   return perms;
}
```

#### 如何实现数组的随机排序？

  方法一：

```javascript
var arr = [1,2,3,4,5,6,7,8,9,10];
function randSort1(arr){
  for(var i = 0,len = arr.length;i < len; i++ ){
    var rand = parseInt(Math.random()*len);
    var temp = arr[rand];
    arr[rand] = arr[i];
    arr[i] = temp;
  }
  return arr;
}
console.log(randSort1(arr));
```

方法二：

```javascript
var arr = [1,2,3,4,5,6,7,8,9,10];
function randSort2(arr){
  var mixedArray = [];
  while(arr.length > 0){
    var randomIndex = parseInt(Math.random()*arr.length);
    mixedArray.push(arr[randomIndex]);
    arr.splice(randomIndex, 1);
  }
  return mixedArray;
}
console.log(randSort2(arr));
```

  方法三：

```javascript
var arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(function(){
  return Math.random() - 0.5;
})
console.log(arr);
```

#### 完成一个函数,接受数组作为参数,数组元素为整数或者数组,数组元素包含整数或数组,函数返回扁平化后的数组

如：[1, [2, [ [3, 4], 5], 6]] => [1, 2, 3, 4, 5, 6]

```javascript
    var data =  [1, [2, [ [3, 4], 5], 6]];

    function flat(data, result) {
        var i, d, len;
        for (i = 0, len = data.length; i < len; ++i) {
            d = data[i];
            if (typeof d === 'number') {
                result.push(d);
            } else {
                flat(d, result);
            }
        }
    }

    var result = [];
    flat(data, result);

    console.log(result);


```



---

#### 参考

[^1]: http://www.thatjsdude.com/interview/js1.html

