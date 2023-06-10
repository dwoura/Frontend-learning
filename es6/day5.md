# ES6 数组的扩展

+ **扩展运算符** 
  `扩展运算符`（spread）是**三个点**（ ... ）。
  
  好比 rest 参数的逆运算，将一个**数组转为用逗号分隔的参数序列**。
  
  `console.log(1, ...[2, 3, 4], 5)`
  
  `// 1 2 3 4 5`
  
  直接把数组拆开来了。
  
  该运算符主**要用于`函数调用`** 。
  
      可以和正常函数参数**结合使用**
  
  ```js
  function f(v, w, x, y, z) { }
  const args = [0, 1];
  f(-1, ...args, 2, ...[3]);
  ```
  
  注意，只有函数调用时，扩展运算符才可以放在圆括号中，否则会报错。
  
  取代apply方法的好例子：
  
  ```js
  //应用 Math.max 方法，简化求出一个数组最大元素的写法
  // ES5 的写法
  Math.max.apply(null, [14, 3, 77])
  // ES6 的写法
  Math.max(...[14, 3, 77])
  // 等同于
  Math.max(14, 3, 77);
  //js不提供数组求最大元素，所以只好...[]把数组转化为参数。
  ```
  
  ```js
  //通过 push 函数，将一个数组添加到另一个数组的尾部。
  // ES5的 写法
  var arr1 = [0, 1, 2];
  var arr2 = [3, 4, 5];
  Array.prototype.push.apply(arr1, arr2); 
  
  // ES6 的写法
  let arr1 = [0, 1, 2];
  let arr2 = [3, 4, 5];
  arr1.push(...arr2);//转化为参数push进去 
  
  //还有就是数组转日期
  // ES6
  new Date(...[2015, 1, 1]);
  ```

+ **扩展运算符的应用** 
  
  + **复制数组** 
    
    数组直接复制的话，**只是复制了指向底层数据结构的指针**，可以理解成引用类型。
    
    把数组a赋值给b，然后修改b的内容，会导致a的内容被修改。
    
    扩展运算符派上用场了：
    
    ```js
    //复制数组好方法
    const a1 = [1, 2];
    // 写法一
    const a2 = [...a1];
    // 写法二
    const [...a2] = a1;
    ```
  
  + **合并数组** 
    
    ```js
    const arr1 = ['a', 'b'];
    const arr2 = ['c'];
    const arr3 = ['d', 'e'];
    // ES5 的合并数组 传统写法！！
    arr1.concat(arr2, arr3);
    // [ 'a', 'b', 'c', 'd', 'e' ] 
    
    // ES6 的合并数组 现代写法！！
    [...arr1, ...arr2, ...arr3]
    // [ 'a', 'b', 'c', 'd', 'e' ]
    ```
    
    不过上面都是浅拷贝，修改了原来的数组也会影响拷贝的数组。
  
  + **与解构赋值结合** 
    
    ```js
    const [first, ...rest] = [1, 2, 3, 4, 5];
    first // 1
    rest  // [2, 3, 4, 5]
    ```
  
  + **字符串** 
    
    `扩展运算符`还可以**将`字符串`转为真正的`数组`。**
    
    ```js
    [...'hello']
    // [ "h", "e", "l", "l", "o" ]
    ```
    
    上面的写法，有一个重要的好处，那就是**能够正确识别四个字节的 Unicode 字符**。
    
    正确返回字符串长度的函数，可以像下面这样写：
    
    ```js
    function length(str) {
      return [...str].length;
    }
    length('x\uD83D\uDE80y') // 3
    ```
  
  + **实现了 Iterator 接口的对象**
    
    任何定义了Iterator接口的对象，都可以用扩展运算符**转为真正的数组**。
    
    ```js
    let nodeList = document.querySelectorAll('div');
    let array = [...nodeList];
    ```
    
     querySelectorAll 方法返回的是一个 NodeList 对象。**它不是数组**，而是一个**类似数组的对象**。
    
    扩展运算符可以将其转为真正的数组，原因就在于 **NodeList 对象实现了 Iterator 。**
    
    也就是说扩展运算符对数组和遍历器都有效。
  
  + **Map 和 Set 结构，Generator 函数** 
    
    好用！
    
    ```js
    let map = new Map([
      [1, 'one'],
      [2, 'two'],
      [3, 'three'],
    ]);
    let arr = [...map.keys()]; // [1, 2, 3]
    ```
    
    ```js
    //Generator 函数运行后，返回一个遍历器对象，因此也可以使用扩展运算符。
    const go = function*(){
      yield 1;
      yield 2;
      yield 3;
    };
    [...go()] // [1, 2, 3]
    ```

