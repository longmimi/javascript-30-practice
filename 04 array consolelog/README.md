
## 实现效果

这一部分主要是熟悉 Array 的几个基本方法，其中有两个（filter、map）是 ES5 定义的迭代方法，这些迭代方法都有一个特点，就是对数组的每一项都运行给定函数，根据使用的迭代方法的不同，有不同的返回结果。

文档给出了一个初始操作的 inventor 数组，基于这个数组可以练习一下 Array 的各个方法，请打开 HTML 后在 Console 面板中查看输出结果。

## 炫酷的调试技巧

在 Console 中我们常用到的可能是 `console.log()` ，但它还有一个很炫的输出，按照表格来输出，效果如下：

```js
console.table(thing)
```

![console.table()](https://cl.ly/0H023s441o2d/Image%202016-12-23%20at%203.51.50%20PM.png)

## 过程指南

1. 筛选 16 世纪出生的发明家  
2. 展示他们的姓和名  
3. 把他们按照年龄从大到小进行排序
4. 计算所有的发明家加起来一共活了多少岁
5. 按照他们活了多久来进行排序
6. 筛选出一个网页里含有某个词语的标题
7. 按照姓氏来对发明家进行排序
8. 统计给出数组中各个物品的数量

## 相关知识

下面从简单的方法开始，后面有很多有意思的玩法。

### [filter](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)

过滤操作，有点像 SQL 里面的 select 语句。筛出运行结果是 true 的组成数组返回。

````js
const __fifteen = inventors.filter(function(inventor) {
  if (inventor.year >= 1500 && inventor.year < 1600 ) {
	  return true;
  } else {
      return false;
  }
});
console.table(__fifteen);
````	  

前面几篇已经提到过箭头函数，这里可以简化一下，用箭头函数来写，而且由于 if 语句的存在并不是必要的，可以写成下面这样：

````js
const fifteen = inventors.filter(inventor =>(inventor.year >= 1500 && inventor.year < 1600));
console.table(fifteen);
````

### [map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)

map 形象的理解就是，把数组中的每个元素进行处理后，返回一个新的数组。

例子如下：

````js 
// 展示数组对象  inventors 里发明家的姓名  
const fullNames = inventors.map(inventor => inventor.first + ' ' + inventor.last);
````

### [sort](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)

默认情况下，`Array.prototype.sort()` 会将数组以字符串的形式进行升序排列（10 会排在 2 之前），但 sort 也可以接受一个函数作为参数。所以需要对数字大小排序时需要自己设定一个比较函数，例子如下：

````js
const __ordered = inventors.sort((a, b) => (a > b) ? 1 : -1);
console.table(__ordered);
````

### filter 和 map 的结合使用

这两个结合起来可以做一些有意思的事情，由于示范代码中用的页面是 Wikipedia，我重新找了个国内的页面，演示如下：

筛选出这一个页面中包含 CSS 的书名。代码如下：
````js
  // https://book.douban.com/tag/web
  const cate = document.querySelectorAll('.subject-list h2 a');
  const book = links
			.map(link => link.title)
			.filter(title => title.includes('CSS'));
````

![豆瓣书单](https://cl.ly/3X2b3i3J4719/Image%202016-12-23%20at%2010.57.01%20AM.png)

除此之外，你还可以去自己个人订单的页面看一下今年买过的书，需要做的就是研究一下标签的 class 值或者是其他能够筛出来的标识信息，然后构造你自己的筛选语句。

需要提一点，由 `querySelectorAll()` 获取到的是一个 NodeList ，它并非是 Array 类型的数据，所以并不具有 `map` 和 `filter` 这样的方法，所以如果要进行筛选操作则需要把它转化成 Array 类型，使用下面示例之中的 `Array.from()` 来转化。

```js
var links = Array.from(document.querySelectorAll('#ordersContainer div.order div.a-row > a.a-link-normal'))

var object = order.map( order => {
var a = {};
var time = order.querySelector('.order-info span.value').textContent.trim();
var title = order.querySelector('div.a-row > a.a-link-normal').textContent.trim();
a["time"] = time;
return a;
})
```

![Amazon 订单筛选](http://ofjku7mlm.bkt.clouddn.com/16-12-23/51544750-file_1482484281402_1a92.png)

### [reduce](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

这是一个归并数组的方法，它接受一个函数作为参数（这个函数可以理解成累加器），它会遍历数组的所有项，然后构建一个最终的返回值，这个值就是这个累加器的第一个参数。例子如下：

````js
[0,1,2,3,4].reduce(function(previousValue, currentValue, index, array){
  return previousValue + currentValue;
});
````

而此处我们需要统计一个给定数组中各个项的值，恰好可以用到这个方法，在累加器之中，将统计信息存入一个新的对象，最后返回统计值。

````js
  const data = ['car', 'car', 'truck', 'truck', 'bike', 'walk', 'car', 'van', 'bike', 'walk', 'car', 'van', 'car', 'truck' ];
  const reduce = data.reduce( (obj, item) => {
	  if( !obj[item]  ) {
		  obj[item] = 0;
	  }
		  obj[item]++;
		  return obj;
  }, {});
  console.log(reduce);
````