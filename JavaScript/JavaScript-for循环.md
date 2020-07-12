## continue语句

continue的作用是跳出当前循环，进入下一次循环，所以遇到2的时候会跳过，输出的是1 3 4 5

```JavaScript
const arr = [1, 2, 3, 4, 5]
for (let i = 0; i < arr.length; i++) {
    if (arr[i] === 2) {
        continue // 跳出当前循环，进入下一次循环
    }
    console.log(arr[i]) // 打印输出的结果 1 3 4 5
}
```

## break语句

 break 语句的作用是直接跳出当前的循环，所以以下的代码只会输出1

```javascript
const arr = [1, 2, 3, 4, 5]
for (let i = 0; i < arr.length; i++) {
    if (arr[i] === 2) {
        break // 直接跳出循环
    }
    console.log(arr[i]) // 打印输出的结果 1 
}
```



## forEach方法

forEach循环callback传入两个参数

- 第一个参数是item，指代的是数组的元素的每一个对象
- 第二个参数是index，指代的是数组中每一个元素的索引

```javascript
const arr = [1, 2, 3, 4, 5]

arr.forEach((item, index) => {
    if (item === 2) {
        //continue // VM356:3 Uncaught SyntaxError: Illegal continue statement: no surrounding iteration statement
        //break // Uncaught SyntaxError: Illegal break statement
    }
    console.log(item,index)
})
```

**需要注意的是forEach是不能使用break或者是continue方法的，不然会报错！！！**



```javascript
const arr = [1, 2, 3, 4, 5]

arr.forEach((item, index) => {
    if (item === 2) {
        return false
    }
    console.log(item, index) 
    // 1 0
    // 3 2
    // 4 3
    // 5 4
})
```

如果想使用forEach方法跳出此次循坏的话，可以return false



```JavaScript
try {
    const arr = [1, 2, 3, 4, 5]

    arr.forEach((item, index) => {
        if (item === 2) {
            throw new Error("forEachError")
        }
        console.log(item, index)
    })
}catch (e) {
    console.log(e)
    if (e.message !== "forEachError") {
        throw e
    }
}
```

如果想要匹配到符合条件的时候终止循环，那么可以使用try catch的方式

## every方法

every方法callback默认可以传入三个参数

- 第一个参数element，用于测试的当前值。
- 第二个参数index，用于测试的当前值的索引。
- 第三个参数array，调用 every 的当前数组。

```javascript
const arr = [1, 2, 3, 4, 5]

arr.every(item => {
    if (item === 2) {
        break // SyntaxError: Illegal break statement
        continue // SyntaxError: Illegal continue statement: no surrounding iteration statement
    }
})
```

**同样的every方法是不能使用break或者是continue方法的，不然会报错！！！**

因为 every 默认返回的是false，如果返回的是false那么循环不再执行，所以我们要手动返回true 不能使用 break continue

```javascript
arr.every(item => {
    if (item === 2) {
        return true
    } else {
        console.log(item)  // 1 3 4 5
        return true
    }
})
```

every方法使用的案例

```js
function isBigEnough(element, index, array) {
  return element >= 10
}
[12, 5, 8, 130, 44].every(isBigEnough)   // false
[12, 54, 18, 130, 44].every(isBigEnough) // true
```



## for in 循环

for in循环适合用来遍历对象，因为数组也是对象，所以for in循环也可以遍历数组，但是用for in 循环的遍历数组的话可能会有一些缺点，比如

- 1.index索引为字符串型数字，不能直接进行几何运算
- 2.遍历顺序有可能不是按照实际数组的内部顺序
- 3.使用for in会遍历数组所有的可枚举属性，包括原型。例如下图的原型方法method和name属性所以for in更适合遍历对象，不要使用for in遍历数组。

```javascript
const arr = [1, 2, 3, 4, 5]
Array.prototype._method = function () {
    console.log(this.length)
}

arr.name = "this is array"
// index 是对象的 key 
for (let index in arr) {
    console.log(arr[index]) 
    /*
    * 1
    * 2
    * 3
    * 4
    * 5
    * this is array
    * [Function]
    * */
}

```

再让我们看看用for in循坏来遍历对象吧

```javascript
const obj = {
    a: 1,
    b: 2,
    c: 3
}

Object.prototype._method = function () {
    console.log(this)
}


for (let key in obj) {
    console.log(key, obj[key])
    /*
    * a 1
    * b 2
    * c 3
    * _method ƒ () {
    console.log(this)
    }
    * undefined
    * */
}
```

for in 可以遍历到obj的原型方法_method,如果不想遍历**原型方法和属性**的话，可以在循环内部判断一下,hasOwnPropery方法可以判断某属性是否是该对象的**实例**属性

```javascript
for (let key in obj) {
    if (obj.hasOwnProperty(key)){
        console.log(key, obj[key])
    }
    /*
    * a 1
    * b 2
    * c 3
    * undefined
    * */
}
```


  