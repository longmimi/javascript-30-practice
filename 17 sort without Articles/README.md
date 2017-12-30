

## 基本思路
1.基本的编程任务有两个要点，即**排序**和**展示**;<br>
2.数组排序部分最外层即为`Array.sort(arr)`函数，内层实现具体排序规则;<br>
3.展示部分即将排列好的新数组拼接成带有标签的HTML元素，然后一次性插入到列表中。

## 过程指南(以非ES6版本为例)
1.声明去前缀函数，使用`String.replace()`函数实现，第一参数使用字面量正则表达式。
```js
function delPrefix(item){
    return item.replace(/^(The|A|An)\s{1}/,'');
}
```
2.使用`Array.sort()`对数组进行排序，将数组中逐项使用`delPrefix()`去掉前缀后再进行对比。
```js
var sortedbands = bands.sort(function(a,b){
    return delPrefix(a) > delPrefix(b) ? 1 : -1;
});
```
3.使用选择器选中无序列表`#bands`，将排序后的数组作为列表项插入其中。
```js
 document.getElementById('bands').innerHTML = '<li>'+arr.join('</li><li>')+'</li>';
```

## 细节知识点
1.`Array.prototype.sort(*param*)`方法虽然有返回值，但排序结果也影响原数组，在非ES6版本的代码中，我们使用了指向原数组的变量`bands`,而在ES6版本的代码中将排序后的结果赋值给了新的变量sortedbands，从结果可以看出，两者达到了相同的目的。

2.在ES6版本的代码结尾处，我们修改原数组`bands`中的第一项，并在控制台打印出排序后的数组`sortedbands`，从结果可以看出`sortedbands`也受到了影响，由此可以看出`Array.prototype.sort()`函数只是返回了一个指向原数组的引用，而并没有生成新的数组。