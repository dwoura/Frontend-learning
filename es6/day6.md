

# ES6 对象的扩展

+ **属性的简洁表示法** 
  
  ```js
  //大括号里面，直接写入变量和函数，作为对象的属性和方法。
  const foo = 'bar';
  const baz = {foo};
  baz // {foo: "bar"}
  
  // 等同于
  const baz = {foo: foo};
  ```
  
  这个真不知道，得记住。变量直接放进对象里面居然直接成了属性名对应属性值了。
  
  ```js
  function f(x, y) {
    return {x, y};
  }
  // 等同于
  function f(x, y) {
    return {x: x, y: y};
  }
  f(1, 2) // Object {x: 1, y: 2}
  ```
  
  除了属性简写，**方法也可以简写**。
  
  ```js
  const o = {
    name: '张三',
    birth,
    method() {
      return "Hello!";
    }
  };
  // 等同于
  const o = {
    name: '张三',
    birth: birth,
    method: function() {
      return "Hello!";
    }
  };
  ```
  
  这种写法**用于函数的返回值**，将会**非常方便**。记得是对象的哦。
  
  ```js
  function getPoint() {
    const x = 1;
    const y = 10;
    return {x, y};
  }
  getPoint()
  // {x:1, y:10}
  ```
  
  CommonJS 模块输出一组变量，就非常合适使用简洁写法。
  
  例如：`module.exports = { getItem, setItem, clear };`//这里面是函数名
  
  注意，简写的对象方法**不能用作构造函数**，会报错。

+ **属性名表达式**
  
  js定义对象属性有两种方法：
  
  ```js
  // 方法一 直接用标识符作为属性名
  obj.foo = true;
  // 方法二 用表达式作为属性名
  obj['a' + 'bc'] = 123;
  ```
  
  表达式还可以用于定义方法名。
  
  ```js
  let obj = {
    ['h' + 'ello']() {
      return 'hi';
    }
  };
  obj.hello() // hi
  ```
  
  注意，**属性名表达式与简洁表示法，不能同时使用** ，会报错。

+ **方法的 name 属性**
  
  函数的`name`属性，返回`函数名`。
  
  如果对象方法用了getter和setter，则这个name属性就改挂载在描述对象的get和set属性上面了。返回值是方法名前加上 get 和 set 。
  
  ```js
  const obj = {
    get foo() {},
    set foo(x) {}
  };
  obj.foo.name
  // TypeError: Cannot read property 'name' of undefined 
  
  const descriptor = Object.getOwnPropertyDescriptor(obj, 'foo');
  descriptor.get.name // "get foo"
  descriptor.set.name // "set foo"
  ```
  
  如果对象的方法是一个 Symbol 值，那么 name 属性返回的是这个 Symbol 值的描述。
  
  ```js
  const key1 = Symbol('description');
  const key2 = Symbol();
  let obj = {
    [key1]() {},
    [key2]() {},
  };
  obj[key1].name // "[description]"
  obj[key2].name // ""
  ```

+ **属性的可枚举性和遍历** 
  
  + **可枚举性**
    
    对象的每个属性都有一个`描述对象`（Descriptor），用来控制该属性的行为。 `Object.getOwnPropertyDescriptor`方法可以获取该属性的描述对象。
    
    描述对象里的一个叫 enumerable 的属性，称为“可枚举性”，如果该属性为 false ，就表示某些操作会忽略当前属性。
    
    引入`“可枚举”`（ enumerable ）这个概念的最初目的，就是**让某些属性可以规避掉 for...in 操作**，不然所有内部属性和方法都会被遍历到。比如，对象原型的 toString 方法，以及数组的 length 属性，就通过`“可枚举性”`，从而避免被 for...in 遍历到。
    
    ES6 规定，**所有 Class 的原型的方法都是不可枚举的**。
    
    总的来说，操作中引入继承的属性会让问题复杂化。大多数时候，我们只关心对象自身的属性。所以，尽量不要用 for...in 循环，**而用 Object.keys() 代替**。

+ **属性的遍历** 
  
  + **for...in**
  
  + **Object.keys(obj)** 
  
  + **Object.getOwnPropertyNames(obj)** 
  
  + **Object.getOwnPropertySymbols(obj)** 
  
  + **Reflect.ownKeys(obj)** 
  
  以上的 5 种方法遍历对象的`键名`，都遵守同样的属性遍历的**次序规则**。
  
  - 首先遍历所有**数值键**，按照数值升序排列。
  - 其次遍历所有**字符串键**，按照加入时间升序排列。
  - 最后遍历所有 **Symbol 键**，按照加入时间升序排列。

