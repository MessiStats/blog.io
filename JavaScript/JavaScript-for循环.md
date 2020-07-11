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

forEach循环传入两个参数

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

