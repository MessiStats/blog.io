## 用JavaScript手写一个方法实现call和apply

先说一下设计的思路吧，因为call和apply都是动态的去绑定this，obj1.call(obj2)，那么obj2就继承了obj1的属性和方法

##### 举个例子：

```javascript
const obj = {
    value:1
}
function say(){
   console.log(this.value)
}
say.call(obj) // 输出1
```



所以按照这个思路，相当于obj这个对象里面已经有了say这个方法，相当于下面这段代码

```javascript
const obj = {
    value:1,
    say(){
        console.log(this.value)
    }
}
obj.say()  // 输出1
```

所以我们自己去实现一个call或者apply的时候，可以分为三步来实现



**先加上在这个对象中加入这个方法，然后执行这个方法，再去删除这个方法**

```javascript
1:obj.say = say

2:obj.say()

3:delete obj.say
```

让我们开始吧~~~

#### 首先是call的实现

```JavaScript
const obj = {
    value: 1
}

function say(username, age) {
    console.log(username)
    console.log(age)
}

Function.prototype._call = function (context) {
    context.say = this
    console.log(context) // { value: 1, say: [Function: say] }
    const args = []
    for (let i = 1; i < arguments.length; i++) {
        args.push('arguments[' + i + ']')
    }
    
    console.log(args) // [ 'arguments[1]', 'arguments[2]' ]
    eval(`context.say(${args})`)
    delete context.say
}

say._call(obj, '郑嘉龙', 22)
// 在eval函数中，args会自动调用args.toString()方法，相当于context.say(arguments[1], arguments[2], ....)

// eval 函数接受的参数是一个字符串，并且会对立面的参数进行求值，可以把 eval 当做是一个<script>标签

```



但是到了这一步的话还是没完，因为我们有可能传入的参数会是 null，比如以下的情况

```javascript
const value = 1

function say() {
  console.log(this.value)
}

say.call(null) // 输出1，此时的this为window
```



所以我们要处理一下这种情况，把这个函数改进一下

```javascript
const obj = {
    value: 1
}

const value = 2

function say(username, age) {
    return {
        username,
        age
    }
}

Function.prototype._call = function (context) {
    var context = context || window  // 判断传进来的参数是否是 null
    console.log(this) //   say: [Function: say]
    context.say = this 
    console.log(context) // {value: ?, say: [Function: say]} 这里的value是不确定的，因为要看this的指向是window还是obj
    const args = []
    for (let i = 1; i < arguments.length; i++) {
        args.push(`arguments[${i}]`)
    }

    const result = eval(`context.say(${args})`)
    delete context.say
    return result
}

say._call(null)

console.log(say._call(obj, '郑嘉龙', 22))
// {username: "郑嘉龙", age: 22}
```

**注意这段代码必须放到浏览器环境下执行，因为node是没有window对象的，所以node环境执行是会报错的！！！**

#### 让我们来看下apply的实现吧

```javascript
const obj = {
    value: 1
}

const value = 2

function say(username, age) {
    return {
        username,
        age
    }
}

Function.prototype._apply = function (context, array) {
    var context = context || window
    console.log(context) // { value: 1 }
    console.log(array) // [ '郑嘉龙', 18 ]
    console.log(this)  // [Function: say]
    let result
    context.say = this

    if (array) {
        const args = []
        for (let i = 0; i < array.length; i++) {
            args.push(`array[${i}]`)
        }
        result = eval(`context.say(${args})`)
        delete context.say
        return result
    } else {
        result = context
        return result
    }

}

say._apply(null)

console.log(say._apply(obj, ['郑嘉龙', 18]))
// { username: '郑嘉龙', age: 18 }
```

在此感谢彭道宽师兄的帮助：<https://github.com/PDKSophia>





