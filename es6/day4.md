# ES6 函数的扩展

+ **函数参数的默认值**
  
  ES6 允许为函数的参数设置默认值，即**直接写在参数定义的后面**。
  
  1. `function log(x, y = 'World') {`
  2. `console.log(x, y);`
  3. `}`
     
     `log('Hello') // Hello World`
  
  参数变量是默认声明的， 用let 或 const **再次声明参数同名变量错误**。
  
  
  
  1. `// 不报错`
  
  2. `function foo(x, x, y) {`
  
  3. `// ...`
  
  4. `}`
  
  5. `// 报错`
  
  6. `function foo(x, x, y = 1) {`
  
  7. `// ...`
  
  8. `}`
  
  使用参数默认值时，函数不能有同名参数。
  
  
  
  注意：
  
  参数默认值是**惰性求值**的。
  
  参数默认值不是传值的，而是每次都重新计算默认值表达式的值。
  
  即参数不会当做一个变量保存多次值。
  
  + **与解构赋值默认值结合使用** 
    
    `参数默认值`可以与`解构赋值`的默认值，结合起来使用。
    
    1. `function foo({x, y = 5}) {`
    
    2. `console.log(x, y);`
    
    3. `}`
    
    4. `foo({}) // undefined 5`
    
    5. `foo({x: 1}) // 1 5`
    
    6. `foo({x: 1, y: 2}) // 1 2`
       
       
    
    {}在参数中代表了一个对象，这个写法可以指定对象内部的默认值。
    
    如果参数不写{}的话，则{}的整体默认值生效。
    
    1. `function fetch(url, { body = '', method = 'GET', headers = {} }) {`
    2. `console.log(method);`
    3. `}`
       
       
    4. `function fetch(url, { body = '', method = 'GET', headers = {} } = {}) {`
    5. `console.log(method);`
    6. `}`
       
       参数中{ xxx={} }={} 这里有些默认值有点嵌套的意思了。
       
       **双重默认值**还得好好理解一下。
       
       
    
    1. `// 写法一`
    
    2. `function m1({x = 0, y = 0} = {}) {`
    
    3. `return [x, y];`
    
    4. `}`
    
    5. `// 写法二`
    
    6. `function m2({x, y} = { x: 0, y: 0 }) {`
    
    7. `return [x, y];`
    
    8. `}`
       
       写法一参数默认值是空对象，但是设置了对象解构赋值的默认值。
       
       写法二函数参数的默认值是一个有具体属性的对象，但是没有设置对象解构赋值的默认值。
       
       开始有点绕了感觉。。
       
       
       
       总之就是等号左边是默认有的，如果左边没有设置默认并且等号右边还缺少赋值的话，那自然就是undefined了。
       
       `// x 和 y 都有值的情况`
    
    9. `m1({x: 3, y: 8}) // [3, 8]`
    
    10. `m2({x: 3, y: 8}) // [3, 8]`
    
    11. `// x 有值，y 无值的情况`
    
    12. `m1({x: 3}) // [3, 0]`
    
    13. `m2({x: 3}) // [3, undefined]`
  
  + **函数的 length 属性**
    
    指定了默认值以后，函数的 length 属性，将返回**没有指定默认值的参数个数**。
    
    也就是说，指定了`默认值`后， length 属性将`失真`。
    
    1. `(function (a, b, c = 5) {}).length // 2`
       
       只有c指定了默认值，所以返回2。
    
    不过这要求是尾参数，如果参数默认值后面还有参数，则不会计入length。
  
  + **作用域**
    
    1. `var x = 1;`
    
    2. `function f(x, y = x) {`
    
    3. `console.log(y);`
    
    4. `}`
    
    5. `f(2) // 2`
       
       可以看出，参数中设置默认值，**优先在自身作用域寻找**，而非全局变量，自身找不到后才会去外部寻找。
       
       一旦设置了参数默认值，参数会形成一个单独的`作用域（context）`。
  
  + **rest 参数**
    
    ES6 引入`rest 参数`（**形式为 ...变量名** ），用于**获取函数的多余参数**，这样就不需要使用 arguments 对象了。  
    
    1. ```js
       function add(...values) {
       let sum = 0;
       for (var val of values) {
         sum += val;
       }
       return sum;
       }
       add(2, 5, 3) // 10
       ```
    
    注意，**rest 参数之后不能再有其他参数**（即只能是最后一个参数），否则会报错。函数的 length 属性，不包括 rest 参数。
  
  + **严格模式**
    
    在函数体内首行写 'use strict';
    
    `ES2016`做了一点修改，规定只要函数参数使用了`默认值`、`解构赋值`、或者`扩展运算符`，那么函数内部就不能显式设定为严格模式，否则会报错。
  
  + **name 属性**
    
    函数的`name`属性，返回该函数的`函数名`。
    
    bind 返回的函数， name 属性值会加上 bound 前缀。
    
    ```js
    function foo() {};
    foo.bind({}).name // "bound foo"
    ```

