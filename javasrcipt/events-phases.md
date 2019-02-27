#事件流

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
    事件流分别有三个执行阶段,捕获阶段/目标阶段/冒泡阶段;
    捕获阶段: 事件从根节点逐级往下执行直到找到触发事件的节点父节点,之后进入目标阶段事件执行;
    目标阶段: 节点注册的事件不论注册的冒泡还是捕获阶段的事件都按注册的顺序执行;
    冒泡阶段: 结束目标阶段后,从目标节点的父节点逐级触发事件;
    备注: 如果在事件流执行过程中删除或者添加节点事件流将按事件分发时确认的节点状态执行该事件

```
// 添加一个全局的点击事件
<div id="a">
    a
    <div id="a-a">
        a-a
        <div id="a-a-a">a-a-a</div>
    </div>
</div>

<script>
  document.addEventListener('click', event => {
    console.log('document - 冒泡 1')
  }, false)
  document.addEventListener('click', event => {
    console.log('document - 捕获 1')
  }, true)
  
  let aaaDom = document.getElementById('a-a-a')
  aaaDom.addEventListener('click', event => {
    console.log('aaaDom - 冒泡 1')
  }, false)
  aaaDom.addEventListener('click', event => {
    console.log('aaaDom - 捕获 1')
  }, true)
  
  let aaDom = document.getElementById('a-a')
  aaDom.addEventListener('click', event => {
    console.log('aaDom - 冒泡 1')
  }, false)
  aaDom.addEventListener('click', event => {
    console.log('aaDom - 捕获 1')
  }, true)
</script>

// 点击 #a-a-a 节点输出  document - 捕获 1 => aaDom - 捕获 1 => aaaDom - 冒泡 1 => aaaDom - 捕获 1 => aaDom - 冒泡 1 => document - 冒泡 1
```





  [1]: https://www.w3cschool.cn/jsref/met-document-addeventlistener.html