+ **Array.from()** 
  
  Array.from 方法用于**将两类对象转为真正的数组**：类似数组的对象（array-like object）和可遍历（iterable）的对象（包括 ES6 新增的数据结构 Set 和 Map）。
  
  下面是一个类似数组的对象， Array.from 将它转为真正的数组。
  
  ```js
  let arrayLike = {
      '0': 'a',
      '1': 'b',
      '2': 'c',
      length: 3
  };
  // ES5的写法
  var arr1 = [].slice.call(arrayLike); // ['a', 'b', 'c']
  // ES6的写法
  let arr2 = Array.from(arrayLike); // ['a', 'b', 'c']
  ```
  
  常见的类似数组的对象是 DOM 操作返回的 NodeList 集合，以及函数内部的 arguments 对象。用Array.from把他们转化成真正数组再好不过了。
  
  ```js
  // NodeList对象
  //筛选出符合内容条件的p元素，并转化成真正数组
  let ps = document.querySelectorAll('p');
  Array.from(ps).filter(p => {
    return p.textContent.length > 100;
  }); 
  
  // arguments对象
  function foo() {
    var args = Array.from(arguments);
    // ...
  }
  ```
  
  Array.from **还可以接受第二个参数**，作用**类似于数组的 map 方法**，用来对每个元素进行处理，将处理后的值放入返回的数组。
  
  ```js
  Array.from(arrayLike, x => x * x);
  // 等同于
  Array.from(arrayLike).map(x => x * x);
  Array.from([1, 2, 3], (x) => x * x)
  // [1, 4, 9]
  ```
  
  只要有一个原始的数据结构，你就可以先对它的值进行处理，然后转成规范的数组结构。
  
  ```js
  //先定义好对象属性，转成数组也会用到该属性。
  Array.from({ length: 2 }, () => 'jack')
  // ['jack', 'jack']
  ```

+ **Array.of()** 
  
  `Array.of`方法用于**将一组值，转换为数组**。
  
  目的，是弥补`数组构造函数` Array() 的不足。
  
  ```js
  Array.of(3, 11, 8) // [3,11,8]
  Array.of(3) // [3]
  Array.of(3).length // 1
  ```

+ **copyWithin()** 
  
  数组实例的`copyWithin()` 方法，在当前数组内部，将**指定位置的成员复制到其他位置（会覆盖原有成员）**，然后返回当前数组。
  
  三个参数：
  
  - target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
  - start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
  - end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。
  
  ```js
  //从 3 号位直到数组结束的成员（4 和 5），
  //复制到从 0 号位开始的位置，结果覆盖了原来的 1 和 2。
  [1, 2, 3, 4, 5].copyWithin(0, 3)
  // [4, 5, 3, 4, 5]
  ```

+ **数组实例的 find() 和 findIndex()** 
  
  数组实例的`find`方法，用于找出**第一个**符合条件的数组成员。
  
  它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为 true 的成员，然后返回该成员。如果没有符合条件的成员，则返回 undefined 。
  
  ```js
  //找出数组中第一个小于 0 的成员。
  [1, 4, -5, 10].find((n) => n < 0)
  // -5
  ```
  
  find 方法的回调函数可以接受三个参数，依次为**当前的值**、**当前的位置**和**原数组**:
  
  ```js
  [1, 5, 10, 15].find(function(value, index, arr) {
    return value > 9;
  }) // 10
  ```
  
  这两个方法都可以接受**第二个参数**，用来**绑定回调函数的 this 对象**。记不太住这个
  
  ```js
  function f(v){
    return v > this.age;
  }
  let person = {name: 'John', age: 20};
  [10, 12, 26, 15].find(f, person);    // 26
  ```
  
  上面的this指的就是第二个参数的对象。

+ **数组实例的 fill()**
  
  只有第一个参数的话，就是把数组所有元素填充为那个值。
  
  **fill 方法用于空数组的初始化非常方便。**
  
  fill 方法还可以接受**第二个和第三个参数**，用于指定**填充的起始位置**和**结束位置**。
  
  ```js
  ['a', 'b', 'c'].fill(7, 1, 2)
  // ['a', 7, 'c']
  ```
  
  <u>这些参数的顺序，似乎总是以value起始？不是（值，下标，数组）就是（值，始，末）？多看一些回头再总结一下。规律似乎就是从小到大</u>