+ **super 关键字** 
  
  `this`关键字总是指向函数所在的`当前对象`，ES6 又新增了另一个类似的关键字 `super` ，**指向当前对象的`原型对象`** 。
  
  - ```js
    const proto = {
      foo: 'hello'
    };
    const obj = {
      foo: 'world',
      find() {
        return super.foo;
      }
    };
    Object.setPrototypeOf(obj, proto);
    obj.find() // "hello"
    ```
  
  注意， super 关键字表示原型对象时，**只能用在对象的方法之中** 

+ **对象的扩展运算符**
  
  之前在数组扩展中有介绍过...。ES2018将这个运算符引入了对象。
  
  + **解构赋值** 
    
    对象的`解构赋值`用于从一个对象`取值`，相当于将目标对象自身的所有可遍历的`（enumerable）`、但尚未被读取的属性，分配到指定的对象上面。
    
    ```js
    let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
    x // 1
    y // 2
    z // { a: 3, b: 4 }
    ```
    
    变量 **z 是解构赋值所在的对象**，获取等号右边所有还没读取的键值，然后一块拷贝过来。
    
    **undefined 和 null 无法转为对象**。
    
    **解构赋值必须是最后一个参数。**
    
    注意，`解构赋值`的拷贝是 `浅拷贝`  ：对于键为复合类型而言。
    
    扩展运算符的解构赋值，**不能复制继承自原型对象的属性**。
    
    ```js
    let o1 = { a: 1 };
    let o2 = { b: 2 };
    o2.__proto__ = o1;
    let { ...o3 } = o2;
    o3 // { b: 2 }
    o3.a // undefined
    ```
    
    上面代码中，对象 o3 复制了 o2 ，但是只复制了 o2 自身的属性，没有复制它的原型对象 o1 的属性。这也算“浅复制”吗？

+ **扩展运算符**
  
  对象的`扩展运算符`（ ... ）用于取出参数对象的所有`可遍历`属性，拷贝到当前对象之中。
  
  对象的扩展运算符**等同于使用 `Object.assign()` 方法**。
  
  ```js
  let aClone = { ...a };
  // 等同于
  let aClone = Object.assign({}, a);
  ```
  
  上面的例子只是拷贝了对象实例的属性，如果想**完整克隆一个对象，还要拷贝对象原型的属性**。可以采用下面的写法：
  
  ```js
  // 写法一 可能有些浏览器没部署proto属性
  const clone1 = {
    __proto__: Object.getPrototypeOf(obj),
    ...obj
  };
  // 写法二
  const clone2 = Object.assign(
    Object.create(Object.getPrototypeOf(obj)),
    obj
  );
  // 写法三
  const clone3 = Object.create(
    Object.getPrototypeOf(obj),
    Object.getOwnPropertyDescriptors(obj)
  )
  ```
  
  扩展运算符可以用于合并两个对象。
  
  如果用户自定义的属性，**放在扩展运算符后面**，则扩展运算符内部的同名属性会被覆盖掉。这**用来修改现有对象部分的属性就很方便了**。直接向后覆盖！
  
  如果把自定义属性放在**扩展运算符前面**，就变成了设置**新对象的默认属性值**。
  
  扩展运算符的参数对象之中，如果有取值函数 get ，这个函数是会执行的：
  
  ```js
  let a = {
    get x() {
      throw new Error('not throw yet');
    }
  }
  let aWithXGetter = { ...a }; 
  // 报错 取值函数 get 在扩展 a 对象时会自动执行，导致报错。
  ```

