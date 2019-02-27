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
    事件流分别有三个执行阶段,捕获阶段(capture phase)/目标阶段(![target phase][2])/冒泡阶段(bubbling phase);
    捕获阶段:事件从根节点逐级往下执行直到找到触发事件的节点父节点,之后进入目标阶段事件执行;
    目标阶段: 节点注册的事件不论注册的冒泡还是捕获阶段的事件都按注册的顺序执行;
    冒泡阶段: 结束目标阶段后,从目标节点的父节点逐级触发事件;
    备注: 如果在事件流执行过程中删除或者添加节点事件流将按事件分发时确认的节点状态执行该事件;

![event phases process](https://raw.githubusercontent.com/undefinedZNN/blog/master/javasrcipt/assets/img/event-phases-process.jpg "事件流执行流程示意")

  [1]: https://www.w3cschool.cn/jsref/met-document-addeventlistener.html
