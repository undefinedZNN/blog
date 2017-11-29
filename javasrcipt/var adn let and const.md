### var, let, const 之间的区别
本文大量参考阮老师的文字, 目的是精简提炼下也做个笔记备忘,也供大家参考!
附上阮老师的文章自己看
http://es6.ruanyifeng.com/#docs/let

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
obj = arr //throws Exception Uncaught TypeError: Assignment to constant variable.
```
what the f##k ...
不是说一旦声明将无法修改, 其实了解赋值原理就不难解释了.
const 赋值时指向的内存不可改变,简单类型的数据是可以保证数据不被改动, 但是对于复合类型（主要是对象和数组）其指向的内存是其存储内容的指针

```javascript
let a = 1
var b = 2
a = 2
b = 3
```
这两家伙有什么区别吗, 答案肯定是有的, 主要的区别在其作用域

```javascript
if (true) {
  let a = 1
  var b = 2
  console.log(a) // 1
  console.log(b) // 2
}
console.log(a) //throws Exception Uncaught ReferenceError: a is not defined
console.log(b) // 2
```
变量 b 被提升了, 而变量 a 其作用访问仅限于 if 语句块内

```javascript
let x = 1
let y = 1
let z = 1
if (true) {
  console.log(y) // 1
 
  let z = 2
  console.log(z) // 2
 
  console.log(x) // throws Exception Uncaught ReferenceError: a is not defined
  let x = 2
}

```
// 在一个作用域类使用let const 声明变/常量之前, 变/常量是无法使用的这个叫做“暂时性死区”（temporal dead zone，简称 TDZ）

```javascript
var x = 1
var x = 2
let y = 1
let y = 2 // throws Exception Uncaught SyntaxError: Identifier 'y' has already been declared
```
// 在一个作用域类使用let const 声明变/常量是不可以重复声明的

```javascript
for (var x = 0; x < 10; x++) {
  setTimeout(function(){
    console.log(x)
  }, 100)
}
// 10 10 10 10 10 10 10 10 10 10
for (let x = 0; x < 10; x++) {
  setTimeout(function(){
    console.log(x)
  }, 100)
}
// 0 1 2 3 4 5 6 7 8 9
```
**划重点: for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域**