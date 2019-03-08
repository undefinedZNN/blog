---
typora-root-url: assets\img
typora-copy-images-to: assets\img
---

# javascript event loop

​	js 是单线程执行的语言(现在版本的js也可以通过新建子进程实现多线程但是有诸多约束这边就不展开了). 单线程相对于多线程的优势有减少了多线程中的复杂的同步逻辑.

​	单线程的缺点无法高效的使用计算机多余的计算能力,待执行的任务必须等待执行中的任务执行完成,其实上面几个缺点都还好主要是,一些I/O任务在占用线程且闲置计算资源如ajax. 但是JavaScript语言的设计者意识到，这时主线程完全可以不管IO任务直接跳过当前任务继续执行下一个任务,在I/O返回结果后在回来执行责任任务.

​	所以js中的任务分成了异步/同步两种任务,js中的额同步任务是在js的执行栈中排队依次执行, js的异步任务js也有个任务队列当任务队列中默认任务可以执行了就通知主线程将任务放进执行栈依次执行.

​	主线程从"任务队列"中读取事件，这个过程是循环不断的，所以整个的这种运行机制又称为Event Loop（事件循环）。

​	下图是js event loop示意图, js进程先执行执行栈内的任务, 执行栈内任务执行完成后再去任务队列(队列分位宏任务 和微任务队列)内读取任务执行,任务栈内的任务是由I/O ,timer, xhr UI交互事件等操作后将任务添加入任务栈如此循环直至J进程结束

![event-loop](/event-loop.png)



### 宏任务

(macro)task（又称之为宏任务），可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。

浏览器为了能够使得JS内部(macro)task与DOM任务能够有序的执行，**会在一个(macro)task执行结束后，在下一个(macro)task 执行开始前，对页面进行重新渲染**，流程如下：

```
(macro)task->渲染->(macro)task->...
```

(macro)task主要包含：script(整体代码)、setTimeout、setInterval、I/O、UI交互事件、postMessage、MessageChannel、setImmediate(Node.js 环境)

## 微任务

microtask（又称为微任务），**可以理解是在当前 task 执行结束后立即执行的任务**。也就是说，在当前task任务后，下一个task之前，在渲染之前。

所以它的响应速度相比setTimeout（setTimeout是task）会更快，因为无需等渲染。也就是说，在某一个macrotask执行完后，就会将在它执行期间产生的所有microtask都执行完毕（在渲染前）。

microtask主要包含：Promise.then、MutaionObserver、process.nextTick(Node.js 环境)

## 运行机制

在事件循环中，每进行一次循环操作称为 tick，每一次 tick 的任务[处理模型](https://www.w3.org/TR/html5/webappapis.html#event-loops-processing-model)是比较复杂的，但关键步骤如下：

- 执行一个宏任务（栈中没有就从事件队列中获取）
- 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
- 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
- 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
- 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）