+ **箭头函数**
  
  我就是被箭头函数吸引来的，这种写法太优雅了。
  
  `ES6`允许使用`“箭头”`（ => ）定义函数。（参数、箭头、返回值）
  
  先看一个等价的：
  
  - ```js
    var f = v => v;
    // 等同于
    var f = function (v) {
      return v;
    };
    ```
  
  如果箭头函数不需要参数或需要多个参数，就使用**一个`圆括号`代表参数部分**。
  
  箭头函数的代码块部分**多于一条语句**，就要使用大括号将它们括起来，并且使用 return 语句返回。
  
  `var sum = (num1, num2) => { return num1 + num2; }`
  
  如果箭头函数只有一行语句，且不需要返回值，可以采用下面的写法，就不用写大括号了。
  
  `let fn = () => void doesNotReturn();`
  
  + **`箭头函数`可以与`变量解构`结合使用。**
    
    ```js
    const full = ({ first, last }) => first + ' ' + last;
    // 等同于
    function full(person) {
      return person.first + ' ' + person.last;
    }
    ```
  
  + 箭头函数使得表达更加`简洁`。
    
    ```js
    const isEven = n => n % 2 === 0;
    const square = n => n * n;
    ```
    
    太优美了，简直是艺术。
    
    注意，等号后面的n是传进来的参数，如果不需要参数则用()，如果要多个参数，直接写(x,y,...)并且无需声明。
  
  + 箭头函数的一个用处是**简化回调函数**。
    
    ```js
    // 正常函数写法
    [1,2,3].map(function (x) {
      return x * x;
    }); 
    
    // 箭头函数写法
    [1,2,3].map(x => x * x);
    ```
    
    rest 参数与箭头函数结合的例子:
    
    ```js
    const numbers = (...nums) => nums;
    numbers(1, 2, 3, 4, 5)
    // [1,2,3,4,5] 
    
    const headAndTail = (head, ...tail) => [head, tail];
    headAndTail(1, 2, 3, 4, 5)
    // [1,[2,3,4,5]]
    ```
    
    **突然有点不理解rest参数了，记得再看看**
  
  + **箭头函数使用注意点**
    
    **箭头函数没有this**，所以this是定义的时候，外部所在的对象是他的this。
    
    （1）函数体内的 `this`对象，就是**定义时所在的对象**，而不是使用时所在的对象。
    
    （2）不可以当作`构造函数`，也就是说，**不可以使用 new 命令**，否则会抛出一个错误。
    
    （3）不可以使用`arguments`对象，该对象在函数体内不存在。如果要用，**可以用 rest 参数代替**。
    
    （4）不可以使用 `yield`命令，因此箭头函数**不能用作 Generator 函数**。
    
    上面四点中，**第一点尤其值得注意**。 this 对象的指向是可变的，但是在箭头函数中，它是固定的。
    
    
    
    <u>为什么setTimeout的this指向的是window对象？</u>
    
    因为setTimeout这个方法是挂载在window对象上的，即window.setTimeout();
    
    因此在setTimeout()里用到普通函数function的话，其this指向的就是window。
    
    而箭头函数正好起到了**固定this指向的作用**，其this指向了定义时所在的对象，这样就便于访问外层对象的属性了。并且箭头函数**只有一个this**，不论你嵌套了多少层。
    
    由于箭头函数没有自己的 this ，所以当然也就**不能用 call() 、 apply() 、 bind() 这些方法去改变 this 的指向**。
  
  + **不适用箭头函数场合**
    
    由于箭头函数使得 this 从`“动态”`变成`“静态”`，下面两个场合不应该使用箭头函数。
    
    **第一个场合是`定义对象`的方法**，且该方法内部包括`this`。
    
    ```js
    const cat = {
      lives: 9,
      jumps: () => {
        this.lives--;
      }
    }
    ```
    
    cat.jumps() 方法是一个箭头函数，这是错误的。this需要指向cat，而不是cat所在的函数对象。
    
    **第二个场合是需要动态 this 的时候**，也不应使用箭头函数。
    
    ```js
    var button = document.getElementById('press');
    button.addEventListener('click', () => {
      this.classList.toggle('on');
    });
    ```
    
    这是错误的。要使用普通函数，让this 动态指向被点击的按钮对象。

+ **尾调用优化**
  
  `尾调用`（Tail Call）是函数式编程的一个重要概念，就是指**某个函数的最后一步是调用另一个函数。**
  
  ```js
  function f(x){
    return g(x);
  }
  ```

