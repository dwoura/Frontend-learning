# [ES6 中文教程_w3cschool](https://www.w3cschool.cn/escript6)

# 变量

+ **var**作用全局
  
  `“变量提升”`现象，即变量可以在声明之前使用，值为undefined。

+ **let** 有代码块区域，通过{ }括起代码块。
  
  如果用到了let，必须先用let声明变量才能用到该变量，否则先使用再声明，块中到声明前的这个区域叫作**暂时性死区**。
  
  多个let在相同作用域内，**不允许重复声明**同一个变量。

+ **const**常量，指的是这个地址中的数据不可更改，**必须声明的同时赋值**。

+ **function** ES5中var和function可声明变量。ES6中这两个是

+ **import**

+ **class**

#### 顶层对象的属性

`顶层对象`，在`浏览器`环境指的是`window对象`，在 `Node` 指的是`global对象`。`ES5`之中，**顶层对象的属性与全局变量是等价的。**

ES6为了保持兼容性，**var和function**声明的全局变量，依旧是**顶层**对象的属性；另一方面规定，**let、const、class**声明的全局变量，**不属于顶层**对象的属性。

ES2020引入**globalThis**作为顶层对象，任何环境下，`globalThis`都是`存在`的，都可以从它拿到顶层对象，指向全局环境下的this。

# 解构赋值

+ 不必再一个个赋值，可以这样`let [a, b, c] = [1, 2, 3];`  属于模式匹配。解构不成功则变量值是undefined。
  
  不完全解构，匹配一部分也可以。

+ 只要某种数据结构**具有`Iterator`接口**，都可以采用数组形式的解构赋值。
  
  Set也可以：`let [x, y, z] = new Set(['a', 'b', 'c']);`

+ **允许指定默认值** `let [foo = true] = [];`
  
  只有匹配的变量是undefined时，默认值才会生效，null不行。因为null不===undefined

+ **对象的解构赋值**
  
  `let { foo, bar } = { foo: 'aaa', bar: 'bbb' };`
  
  对象的属性没有次序，不过数组是有的。

+ **字符串也可以解构赋值**
  
  `const [a, b, c, d, e] = 'hello';`
  
  `let {length : len} = 'hello';`

+ **数值和布尔值的解构赋值**
  
  `let {toString: s} = 123;`
  
  `s === Number.prototype.toString // true`
  
  `let { prop: x } = undefined; // TypeError`
  
  `let { prop: y } = null; // TypeError` `undefined`和`null`无法转为对象

+ **函数参数解构赋值** 
  
  1. `function add([x, y]){`
  
  2. `return x + y;`
  
  3. `}`
     
     函数`add`的参数表面上是一个数组，但在传入参数的那一刻，**数组参数就被解构**成变量`x`和`y`
     
     `[[1, 2], [3, 4]].map(([a, b]) => a + b);` 这个例子也是。
     
     也可以使用**默认值**
