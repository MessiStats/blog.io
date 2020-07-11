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