+ **尾递归**
  
  函数调用自身，称为`递归`。如果尾调用自身，就称为`尾递归`。
  
  **递归非常耗`费内存`**，因为需要同时保存成千上百个调用帧，很容易发生`“栈溢出”`错误（stack overflow）。**但对于尾递归来说不是**，由于**只存在一个调用帧**，所以永远不会发生“栈溢出”错误。
  
  + **阶乘优化**   不会，得好好看看
  
  + **计算 Fibonacci 数列** 
    
    计算斐波那契数列也能充分说明尾递归优化的重要性 // 超时
    
    ```js
    //非尾递归
    function Fibonacci (n) {
      if ( n <= 1 ) {return 1};
      return Fibonacci(n - 1) + Fibonacci(n - 2);
    }
    Fibonacci(10) // 89
    Fibonacci(100) // 超时
    Fibonacci(500) // 超时
    
    
    //尾递归优化后
    function Fibonacci2 (n , ac1 = 1 , ac2 = 1) {
      if( n <= 1 ) {return ac2};
      return Fibonacci2 (n - 1, ac2, ac1 + ac2);
    }
    Fibonacci2(100) // 573147844013817200000
    Fibonacci2(1000) // 7.0330367711422765e+208
    Fibonacci2(10000) // Infinity
    ```
    
    由此可见，**`“尾调用优化”`对递归操作意义重大**，所以一些函数式编程语言将其写入了语言规格。ES6 中**只要使用`尾递归`，就不会发生栈溢出**，还能节省内存。
    
    **还是没看懂怎么写的！**

+ **递归函数的改写**
  
  尾递归的实现，往往需要改写递归函数，**确保最后一步只调用自身**。
  
  做到这一点的方法，就是**把所有用到的内部变量改写成函数的参数**。缺点就是不太直观。
  
  **两个方法**可以解决这个问题。
  
  方法一是在**尾递归函数之外，再提供一个正常形式的函数**。
  
  ```js
  function tailFactorial(n, total) {
    if (n === 1) return total;
    return tailFactorial(n - 1, n * total);
  }
  function factorial(n) {
    return tailFactorial(n, 1);
  }
  factorial(5) // 120
  ```
  
  上面代码通过一个正常形式的阶乘函数 factorial ，调用尾递归函数 tailFactorial ，看起来就正常多了。
  
  + **柯里化（currying）**
    
    意思是**将多参数的函数转换成单参数**的形式。这里也可以使用柯里化。
    
    ```js
    function currying(fn, n) {
      return function (m) {
        return fn.call(this, m, n);
      };
    }
    function tailFactorial(n, total) {
      if (n === 1) return total;
      return tailFactorial(n - 1, n * total);
    } 
    
    const factorial = currying(tailFactorial, 1); 
    
    factorial(5) // 120
    ```
    
    上面代码通过柯里化，将尾递归函数 tailFactorial 变为只接受一个参数的 factorial 。
  
  第二种方法就简单多了，就是**采用 ES6 的函数默认值**。
  
  ```js
  function factorial(n, total = 1) {
    if (n === 1) return total;
    return factorial(n - 1, n * total);
  } 
  
  factorial(5) // 120
  ```
  
  上面代码中，参数 total 有默认值 1 ，所以调用时不用提供这个值。
  
  纯粹的函数式编程语言 **没有循环操作命令**，**所有的循环都用递归实现**，这就是为什么尾递归对这些语言极其重要。

+ **尾递归优化的实现**
  
  ES6 的`尾调用`优化**只在`严格模式`下开启**，正常模式是无效的。
  
  正常模式下，或者那些不支持该功能的环境中，有没有办法也使用尾递归优化呢？回答是可以的，就是**自己实现尾递归优化**。
  
  尾递归优化的本质：防止调用栈太多，造成溢出。也就是说**采用`“循环”`换掉`“递归”`** 就可以实现功能意义上的“尾递归优化了”。
  
  **尾递归优化这块也需要深入研究，有些迷了！**

+ **函数参数的尾逗号**
  
  ES2017 [允许](https://github.com/jeffmo/es-trailing-function-commas)函数的最后一个参数有`尾逗号`（trailing comma）。
  
  1. `clownsEverywhere(`
  2. `'foo',`
  3. `'bar',`
  4. `);`

+ **Function.prototype.toString()**
  
  [ES2019](https://github.com/tc39/Function-prototype-toString-revision) 对函数实例的 `toString()`方法做出了修改。
  
  toString() 方法**返回函数代码本身一模一样的原始代码**，以前会省略注释和空格。

+ **catch 命令的参数省略**
  
  以前明确要求 catch 命令后面必须跟参数
  
  1. `try {`
  2. `// ...`
  3. `} catch (err) {`
  4. `// 处理错误`
  5. `}`
     
     现在时代变了，从[ES2019](https://github.com/tc39/proposal-optional-catch-binding)开始，允许 catch 语句省略参数。
     
     `try {`
  6. `// ...`
  7. `} catch {`
  8. `// ...`
  9. `}`
     
     用到了再写，挺好的。


