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
