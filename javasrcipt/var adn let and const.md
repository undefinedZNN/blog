### 什么是var, let, const ?
这还要问,  变/常量声明的修饰符啊!
附上阮老师的文章自己看
http://es6.ruanyifeng.com/#docs/let

### 那么我要讲什么
在讲var, let, const是什么的怎么用的文章一大把, 我在这也不赘述那么我要讲什么呢?
虽然互联网上充斥了很多解释var, let, const 但是看完后也就泛泛的了解下, 所有我想写些例子做个备忘,也供大家参考!

好开始撸了, 你们退开


```javascript
var a = 1
let b = 2
const c = 3
a++
console.log(a) // 2
b++
console.log(b) // 3
// const 是一个产量变量的修饰符所以赋值后不能修改,  下面code 试图修改这个产量所以抛出异常
c++ // throws Exception Uncaught TypeError: Assignment to constant variable.
```
从上面的代码可以看出用 const 声明的常量是只读的,一旦声明将无法修改

```javascript
const obj = {}
const arr = []
obj.a = 1
arr[0] = 1
console.log(obj) // {a: 1}
console.log(arr) // [1]
```
what the f##k ...
不是说一旦声明将无法修改, 其实了解赋值原理就不难解释了.
const 赋值时是将