+ **链判断运算符** 
  
  在实际编程中，如果读取对象内部的某个属性，**往往需要判断一下该对象是否存在**。比如，要读取 message.body.user.firstName ，安全的写法是写成下面这样。
  
  ```js
  const firstName = (message
    && message.body
    && message.body.user
    && message.body.user.firstName) || 'default';
  ```
  
  这样的层层判断非常麻烦，因此 [ES2020](https://github.com/tc39/proposal-optional-chaining) 引入了“链判断运算符”（optional chaining operator） **?.** 。
  
  ```js
  const firstName = message?.body?.user?.firstName || 'default';
  const fooValue = myForm.querySelector('input[name=foo]')?.value
  ```
  
  直接在链式调用的时候判断，左侧的对象是否为 null 或 undefined 。如果是的，就不再往下运算，而是返回 undefined 。
  
  下面是判断**对象方法**是否存在，**如果存在就立即执行的例子**。
  
  `iterator.return?.()`
  
  对于那些可能没有实现的方法，这个运算符很有用。
  
  比如：
  
  ```js
  if (myForm.checkValidity?.() === false) {
    // 表单校验失败
    return;
  }
  ```
  
  老式浏览器的表单可能没有 `checkValidity` 这个方法，所以?.()就能先判断有没有这个方法，如果没有就会出现undefined === false ，所以就会跳过上面的代码。
  
  ```js
  a?.()
  // 等同于
  a == null ? undefined : a()
  ```
  
  使用这个运算符，有几个注意点：
  
  + **短路机制**
  
  + **delete 运算符**
    
    ```js
    delete a?.b
    // 等同于
    a == null ? undefined : delete a.b
    ```
    
    delete运算符也遵循 ?. 
  
  + **括号的影响**
    
    如果属性链有圆括号，只对圆括号内部有影响。
    
    ```js
    (a?.b).c
    // 等价于
    (a == null ? undefined : a.b).c
    ```
    
    不管 a 对象是否存在，圆括号后面的 .c 总是会执行。但一般来说用了?.就不该用圆括号。
  
  + **报错场合**
    
    ```js
    // 构造函数
    new a?.()
    new a?.b()
    // 链判断运算符的右侧有模板字符串
    a?.`{b}`
    a?.b`{c}`
    // 链判断运算符的左侧是 super
    super?.()
    super?.foo
    // 链运算符用于赋值运算符左侧
    a?.b = c
    ```
  
  + **右侧不得为十进制数值** 
    
    很简单，就是?.运算符有小数点，会导致后面的十进制数字被判断成小数。

+ **Null 判断运算符** 
  
  读取对象属性的，如果null 或 undefined，有时要指定默认值。
  
  旧常见做法是**通过 || 运算符指定默认值**。
  
  [ES2020](https://github.com/tc39/proposal-nullish-coalescing) 引入了一个**新的 Null 判断运算符 ??**。它的行为类似 || ，但是**只有运算符左侧的值为 null 或 undefined 时**，才会返回右侧的值。
  
  ```js
  const headerText = response.settings.headerText ?? 'Hello, world!';
  ```
  
  只有运算符左侧null 或 undefined 时，才会返回右侧的值。
  
  
  
  ??运算符的一个目的，就是跟链判断运算符 ?. 配合使用。
  
  `const animationDuration = response.settings?.animationDuration ?? 300;`
  
  response.settings 如果是 null 或 undefined ，就会返回默认值300。
  
  + ??运算符很**适合判断函数参数是否赋值**。
    
    ```js
    function Component(props) {
      const enable = props.enabled ?? true;
      // …
    }
    ```
  
  + 优先级规则
    
    现在的规则是，如果多个逻辑运算符一起使用，**必须用括号表明优先级**，否则会报错。
    
    代码直观才是硬道理！！！



# ES6 对象的新增方法

+ **Object.is()** 

+ **Object.assign()** 

+ **Object.getOwnPropertyDescriptors()** 

+ **proto 属性，Object.setPrototypeOf()，Object.getPrototypeOf()** 
  
  `__proto__` 属性（**前后各两个下划线**），用来读取或设置当前对象的`原型对象`（prototype）。
  
  **proto** 前后的双下划线，**说明它本质上是一个`内部属性`**，而不是一个正式的对外的 API，只是因为浏览器广泛支持才加入了es6。
  
  因此，无论从语义的角度，还是从兼容性的角度，都不要使用这个属性。
  
  而是使用下面的 `Object.setPrototypeOf()`（写操作）、 `Object.getPrototypeOf()`（读操作）、 `Object.create()` （生成操作）代替。
  
  实现上， `__proto__` 调用的是`Object.prototype.__proto__`。
  
  
  
  + **Object.setPrototypeOf() / Object.getPrototypeOf()**
    
    作用与 `__proto__`相同，用来设置与获取一个对象的`原型对象`（prototype），返回参数对象本身。**ES6 正式推荐的设置原型对象的方法。**
    
    ```js
    // 格式
    Object.setPrototypeOf(object, prototype)
    // 用法
    const o = Object.setPrototypeOf({}, null); 
     
    //等同于这个函数
    function setPrototypeOf(obj, proto) {
      obj.__proto__ = proto;
      return obj;
    }
    ```

+ **Object.keys()，Object.values()，Object.entries()**
  
  用于**操作对象**的三个常用方法，这三种方法只能遍历可枚举的属性。
  
  + **Object.entries()** 
    
    `Object.entries` 的基本用途是遍历对象的属性。
    
    `Object.entries` 方法的另一个用处是，将`对象`转为真正的 `Map 结构`。

+ **Object.fromEntries()** 
  
  `Object.fromEntries()`方法是`Object.entries()`的逆操作，用于**将一个键值对数组转为对象**。
  
  特别适合将 Map 结构转为对象！
  
  ```js
  // 例一
  const entries = new Map([
    ['foo', 'bar'],
    ['baz', 42]
  ]);
  Object.fromEntries(entries)
  // { foo: "bar", baz: 42 }
  // 例二
  const map = new Map().set('foo', true).set('bar', false);
  Object.fromEntries(map)
  // { foo: true, bar: false }
  ```
  
  该方法的一个用处是配合`URLSearchParams`对象，**将查询`字符串`转为`对象`**。好用！
  
  ```js
  Object.fromEntries(new URLSearchParams('foo=bar&baz=qux'))
  // { foo: "bar", baz: "qux" }
  ```






