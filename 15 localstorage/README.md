# 遇到的问题
原作者是添加了一个删除全部的功能，我将这个改为了删除选中项目并删除，使用数组的forEach方法遍历数组判断是否勾选，并删除，但是发现并不能如我所愿，首先是用splice(index,1)来删除，发现每次只能删除所选元素的一半元素，一番查阅发现，forEach这个方法只是for的一个简化版，在每次循环过程中不能中断，因此这个index会改变，所以，我用了for循环逆向遍历，这样完美解决。还可以用filter()

###　关键点

- Event
    - [event.preventDefault](https://developer.mozilla.org/zh-CN/docs/Web/API/Event/preventDefault)
    - [eventTarget.addEventListener](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)
- [localStorage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage/LocalStorage)
    - `localStorage.setItem()`
    - `localStorage.getItem()`
- [JSON](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)
    - `JSON.stringify()`
    - `JSON.parse()`

## 过程指南

1. 默认情况下，在表单空间拥有焦点时按下 `Enter` 键或者点击提交按钮，会提交表单，提交时，浏览器会在发送请求给服务器之前触发 `submit` 事件，为了验证这个行为，我们可以添加事件监听后看看效果，此处先写一个处理函数：
    ```js
    function addItem(e) {
      console.log('hello');
    }

    addItems.addEventListener('submit', addItem);
    ```
    点击按钮后发现 `submit` 事件触发后的结果是， Console 中闪现 hello 后刷新整个页面，这是 `submit` 的默认行为，在当前的场景中不适用，所以我们需要先去除此事件的默认行为。
    ```js
    function addItem(e) {
      e.preventDefault();
    }
    ```

2. 下面我们开始改造 `addItem` 方法，以实现我们的功能。  
   首先在事件监听中，`this` 可以获取当前 form 即我们为其添加事件监听的 `addItem` 元素，所以可以借此方便的获取输入框中的值。在 `addItem()` 方法中获取输入，并构造一个对象 `item` 来存储这个信息，：
    ```js
    /* function addItem(){} 中 */
    const text = this.querySelector('[name=item]').value;
      const item = {
          text, // ES6中对 text: text, 的简写
          done: false // 为后续的勾选做准备
      }
    ```
3. 把对象放入此前声明好的数组 `items`，同时更新数据，包括页面中的 HTML 内容、LocalStorage。
    ```js
    /* function addItem(){} 中 */
    items.push(item);
    this.reset();// 以清空 input 中正在输入的值
    populateList(items, itemsList);
    localStorage.setItem('items', JSON.stringify(items));
    ```
    - **HTML 内容更新**  
        声明一个 `populateList` 方法来更新页面的内容。接收需要更新的数组作为参数，根据数组里的内容构造一组 `<li>` 组成的列表，并且加上一些标识信息，以助于之后需要实现的选中功能。
        ```js
        function populateList(plates = [], plateslist) {
            plateslist.innerHTML = plates.map((plate, i) => {
                return `
                  <li>
                      <input type="checkbox" data-index=${i} id="item${i}" ${plate.done ? 'checked' : ''} >
                      <label for="item${i}">${plate.text}</label>
                  </li>
                `;
            }).join('');
        }
        ```
    - **LocalStorage 更新**  
        我们利用 LocalStorage 把信息存到本地，从而可以保证刷新后内容不变。但使用 `localStorage` 的时候，直接把 `items` 传入得到的值是 [object Object]，所以需要在把数据传进去之前就把内容转换成 String 类型的数据，之后读取时也使用 `JSON.parse()` 来将数据转换成 JSON 格式。