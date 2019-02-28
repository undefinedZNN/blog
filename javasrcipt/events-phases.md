# 事件流

------

### 语法
```
document.addEventListener(event, function, useCapture)
```
#### 参数说明
| 参数        | 描述   |
| --------   | -----:  |
| event     | 必须。字符串，指定事件名。 |
| function  | 必须。指定要事件触发时执行的函数。|
| useCapture | 可选。布尔值，指定事件是否 在捕获或冒泡阶段执行。 |


    其实 addEventListener 第三个参数时也可以使用对象.

    在使用布尔值时false注册的事件在冒泡阶段(bubblingphase)执行,在使用true注册的事件在捕获阶段(capture phase)执行.

    如果useCapture使用对象参数有三个属性
        - capture:  布尔值，指定事件是否 在捕获或冒泡阶段执行。
        - once:  布尔值，表示事件是否只能执行一次。
        - passive:  布尔值，True永远不会调用 preventDefault()。如果 listener 仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。

[详细的API文档][1]

----------
### 事件流执行过程
    事件流分别有三个执行阶段,捕获阶段(capture phase)/目标阶段(target phase)/冒泡阶段(bubbling phase);
    
    捕获阶段: 事件从根节点逐级往下执行直到找到触发事件的节点父节点,之后进入目标阶段事件执行;
    
    目标阶段: 节点注册的事件不论注册的冒泡还是捕获阶段的事件都按注册的顺序执行;
    
    冒泡阶段: 结束目标阶段后,从目标节点的父节点逐级触发事件;
    
    备注: 如果在事件流执行过程中删除或者添加节点事件流将按事件分发时确认的节点状态执行该事件;

[事件流执行流程Demo][2]

![event phases process](https://raw.githubusercontent.com/undefinedZNN/blog/master/javasrcipt/assets/img/event-phases-process.jpg "事件流执行流程示意")

### 事件的阻止
    通常我们使用Event的stopPropagation()和stopImmediatePropagation()方法阻止冒泡和捕获事件的派发.
    
    stopPropagation方法是阻止事件往下一个节点派发但是它不能阻止已注册事件在当前节点继续派发.
    
    stopImmediatePropagation方法是阻止事件往下一个事件继续派发.
    
[更多事件阻止Demo][3]

```javascript
<table>
  <tr id="parent-node">
    <td id="target-node"> stopPropagation </td>
    <td id="target-node-1"> stopImmediatePropagation </td>
  </tr>
</table>

// 注册根节点事件
document.addEventListener('click', event => {
  console.log('document - capture phase')
}, true)
document.addEventListener('click', event => {
  console.log('document - bubbling phase')
}, false)

// 注册目标节点事件
let targetNode = document.getElementById('target-node')
targetNode.addEventListener('click', event => {
  event.stopPropagation()
  console.log('targetNode - capture phase')
}, true)
targetNode.addEventListener('click', event => {
  console.log('targetNode - bubbling phase')
}, false)

// 点击 #target-node 输出: 'document - capture phase' > 'targetNode - capture phase' > 'targetNode - bubbling phase' 

// 注册目标节点事件
let targetNode1 = document.getElementById('target-node-1')
targetNode1.addEventListener('click', event => {
  event.stopImmediatePropagation()
  console.log('targetNode1 - capture phase')
}, true)
targetNode1.addEventListener('click', event => {
  console.log('targetNode1 - bubbling phase')
}, false)

// 点击 #target-node-1 输出: 'document - capture phase' > 'targetNode1 - capture phase' 
```


  [1]: https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener
  [2]: https://codepen.io/undefinedznn/pen/NJGxzy "事件流执行流程Demo"
  [3]: https://codepen.io/undefinedznn/pen/ywYJmp "stopPropagation&#40;&#41; 执行Demo"
