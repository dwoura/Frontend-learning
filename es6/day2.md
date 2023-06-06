# ES6 字符串的扩展

+ 加强了对 `Unicode` 的支持,允许采用 \uxxxx 形式表示一个字符
  
  1. `"\u0061"`
  2. `// "a"`
     
     
     
     `'\u{1F680}' === '\uD83D\uDE80'`   ` 大括号`表示法与四字节的 `UTF-16` 编码是等价的。



+  **字符串的遍历器接口**
1. `for (let codePoint of 'foo') {`
2. `console.log(codePoint)`
3. `}`
4. `// "f"`
5. `// "o"`
6. `// "o"`
   
   除了遍历字符串，这个遍历器最大的优点是可以识别大于 0xFFFF 的码点，传统的 for 循环无法识别这样的码点。( 码点(即字符编码) )
+ **直接输入 U+2028 和 U+2029**  [ES6 字符串的扩展_w3cschool](https://www.w3cschool.cn/escript6/escript6-jmas37ev.html)

+  **JSON.stringify() 的改造**
  
  根据标准，`JSON` 数据必须是`UTF-8` 编码。但是，现在的 JSON.stringify() 方法有可能返回不符合 UTF-8 标准的字符串。
  
  为了确保返回的是合法的 UTF-8 字符，[ES2019](https://github.com/tc39/proposal-well-formed-stringify) 改变了 JSON.stringify() 的行为。如果遇到 0xD800 到 0xDFFF 之间的单个码点，或者不存在的配对形式，它会返回转义字符串，留给应用自己决定下一步的处理。
  
  1. `JSON.stringify('\u{D834}') // ""**\\uD834**""`
  2. `JSON.stringify('\uDF06\uD834') // ""\\udf06\\ud834""`

+ **模板字符串**   
  
  传统字符串拼接的形式过于繁琐，ES6 引入了模板字符串：
  
  1. `` $('#result').append(` ``
  2. `There are <b>${basket.count}</b> items`
  3. `in your basket, <em>${basket.onSale}</em>`
  4. `are on sale!`
  5. `` `); ``
     
     
     
     **模板字符串**（template string）是增强版的字符串，**用反引号 ' ' 标识**。它可以当作普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。
     
     `// 字符串中嵌入变量`
  6. `var name = "Bob", time = "today";`
  7. `` `Hello ${name}, how are you ${time}?` ``   
     
     模板字符串的空格和换行，都是被保留的，如果你不想要这个换行，**可以使用 `trim` 方法消除换行**。
     
     **${ }用法:** 大括号内部可以放入表达式，引用对象属性，还可以调用函数。

+ **模板编译** 
  
  使用 <%...%> 放置 JavaScript 代码，使用 <%= ... %> 输出 JavaScript 表达式。**这个以后再看**



# ES6 字符串的新增方法

+ **String.fromCodePoint()**
  
  `ES6` 提供了 `String.fromCodePoint()`方法，可以识别大于 0xFFFF 的字符，弥补了 String.fromCharCode() 方法的不足。
  
  注意， fromCodePoint 方法定义在 **String 对象**上，而 codePointAt 方法定义在**字符串的实例对象**上。

+ **String.raw()**
  
  返回一个斜杠都被转义（即**斜杠前面再加一个斜杠**）的字符串，往往用于模板字符串的处理方法。
1. ``String.raw`Hi\u000A!`;``
2. `// 实际返回 "Hi\\u000A!"，显示的是转义后的结果 "Hi\u000A!"`
   
   
   
   `` String.raw`Hi\\n` ``
3. `// 返回 "Hi\\\\n"`
+ **实例方法：codePointAt()**
  
  JavaScript 内部，字符以`UTF-16` 的格式储存，每个字符固定为 2 个字节。对于那些需要 4 个字节储存的字符（Unicode 码点大于 0xFFFF 的字符），JavaScript 会认为它们是两个字符。
  
  `ES6` 提供了 `codePointAt()` 方法，能够正确处理 4 个字节储存的字符，**返回一个字符的码点**。
  
  codePointAt() 方法返回的是码点的十进制值，如果想要十六进制的值，可以使用 toString() 方法转换一下。
  
  1. `let s = '吉a';`
  
  2. `s.codePointAt(0).toString(16) // "20bb7"`
  
  3. `s.codePointAt(2).toString(16) // "61"`
     
     **codePointAt() 方法是测试一个字符由两个字节还是由四个字节组成的最简单方法。**
     
     `return 参数.codePointAt(0) > 0xFFFF;`

+ **normalize()** 
  
  ES6 提供字符串实例的 `normalize()`方法，用来将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。
  
  O一个字符，Ǒ实际上是两个字符。JavaScript 将**合成字符视为两个字符**。目前不能识别三个或三个以上字符的合成。
  
  

+ **实例方法：includes(), startsWith(), endsWith()** 
  
  传统上，JavaScript 只有 indexOf 方法，可以用来确定一个字符串是否包含在另一个字符串中。`ES6` 又提供了`三种`新方法。
  
  **includes()**：返回布尔值，表示是否找到了参数字符串。
  
  **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
  
  **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。
  
  三个方法都支持**第二个参数，表示开始搜索的位置**。
  
  1. `let s = 'Hello world!';`
  
  2. `s.startsWith('world', 6) // true`
  
  3. `s.endsWith('Hello', 5) // true`
  
  4. `s.includes('Hello', 6) // false`
  
  可以看出使用第二个参数 n 时， **endsWith 有所不同**。**它针对前 n 个字符**，而其他两个方法针对从第 n 个位置直到字符串结束。

+ **实例方法：repeat()**
  
  `repeat` 方法返回一个`新字符串`，参数表示将原字符串重复 n 次。
  
  1. `'x'.repeat(3) // "xxx"`
     
     `'na'.repeat(2.9) // "nana"`参数如果是小数，会被取整。

+ **实例方法：padStart()，padEnd()**
  
  `ES2017` 引入了字符串**补全长度**的功能。`padStart()` 用于头部补全， `padEnd()` 用于尾部补全。
  
  1. `'x'.padStart(4, 'ab') // 'abax'`
  
  2. `'x'.padEnd(5, 'ab') // 'xabab'`
     
     padStart() 和 padEnd()两个参数，第一个参数是**字符串补全生效的最大长度**，第二个参数是**用来补全的字符串**。
     
     若省略第二个参数，默认使用空格补全长度。
     
     **常见用途：**
     
     padStart() 的常见用途是为数值补全指定位数。下面代码生成 10 位的数值字符串
     
     `'123456'.padStart(10, '0') // "0000123456"`
     
     提示字符串格式
     
     `'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"`
     
     `'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"`

+ **实例方法：trimStart()，trimEnd()**
  
  [ES2019](https://github.com/tc39/proposal-string-left-right-trim) 对字符串实例新增了 `trimStart()` 和 `trimEnd()` 这两个方法。
  
  行为与`trim()` 一致， `trimStart()`消除字符串头部的空格，`trimEnd()` 消除尾部的空格。它们**返回的都是新字符串**。
  
  1. `const s = ' abc ';`
  
  2. `s.trim() // "abc"`
  
  3. `s.trimStart() // "abc "`
  
  4. `s.trimEnd() // " abc"`
     
     trimLeft() 是 trimStart() 的别名， trimRight() 是 trimEnd() 的别名。

+ **实例方法：matchAll()** 
  
  返回一个`正则表达式`在当前字符串的`所有匹配`，以后再看。