+ **数组实例的 entries()，keys() 和 values()**
  
  他们可以遍历数组，返回一个遍历器对象。
  
  可以用for...of循环进行遍历。
  
  keys() 是对键名的遍历、values() 是对键值的遍历、**entries() 是对键值对的遍历**。
  
  ```js
  for (let index of ['a', 'b'].keys()) {
    console.log(index);
  }
  // 0
  // 1
  for (let elem of ['a', 'b'].values()) {
    console.log(elem);
  }
  // 'a'
  // 'b'
  for (let [index, elem] of ['a', 'b'].entries()) {
    console.log(index, elem);
  }
  // 0 "a"
  // 1 "b"
  ```
  
  可以看出，数组中的键值对是下标对应值。

+ **数组实例的 includes()**
  
  返回一个布尔值，表示某个数组是否包含给定的值，与字符串的 includes 方法类似。
  
  ```js
  [1, 2, 3].includes(2)     // true
  [1, 2, 3].includes(4)     // false
  [1, 2, NaN].includes(NaN) // true 
  
  [1, 2, 3].includes(3, 3);  // false
  [1, 2, 3].includes(3, -1); // true
  ```
  
  二个参数表示搜索的起始位置，默认为 0 。
  
  没有该方法之前，通常用数组的 `indexOf`方法，检查是否包含某个值。这看起来不够直观，还有可能导致对NaN的误判。
  
  另外，`Map` 和`Set`数据结构**有一个 `has`方法**，需要**注意与 includes 区分**。
  
  - **Map 结构**的 has 方法，是用来查找**键名**的，比如 Map.prototype.has(key) 、 WeakMap.prototype.has(key) 、 Reflect.has(target, propertyKey) 。
  - **Set 结构**的 has 方法，是用来查找**值**的，比如 Set.prototype.has(value) 、 WeakSet.prototype.has(value) 。

+ **数组实例的 flat()，flatMap()**
  
  数组的成员有时还是数组,flat()用于将嵌套的数组 **“拉平”**，变成一维的数组。该方法返回一个新数组，不影响原来的。
  
  ```js
  [1, 2, [3, 4]].flat()
  // [1, 2, 3, 4]
  ```
  
  flat()里面的参数可以写拉平的层数，整数，默认1层。如果想全拉平，那就用**infinity**！
  
  如果原数组有空位， flat() 方法会跳过空位。
  
  `flatMap()`方法**对原数组的每个成员执行一个函数**：
  
  ```js
  // 相当于 [[2, 4], [3, 6], [4, 8]].flat()
  [2, 3, 4].flatMap((x) => [x, x * 2])
  // [2, 4, 3, 6, 4, 8]
  ```
  
  相当于把每个元素按照某种规则**扩展成数组**，然后再拉平。

+ **数组的空位**
  
  Array 构造函数返回的数组都是空位，没有任何值。
  
  `Array(3) // [, , ,]`
  
  注意， **空位不是 undefined**  ，一个位置的值等于 undefined ，依然是有值的。
  
  ```js
  //判断0号位有没有值
  0 in [undefined, undefined, undefined] // true
  0 in [, , ,] // false
  ```
  
  **ES6 明确将空位转为 undefined 。** 有一些还是会跳过，空位处理规则不太统一，建议避免出现空位。

+ **Array.prototype.sort() 的排序稳定性**
  
  稳定排序：排序后，同类结果仍按原始顺序一致。
  
  例如：
  
  ```js
  const arr = [
    'peach',
    'straw',
    'apple',
    'spork'
  ];
  const stableSorting = (s1, s2) => {
    if (s1[0] < s2[0]) return -1;
    return 1;
  };
  arr.sort(stableSorting)
  // ["apple", "peach", "straw", "spork"]
  ```
  
  上面代码按照首字母进行排序，排序结果中， **straw 在 spork 的前面，跟原始顺序一致** ，所以排序算法 stableSorting 是稳定排序。
  
  即 次排序时以原始顺序优先，而非规则优先。
  
  常见的排序算法之中，`插入排序`、`合并排序`、`冒泡排序`等都是稳定的，`堆排序`、`快速排序`等是不稳定的。
  
  不稳定排序的主要缺点是，**多重排序时可能会产生问题**。
  
  [ES2019](https://github.com/tc39/ecma262/pull/1340) 明确规定， Array.prototype.sort() 的**默认排序算法必须稳定**。




