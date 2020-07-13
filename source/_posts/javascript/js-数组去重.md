---
title: JavaScript数组去重
date: 2018-11-04 20:59:53
tags: [数组去重,JavaScript]
---
## JavaScript数组去重

###一、ES6 语法去重

1.  Set去重（ES6中最常用）

   ```javascript
    var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

       function unique (arr) {
           return Array.from(new Set(arr))
       }
       console.log(unique(testArr))
       //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {}, {}]
       //不考虑兼容性，这种去重的方法代码最少。这种方法还无法去掉“{}”空对象
   ```

   ​

2. ```javascript
   [...new Set(arr)]
   ```

3. Map 数据结构数组去重

   > Map中不会出现相同的key值 , 把数组的作为key存到Map
   >
   > 与利用对象的属性不能相同 类似

   ```javascript
    var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

   function unique(arr) {
     let map = new Map();
     let array = new Array();
     for (let i = 0; i<arr.length; i++) {
       if (map.has(arr[i])) {
         map.set(arr[i], true)
       }else{
         map.set(arr[i], false)
         array.push(arr[i])
       }
     }
     return array
   }
    console.log(unique(testArr))
   //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
   ```

   ​

### 二、ES5 语法去重

1. 利用 for 循环嵌套, splice()

   > NaN和{}没有去重，

   ```javascript
   var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

       function unique (arr) {
           for (let i=0; i<arr.length; i++) {
               for (let j=i+1; i<arr.length; j++) {
                   if (arr[i] == arr[j]) {   //第一个 和 后面元素比较
                       arr.splice(j,1);       // 删除 后一个元素
                       j--;        // 原数组 长度改变
                   }
               }
           }
           return arr
       }
       console.log(unique(testArr))
       //[1, "true", 15, false, undefined, NaN, NaN, "NaN", "a", {…}, {…}]  直接修改原数组
       //NaN和{}没有去重，两个null直接消失了
   ```

   ​

2. indexOf 去重

   >NaN和{}没有去重，

   ```javascript
   var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

       function unique (arr) {
          if (!Array.isArray(arr)) {
              console.log('type error')
              return
          }
          var array = []
          for (let i=0; i<arr.length; i++) {
              if (array.indexOf( arr[i] ) === -1) {
                  array.push(arr[i])
              }
          }
           return array
       }
       console.log(unique(testArr))
       //[1, "true", true, 15, false, undefined, null, NaN, NaN, "NaN", 0, "a", {…}, {…}]
       //NaN和{}没有去重
   ```

3. includes

   > **includes() 和 indexOf()  判断方法不一样**
   >
   > {}没有去重

```javascript
var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

function unique (arr) {
	 if (!Array.isArray(arr)) {
           console.log('type error')
           return
       }
       var array = []
       for (let i=0; i<arr.length; i++) {
           if (!array.includes(arr[i]) ) {
               array.push(arr[i])
           }
       }
        return array
}
console.log(unique(testArr))
//[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
//{}没有去重
```
4. **hasOwnProperty**

   > **所有的都去重了**
   >
   > 利用hasOwnProperty 判断是否存在对象属性
   >
   > filter 返回true表示保留该元素
   >
   > null typeof 为 Object
   >
   > undefined typeof 为 undefined

```javascript
var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

function unique (arr) {
    var obj = {};
    return arr.filter((item, index, arr) => {
       return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = 1)
    })
}
console.log(unique(testArr))
//[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]
//所有的都去重了
```


5. **利用对象的属性不能相同的特点进行去重**

   > true  不能去重
   >
   > NaN和空对象{} 可以去重

   ```javascript
   var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

   function unique (arr) {
   	 if (!Array.isArray(arr)) {
              console.log('type error')
              return
         }
         var array = [];
     	  var obj = {};
     	  for (let i=0; i<arr.length; i++) {
              if (!obj[arr[i]]) {
                  array.push(arr[i]);
                  obj[arr[i]] = 1;
              }
          }
     	return array
   }

   console.log(unique(testArr))

   //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]

   //[1, "true", 15, false, undefined, null, NaN, 0, "a", {…}]
   //两个true直接去掉了，NaN和{}去重

   ```

   6. filter ()

      > 当前元素，在原始数组中的第一个索引==当前索引值，否则返回当前元素

      ```javascript
      var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

      function unique (arr) {
          return arr.filter((item, index, arr) => {
             return arr.indexOf(item, 0) === index
          })
      }
      console.log(unique(testArr))
      //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}]
      ```

      ​

   7. reduce + includes

      > reduce ()
      >
      > 对累加器和数组中的每个元素（从左到右）应用一个函数，将其简化为单个值。
      >
      > 1. Accumulator (acc) (累加器)
      >
      > 2. Current Value (cur) (当前值)
      >
      > 3. Current Index (idx) (当前索引)
      >
      > 4. Source Array (src) (源数组)
      >
      >    `accumulator` 和`currentValue`的取值有两种情况：如果调用`reduce()`时提供了`initialValue`，`accumulator`取值为`initialValue`，`currentValue`取数组中的第一个值；如果没有提供 `initialValue`，那么`accumulator`取数组中的第一个值，`currentValue`取数组中的第二个值。
      >
      >    **注意：**如果没有提供`initialValue`，reduce 会从索引1的地方开始执行 callback 方法，跳过第一个索引。如果提供`initialValue`，从索引0开始。

      ```javascript
      var testArr = [1, 1, 'true', 'true', true, true, 15, 15, false, false, undefined, undefined, null, null, NaN, NaN, 'NaN', 0, 0, 'a', 'a', {}, {}]

      function unique (arr) {
          return arr.reduce((prev,cur) => prev.includes(cur) ? prev : [...prev,cur],[])
      }
      console.log(unique(testArr))
      //[1, "true", true, 15, false, undefined, null, NaN, "NaN", 0, "a", {…}, {…}]
      ```
