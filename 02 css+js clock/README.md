### 关键点
这个指针的效果是首先由`date对象`获取到本地时间，然后在`setInterval`定时器中每秒改变一次指针状态，就是算出弧度然后改变，用`transform`:`rotate()`

### css部分
1. flex ：1
意为：
```css
.item{
  flex-grow:1; //属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大
  flex-shrink1； //定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小
  flex-basis:auto; //定义了在分配多余空间之前，项目占据的主轴空间（main size）。浏览器根据这个属性，计算主轴是否有多余空间。它的默认值为auto，即项目的本来大小。
}
```
2. 使用伪元素给表盘中心添加一个中心点
```css
.clock-face:after {   //绝对定位实现垂直水平居中，然后translate矫正回一半
        width: .8em;
        height: .8em;
        left: 50%;
        top: 50%;
        position: absolute;
        display: block;
        transform: translate(-50%, -50%);
        content: '';
        background-color: #a8c5d1;
        border-radius: 50%;
        box-shadow:
                0 0 0 2px rgba(0,0,0,0.1),
                0 0 10px rgba(0,0,0,0.2);
    }
```

