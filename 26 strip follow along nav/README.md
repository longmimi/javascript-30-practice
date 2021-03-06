## 编程思路  
监听每一个导航栏按钮绑定鼠标移入和移出的事件，在对应的回调函数中为相应的元素增加已经编写好的类名即可。

## 过程指南   
1.监听每一个导航栏按钮的鼠标移入移出事件
```js
var mainUl = document.querySelectorAll('.cool > li >a');
var navArr = Array.from(mainUl);
var bg = document.querySelector('.dropdownBackground');

navArr.map(function(item,index){
  item.onmouseover = function (){
        item.parentNode.classList.add('trigger-enter');
        item.parentNode.classList.add('trigger-enter-active');
        toggleBackground(index+1);
  }

  item.onmouseout = function (){
      item.parentNode.classList.remove('trigger-enter');
      item.parentNode.classList.remove('trigger-enter-active');
      toggleBackground();
  }
});
```
2.toggleBackground()函数为自定义函数，当传入整数参数时，根据对应的下拉菜单序号获取下拉菜单的尺寸，将绝对定位的白色背景衬底改编为对应大小，并移动至相应位置；当不传入任何参数时，表示鼠标没有悬停于任何按钮上，此时需要将下拉菜单的背景置为透明。        
```js
function toggleBackground(item){
    var itemPos;
    if(item === 1 || item === 2 || item ===3){
      //计算位置
    itemPos = document.querySelector('.dropdown'+item).getBoundingClientRect();
      bg.style.left = `${itemPos.left}px`;
      bg.style.top = `${itemPos.top-60}px`;
      bg.classList.add('open');
      bg.style.width = `${itemPos.width}px`;
      bg.style.height = `${itemPos.height}px`;
    }else{
      bg.style.height = 0;
      bg.style.width = 0;
      bg.classList.remove('open');
    }
}
```
  
## 延伸思考  
1.本例中将相应的样式编写在一个css类中，通过添加和移出类来实现需要的效果，js编程实践中非常推荐这种将DOM操作集中化的做法。      
2.除了使用js脚本来实现交互效果外，也可以使用下面的css来实现鼠标悬浮在按钮上时显示下拉菜单的效果（代码中`~`表示同级的元素,由于`index-start.html`中给定的结构中下拉菜单的白色沉底为独立元素，需要动态获取其对应尺寸，故使用js脚本更容易实现）   
```css
 .cool>li>a:hover ~.dropdown{
    display:block;
    opacity:1;
 }
```