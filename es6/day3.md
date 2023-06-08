# ES6 正则的扩展

往后再看



# ES6 数值的扩展

+ **二进制和八进制表示法**
  
  ES6 提供了`二进制`和`八进制`数值的新的写法。
  
  分别用前缀 0b （或 0B ）和 0o （或 0O ）表示。
  
  1. `0b111110111 === 503 // true` 二进制
  2. `0o767 === 503 // true` 八进制

+ **Number.isFinite(), Number.isNaN()**

+ **Number.parseInt(), Number.parseFloat()**
  
  ES6 将全局方法 `parseInt()` 和 `parseFloat()`，移植到 Number 对象上面，行为完全保持不变。
  
  **使用时要用到Number对象。** 目的是逐步减少全局性方法，使得语言逐步`模块化`。

+ **Number.isInteger()**
  
  js中整数和浮点数采用的是**同样的储存方法**，所以 25 和 25.0 被视为同一个值。
  
  由于 JavaScript 采用 IEEE 754 标准，数值存储为**64位双精度格式**，数值精度最多可以达到 **53 个二进制位**（1 个隐藏位与 52 个有效位）。如果数值的精度超过这个限度，第54位及后面的位就会被丢弃。
  
  [IEEE 754标准学习](https://zhuanlan.zhihu.com/p/353013671)

+ **Number.EPSILON**
  
  它表示 1 与大于 1 的`最小浮点数`之间的差。
  
  Number.EPSILON 实际上是js能够表示的`最小精度`。
  
  引入一个这么小的量的目的，在于为`浮点数`计算，设置一个**误差范围**。
  
  1. `0.1 + 0.2`
  
  2. `// 0.30000000000000004`
  
  3. `0.1 + 0.2 - 0.3`
  
  4. `// 5.551115123125783e-17`
  
  5. `5.551115123125783e-17.toFixed(20)`
  
  6. `// '0.00000000000000005551'`
     
     上面代码解释了，为什么比较 0.1 + 0.2 与 0.3 得到的结果是 false 。
     
     设置了Number.EPSILON，比如，误差范围设为 2 的-50 次方（即 Number.EPSILON * Math.pow(2, 2) ），即如果两个浮点数的差小于这个值，我们就认为这两个浮点数相等。
     
     `5.551115123125783e-17 < Number.EPSILON * Math.pow(2, 2)`
     
     `// true`
     
     **Math.pow(底数x,指数y)**

+ **安全整数和 Number.isSafeInteger()**
  
  JavaScript 能够准确表示的整数范围在  **-2^53 到 2^53 之间**。
  
  ES6 引入了`Number.MAX_SAFE_INTEGER`和 `Number.MIN_SAFE_INTEGER`这**两个常量**，用来**表示这个范围的上下限**。
  
  1. `Number.MIN_SAFE_INTEGER === -Number.MAX_SAFE_INTEGER`
  2. `// true`
  3. `Number.MIN_SAFE_INTEGER === -9007199254740991`
  4. `// true`
     
     以上就是两个安全整数常量。
     
     Number.isSafeInteger() 则是用来判断一个整数是否落在这个范围之内。
     
     注意：验证运算结果是否落在安全整数的范围内，不要只验证运算结果，而要**同时验证参与运算的每个值**。

+ **Math 对象的扩展**
  
  ES6 在`Math` 对象上新增了 `17`个与数学相关的方法。所有这些方法都是`静态方法`。
  
  + `Math.trunc()` 方法用于去除一个数的小数部分，**返回整数部分**。对非数值，会先转为数值。无法截取整数的值，返回 NaN 。
    
    `Math.trunc('123.456') // 123`
    
    `Math.trunc('foo'); // NaN`
  
  + `Math.sign()`用来判断一个数到底是正数、负数、还是零。
    
    参数为正数，返回 +1 ；参数为负数，返回 -1 ；...
  
  + `Math.cbrt()` 方法用于计算一个数的`立方根`。
  
  + `Math.clz32()` 方法将参数转为 32 位无符号整数的形式，返回这个 32 位值里面有多少个前导 0。
    
    左移运算符（ << ）与 Math.clz32 方法直接相关。
    
    1. `Math.clz32(1 << 1) // 30`
    2. `Math.clz32(1 << 2) // 29`
  
  + `Math.imul`方法返回两个数以 **32 位带符号整数形式相乘的结果**，返回的也是一个 **32 位的带符号整数**。
    
    1. `Math.imul(2, 4) // 8`
    2. `Math.imul(-1, 8) // -8`
    3. `Math.imul(-2, -2) // 4`
  
  + `Math.fround` 方法返回一个数的32位单精度浮点数形式。
    
    对于32位单精度格式来说，数值精度是**24个二进制位**（1 位隐藏位与 23 位有效位）
    
    Math.fround 方法的主要作用，是将64位双精度浮点数转为32位单精度浮点数。
    
    **后续再研究一下**
  
  + `Math.hypot`方法返回**所有参数**的**平方和的平方根**。求斜边好用
    
    1. `Math.hypot(3, 4); // 5`
    2. `Math.hypot(3, 4, 5); // 7.0710678118654755`

+ **对数方法**
  
  `ES6` 新增了 4 个`对数`相关方法。
  
  + **Math.expm1()**
    
    Math.expm1(x) 返回 ex - 1，即 Math.exp(x) - 1 。Math.exp(x)是 **计算自然对数基数e的指数** ，x是幂，代表了x次方。
    
    Math.expm1(x)比Math.exp(x) - 1**更加精确**。
  
  + **Math.log1p()**
    
    Math.log1p(x) 方法返回 1 + x 的自然对数，即 Math.log(1 + x) 。如果 x 小于-1，返回 NaN 。
  
  + **Math.log10()** 
    
    Math.log10(x) 返回以 10 为底的 x 的对数。如果 x 小于 0，则返回 NaN。
  
  + **Math.log2()**
    
    Math.log2(x) 返回以 2 为底的 x 的对数。如果 x 小于 0，则返回 NaN。

+ **双曲函数方法**
  
  `ES6`新增了 6 个`双曲函数方法`。
  
  - Math.sinh(x) 返回 x 的双曲正弦（hyperbolic sine）
  - Math.cosh(x) 返回 x 的双曲余弦（hyperbolic cosine）
  - Math.tanh(x) 返回 x 的双曲正切（hyperbolic tangent）
  - Math.asinh(x) 返回 x 的反双曲正弦（inverse hyperbolic sine）
  - Math.acosh(x) 返回 x 的反双曲余弦（inverse hyperbolic cosine）
  - Math.atanh(x) 返回 x 的反双曲正切（inverse hyperbolic tangent）
    
    可能画图用得上？

+ **指数运算符**
  
  ES2016 新增了一个`指数运算符`（ ** ）。
  
  `2 ** 3 // 8`
  
  `// 相当于 2 ** (3 ** 2)`
  
  `2 ** 3 ** 2`
  
  `// 512`
  
  特点是**右结合**，而不是常见的左结合。多个指数运算符连用时，是从最右边开始计算的。
  
  **= 也可以用

+ **BigInt 数据类型** 
  
  `Math.pow(2, 53) === Math.pow(2, 53) + 1 // true`
  
  超过 53 个二进制位的数值，无法保持精度。
  
  `Math.pow(2, 1024) // Infinity`
  
  超过 2 的 1024 次方的数值，无法表示。
  
  [ES2020](https://github.com/tc39/proposal-bigint) 引入了 `BigInt（大整数）`来解决。
  
  BigInt **只用来表示整数**，没有位数的限制，任何位数的整数都可以精确表示。
  
  1. `const a = 2172141653n;`
  
  2. `const b = 15346349309n;`
  
  3. `// BigInt 可以保持精度`
  
  4. `a * b // 33334444555566667777n`
  
  5. `// 普通整数无法保持精度`
  
  6. `Number(a) * Number(b) // 33334444555566670000`
  
  **注意**：为了与 Number 类型区别，BigInt 类型的数据 **必须添加后缀 n** 。
  
  然后**各进制**也可以用BigInt表示，也必须添加后缀n。
  
  运算：
  
  1. `// BigInt 的运算`
  2. `1n + 2n // 3n`
  
  BigInt 可以使用负号（ - ），但是**不能使用正号（ + ）**，因为会与 asm.js 冲突。

+ **BigInt 对象**
  
  1. `BigInt(123) // 123n`
  2. `BigInt('123') // 123n`
  
  js原生提供 `BigInt 对象`，，可用作`构造函数`生成 BigInt 类型的数值。
  
  还提供了三个静态方法。
  
  - BigInt.asUintN(width, BigInt) ： 给定的 BigInt 转为 0 到 2width - 1 之间对应的值。
  - BigInt.asIntN(width, BigInt) ：给定的 BigInt 转为 -2width - 1 到 2width - 1 - 1 之间对应的值。
  - BigInt.parseInt(string[, radix]) ：近似于 Number.parseInt() ，将一个字符串转换成指定进制的 BigInt。
    
    
    
    几乎所有的数值运算符都可以用在 BigInt，但是**有两个例外**。
    - 不带符号的右移位运算符 >>>
    - 一元的求正运算符 +
    
    **往后再研究后面几个方法**


