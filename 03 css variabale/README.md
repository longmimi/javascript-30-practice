### Update CSS Variables with JS
使用css的变量功能，通过js动态改变变量的方式来改变图片的样式
#### 关键点
1. css variables
这是css的新功能，基本语法如下：
* 声明：在变量名前面加 `--`，注：大小写敏感
  ```css
  :root{
    --color1:#000bbb
    --size: 'px'
  }
  ```
* `var()`函数：变量使用的方式是`var(--size)`,方法的第二个参数是变量的默认值，当变量不存在时，就会使用这个默认值
* 变量的类型：可以是`字符串`，也可以是`带单位的值`，`字符串`可以与其他字符串`拼接`，但是数值不能，必须使用`calc()函数`
  ```css
  //eg:
  .foo {
    --gap: 20;
    margin-top: calc(var(--gap) * 1px);
  }
  ```
* 作用域：变量是有作用域的，就是声明变量大括号之前的元素
* js操作css 变量：

  ```javascript
  // 设置变量
  document.body.style.setProperty('--primary', '#7F583F')

  // 读取变量
  document.body.style.getPropertyValue('--primary').trim()
  // '#7F583F'

  // 删除变量
  document.body.style.removeProperty('--primary')
  ```