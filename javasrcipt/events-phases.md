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

```
<table>
  <tr id="parent-node">
    <td id="target-node"> click me </td>
    <td></td>
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

// output: 'document - capture phase' > 'targetNode - capture phase' > 'targetNode - bubbling phase' 
```
[更多stopPropagation 执行Demo][3]


  [1]: https://www.w3cschool.cn/jsref/met-document-addeventlistener.html
  [2]: https://codepen.io/undefinedznn/pen/NJGxzy "事件流执行流程Demo"
  [3]: https://codepen.io/undefinedznn/pen/ywYJmp "stopPropagation&#40;&#41; 执行Demo"
