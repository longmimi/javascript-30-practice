### 关键点
1. #### 键盘事件
在window上添加键盘的`keydown`事件，通过`data-*`属性存储键码，用`属性选择器`来获取和对应建码相同的元素，还用了`模板字面量`.
```css
const keys = document.querySelector(`div[data-key="${e.keyCode}"]`)
```

2. #### audio元素
通过`<audio>`元素将声音文件添加，同时设置`data-*`属性
`<audio>`元素的`currentTime`属性将声音文件时间戳置0

3. #### transtionend事件

`transtion`事件结束后会触发`transtionend`事件，利用这个事件，可以在打鼓样式添加完之后去除这个样式
4. 其他
 * `addEventListener()`方法第三个参数默认为`false`，表示以`事件冒泡`执行，`true`则以`事件捕获`
 * ES6 实现forEach()
 ```javascript
 arr.forEach(key => {...}) //ES6
 arr.forEach(function(key){...})  //ES5
```

 5. CSS部分
使用了一种单位`vh`,这是视窗(`viewport`)单位,视窗是我们浏览器实际可见的区域，不包括工具栏和托盘部分。
假如说有一个`1000px` * `800px` 的视窗，那么
* vw——代表视窗(Viewport)的宽度为1%，50vw = 500px。
* vh——窗口高度的百分比 50vh = 400px。
* vmin——vmin的值是当前vw和vh中较小的值。在我们的例子里因为是横向模式，所以50vmin = 400px。
* vmax——大尺寸的百分比。50vmax = 500px。