

## 基本知识
`text-shadow`样式为标准CSS3样式，用于添加**一个或多个**文字阴影，用于其的语法格式为：
```css
text-shadow: h-shadow v-shadow blur color

```

## 过程指南
1.在`script`标签中，我们使用3个变量，一个指向`div`元素，一个指向其子元素`h1`，最后一个变量`factor`用于标记阴影距离`h1`中心的距离和鼠标距离`h1`中心距离的比例，用于计算阴影的具体位置。

2.在`hero`元素上监听鼠标移动事件`mousemove`，并添加事件处理的回调函数`shadowMove`.
```js
hero.addEventListener('mousemove',shadowMove);
```

3.为获得第一个阴影的瞬时位置，需要通过鼠标位置距离`h1`中心的距离乘以`factor`系数来获得，`pos`表示鼠标当前位置的坐标,range指代`hero`元素的宽和高：
```js
    var disX = parseInt((pos.x-range.x/2)*factor);
    var disY = parseInt((pos.y-range.y/2)*factor);
```
4.从事件发生的event对象中获取需要的值：
```js
    var range = {
      x:hero.offsetWidth,
      y:hero.offsetHeight
    }
    var pos = {
      x:e.target.offsetLeft+e.offsetX,
      y:e.target.offsetTop+e.offsetY
    }
```
5.计算出`h1`元素第一个阴影位置后，可以以坐标镜像或旋转90°等不同的方式来生成其他阴影，本例中我们采用绕`h1`元素中心旋转90°的方式共生成4个阴影：
```js
    text.style.textShadow = `
      ${xWalk}px ${yWalk}px 0 rgba(255,0,255,0.7),
      ${xWalk * -1}px ${yWalk}px 0 rgba(0,255,255,0.7),
      ${yWalk}px ${xWalk * -1}px 0 rgba(0,255,0,0.7),
      ${yWalk * -1}px ${xWalk}px 0 rgba(0,0,255,0.7)
    `;
```
