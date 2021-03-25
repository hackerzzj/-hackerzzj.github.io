### js函数学习

- var squares = Array(9).fill(null); //创建长度为9的数组

- var squaresCope = squares.slice();  //返回一个新的数组，如果指定了start和end参数，则从start到end，否则会全部拷贝。

- var newPlayer = Object.assign({},player,{score : 2});  //`**Object.assign()**` 方法用于将所有可枚举属性的值从一个或多个源对象复制到目标对象。它将返回目标对象。第一个参数为目标对象，后面的都是源对象，源对象和目标对象中有相同的属性，源对象的属性会覆盖目标属性。

- array.map(function(currentValue,index,arr),thisValue)  数组中的每个元素都会执行这个函数,thisValue可以省略。

- 函数表达式会有一个name属性，当没有命名时会将分配变量的名字作为自己的隐式名。

- bind()、call()、apply()方法用法,都是用来重定义this这个对象的。[具体区别](https://www.runoob.com/w3cnote/js-call-apply-bind.html)

- es6 字符串拼接,可以配合反单引号完成拼接字符串的功能

  1、反单引号怎么打出来？ 
  将输入法调整为英文输入法，单击键盘上数字键1左边的按键。

  2、用法 
  step1： 定义需要拼接进去的字符串变量 
  step2： 将字符串变量用${}包起来，再写到需要拼接的地方

  3、示例代码：

  let a='Karry Wang';

  let str=`I love ${a}, because he is handsome.`;
  //注意：这行代码是用返单号引起来的

  alert(str);
  一定是用反单引号啊！不要写成单引号了！

- opacity属性 （让 DIV 元素半透明）

  元素的不透明度级别描述了透明度级别，当不透明度为 1 时表示完全不透明，当不透明度为 0.5 时表示半透明，当不透明度为 0 时表示完全透明。

- 定时器

  - setInterval() ：按照指定的周期（以毫秒计）来调用函数或计算表达式。方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。（循环定时器）
  - setTimeout() ：在指定的毫秒数后调用函数或计算表达式。（延时定时器）

- 数组添加删除

  - 添加在第一个：unshift(xxx)
  - 删除指定元素:splice(index,number,item)

- `  &&` 和 `||` 运算符使用短路逻辑（short-circuit logic），是否会执行第二个语句（操作数）取决于第一个操作数的结果。在需要访问某个对象的属性时，使用这个特性可以事先检测该对象是否为空：

```js
var name = o && o.getName();
```

- `switch` 和 `case` 都可以使用需要运算才能得到结果的表达式；在 `switch` 的表达式和 `case` 的表达式是使用 `===` 严格相等运算符进行比较的：

- ES2015 引入了更加简洁的 [`for`...`of`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of) 循环，可以用它来遍历可迭代对象，例如数组：

  ```js
  for (const currentValue of a) {
    // Do something with currentValue
  }
  ```

- ECMAScript 5 增加了另一个遍历数组的方法，`forEach()`：

  ```js
  ["dog", "cat", "hen"].forEach(function(currentValue, index, array) {
    // Do something with currentValue or array[index]
  });
  ```

- 数组函数 

  shift   \ unshift  \ push  \ pop  \ concat \join  \ length  \ splice \ 

- js取非行间样式

  - getComputedStyle(obj,false)['attr']方法
  - div.currentStyle['width'];方法

- dom节点

  - childNodes  \ nodeType
  - children  //好用

- String
  
  - search方法
  
- 事件冒泡

  - event.cancelBubble=true
  - 鼠标位置  clinetX,clientY
  - 键盘按键  keyCode
  - 其他属性  boolean值   ctrlKey  shiftKey   altKey

- 默认行为

  - documemnt.oncontextmenu=()=>{return false;}
  - otxt.onkeydown=()=>{return false}
  - 计算窗口的高度document.documentElement.clientHeight
  - 计算物体自己的高度 document.documentElement.clientWidth

- 事件绑定

  - attachEvent('onclick',函数)
  - addEventListener('click',函数,false)

- 事件捕获

  - setCapture()  ie浏览器下

- AJAX

  - 创建Ajax对象  var oAjax = new XMLHttpRequest();

  - 链接服务器   oAjax.open()     |   open(方法,文件名,异步传输)

  - 发送请求   oAjax.send()

  - 接受返回值  oAjax.onreadystatechange = function(){

    oAjax.readystate // 浏览器和服务器进行到哪一步了 

  - };

  - 看看是否成功  oAjax.status==200

  - 查看返回的文本   oAjax.responseText

- 原型

  - prototype   | 用法   类.prototy.属性  (集体添加上属性)

- 继承

  - A继承B的属性     在A的函数中添加B.call(this)
  - A继承B的方法      A.prototype=B.prototype;

- Generator函数(函数名后有*号,用yeild表达式来表示)用来生成iterator表达式 
```js 
function* myGenerator(){
    yeild 'hello';
    yeild 'generator';
}
let MG = myGenerator(); // 这里生成一个Generator函数
MG.next();// 第一种方法
for( var a of MG){
    console.log(a)
}
```

- promise函数(可以保存状态，pending,从pending变为fulfilled
  从pending变为rejected)

```js
let p1 = new Promise(function(resolve,reject){
    $.ajax({
        url:'data/json.txt',
        dataType: 'json',
        success(arr){
            resolve(arr);
        },
        error(err){
            reject(err);
        }
    })
})
p1.then(function(arr){
    alert('成功了'+arr);
},function(){
    alert('失败了')；
})
```

- export和module.export(还是不太了解)

  ```javascript
  exports 是 module.exports 的一个引用
  module.exports 初始值为一个空对象 {}，所以 exports 初始值也是 {}
  require 引用模块后，返回的是 module.exports 而不是 exports!!!!!
  exports.xxx 相当于在导出对象上挂属性，该属性对调用模块直接可见
  exports = 相当于给 exports 对象重新赋值，调用模块不能访问 exports 对象及其属性
  如果此模块是一个类，就应该直接赋值 module.exports，这样调用者就是一个类构造器，可以直接 new 实例。
  ```


- html 中rel属性

  | 值         | 描述                                                         |
  | ---------- | ------------------------------------------------------------ |
  | alternate  | 文档的可选版本（例如打印页、翻译页或镜像）。                 |
  | stylesheet | 文档的外部样式表。                                           |
  | start      | 集合中的第一个文档。                                         |
  | next       | 集合中的下一个文档。                                         |
  | prev       | 集合中的前一个文档。                                         |
  | contents   | 文档目录。                                                   |
  | index      | 文档索引。                                                   |
  | glossary   | 文档中所用字词的术语表或解释。                               |
  | copyright  | 包含版权信息的文档。                                         |
  | chapter    | 文档的章。                                                   |
  | section    | 文档的节。                                                   |
  | subsection | 文档的子段。                                                 |
  | appendix   | 文档附录。                                                   |
  | help       | 帮助文档。                                                   |
  | bookmark   | 相关文档。                                                   |
  | nofollow   | Google 使用 "nofollow"，用于指定 Google 搜索引擎不要跟踪链接。 |

  

### js解构

​	可以查看https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment

### React.Fragment（简写<></>）

​	片段(fragments) 可以让你将子元素列表添加到一个分组中，并且`不会在DOM中增加额外节点`（还可以使用返回数组的方式来解决。）;您可以像使用其他元素一样使用`<></>`，只是它不支持 键(keys) 或 属性(attributes)。

### 插槽(Portals)

```javascript
ReactDOM.createPortal(child, container)
```

​	第一个参数（child）是任何可渲染的 React 子元素，例如一个元素，字符串或 片段(fragment)。第二个参数（container）则是一个 DOM 元素。

`	对于 portal 的一个典型用例是当父组件有 overflow: hidden 或 z-index 样式，但你需要子组件能够在视觉上 “跳出(break out)” 其容器。`例如，对话框、hovercards以及提示框。

### 

​	