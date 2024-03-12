# ES6

[TOC]

 [菜鸟 ES6 教程](https://www.runoob.com/w3cnote/es6-tutorial.html)

[ES6 入门教程](https://es6.ruanyifeng.com/)

# 1简介

ES6， 全称 **ECMAScript 6.0** ，是 JavaScript 的下一个版本标准，2015.06 发版。

ES6 主要是为了解决 ES5 的先天不足，比如 JavaScript 里并没有类的概念

**ECMAScript 和 JavaScript 的关系**是，**前者是后者的规格，后者是前者的一种实现**（另外的 ECMAScript 方言还有 JScript 和 ActionScript）

# 2.let 和 const 命令

## let 命令

比var更好用，不在容易写出难以预料的错误代码。

let 允许创建块级作用域，**ES6 推荐在函数中使用 let 定义变量，**而非 var。

- 只在`let`命令所在的代码块内有效
- 不存在变量提升
- 暂时性死区
- 不允许重复声明

ES6 新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量，**只在`let`命令所在的代码块内有效。**

### 不存在变量提升  

`var`命令会发生**“变量提升”现象**，**即变量可以在声明之前使用**，值为`undefined`。

为了纠正这种现象，`let`命令改变了语法行为，它所声明的变量一定要在声明后使用，否则报错。

```js
// var 的情况
console.log(foo); // 输出undefined
var foo = 2;

// let 的情况
console.log(bar); // 报错ReferenceError
let bar = 2;
```

### [暂时性死区](https://es6.ruanyifeng.com/#docs/let#%E6%9A%82%E6%97%B6%E6%80%A7%E6%AD%BB%E5%8C%BA)

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

### 不允许重复声明

`let`不允许在相同作用域内，重复声明同一个变量。

## ES6 的块级作用域

`let`实际上为 JavaScript 新增了块级作用域。

块级作用域的出现，实际上使得获得**广泛应用的匿名立即执行函数表达式**（匿名 IIFE）不再必要了。

```js
// IIFE 写法
(function () {
  var tmp = ...;
  ...
}());

// 块级作用域写法
{
  let tmp = ...;
  ...
}
```

## const 命令

`const`声明一个**只读的常量**。一旦声明，常量的值就不能改变。

`const`声明的变量不得改变值，这意味着，**`const`一旦声明变量，就必须立即初始化**，不能留到以后赋值(java中是不必要的)。

```js
const foo;
// SyntaxError: Missing initializer in const declaration
```

`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。`const`命令声明的常量也是不提升，同样存在暂时性死区，只能在声明的位置后面使用。`const`声明的常量，也与`let`一样不可重复声明。

### 本质

`const`实际上保证的，并不是变量的值不得改动，而是**变量指向的那个内存地址所保存的数据不得改动。**对于**简单类型的数据**（数值、字符串、布尔值），**值就保存在变量指向的那个内存地址**，**因此等同于常量**。但对于**复合类型的数据（主要是对象和数组）**，变量指向的内存地址，保存的只是一个指向实际数据的指针，**`const`只能保证这个指针是固定的（即总是指向另一个固定的地址）**，至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

```javascript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

上面代码中，常量`foo`储存的是一个地址，这个地址指向一个对象。不可变的只是这个地址，即不能把`foo`指向另一个地址，但对象本身是可变的，所以依然可以为其添加新属性。

如果真的想将对象冻结，**应该使用`Object.freeze`方法。**

```javascript
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

上面代码中，常量`foo`指向一个冻结的对象，所以添加新属性不起作用，严格模式时还会报错。

**除了将对象本身冻结，对象的属性也应该冻结。**下面是一个将对象彻底冻结的函数。

```javascript
var constantize = (obj) => {
  Object.freeze(obj);
  Object.keys(obj).forEach( (key, i) => {
    if ( typeof obj[key] === 'object' ) {
      constantize( obj[key] );
    }
  });
};
```

## ES6 声明变量的六种方法

ES5 只有两种声明变量的方法：`var`命令和`function`命令。ES6 除了添加`let`和`const`命令，后面章节还会提到，另外两种声明变量的方法：`import`命令和`class`命令。所以，ES6 一共有 6 种声明变量的方法。

## 顶层对象的属性

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的。

```javascript
window.a = 1;
a // 1

a = 2;
window.a // 2
```

上面代码中，顶层对象的属性赋值与全局变量的赋值，是同一件事。

顶层对象的属性与全局变量挂钩，被认为是 JavaScript 语言最大的设计败笔之一。这样的设计带来了几个很大的问题，首先是没法在编译时就报出变量未声明的错误，只有运行时才能知道（因为全局变量可能是顶层对象的属性创造的，而属性的创造是动态的）；其次，程序员很容易不知不觉地就创建了全局变量（比如打字出错）；最后，顶层对象的属性是到处可以读写的，这非常不利于模块化编程。另一方面，`window`对象有实体含义，指的是浏览器的窗口对象，顶层对象是一个有实体含义的对象，也是不合适的。

ES6 为了改变这一点，一方面规定，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性。也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。

```javascript
var a = 1;
// 如果在 Node 的 REPL 环境，可以写成 global.a
// 或者采用通用方法，写成 this.a
window.a // 1

let b = 1;
window.b // undefined
```

上面代码中，全局变量`a`由`var`命令声明，所以它是顶层对象的属性；全局变量`b`由`let`命令声明，所以它不是顶层对象的属性，返回`undefined`。

## globalThis 对象

JavaScript 语言存在一个顶层对象，它提供全局环境（即全局作用域），所有代码都是在这个环境中运行。但是，顶层对象在各种实现里面是不统一的。

- 浏览器里面，顶层对象是`window`，但 Node 和 Web Worker 没有`window`。
- 浏览器和 Web Worker 里面，`self`也指向顶层对象，但是 Node 没有`self`。
- Node 里面，顶层对象是`global`，但其他环境都不支持。

同一段代码为了能够在各种环境，都能取到顶层对象，现在一般是使用`this`关键字，但是有局限性。

- 全局环境中，`this`会返回顶层对象。但是，Node.js 模块中`this`返回的是当前模块，ES6 模块中`this`返回的是`undefined`。
- 函数里面的`this`，如果函数不是作为对象的方法运行，而是单纯作为函数运行，`this`会指向顶层对象。但是，严格模式下，这时`this`会返回`undefined`。
- 不管是严格模式，还是普通模式，`new Function('return this')()`，总是会返回全局对象。但是，如果浏览器用了 CSP（Content Security Policy，内容安全策略），那么`eval`、`new Function`这些方法都可能无法使用。

综上所述，很难找到一种方法，可以在所有情况下，都取到顶层对象。下面是两种勉强可以使用的方法。

```javascript
// 方法一
(typeof window !== 'undefined'
   ? window
   : (typeof process === 'object' &&
      typeof require === 'function' &&
      typeof global === 'object')
     ? global
     : this);

// 方法二
var getGlobal = function () {
  if (typeof self !== 'undefined') { return self; }
  if (typeof window !== 'undefined') { return window; }
  if (typeof global !== 'undefined') { return global; }
  throw new Error('unable to locate global object');
};
```

[ES2020](https://github.com/tc39/proposal-global) 在语言标准的层面，引入`globalThis`作为顶层对象。也就是说，任何环境下，`globalThis`都是存在的，都可以从它拿到顶层对象，指向全局环境下的`this`。

垫片库[`global-this`](https://github.com/ungap/global-this)模拟了这个提案，可以在所有环境拿到`globalThis`。

# 4字符串的扩展

## 字符的 Unicode 表示法

ES6 加强了对 Unicode 的支持，允许采用`\uxxxx`形式表示一个字符，其中`xxxx`表示字符的 Unicode 码点。

```javascript
"\u0061"
// "a"
```

但是，这种表示法只限于码点在`\u0000`~`\uFFFF`之间的字符。超出这个范围的字符，必须用两个双字节的形式表示。

```javascript
"\uD842\uDFB7"
// "𠮷"

"\u20BB7"
// " 7"
```

上面代码表示，如果直接在`\u`后面跟上超过`0xFFFF`的数值（比如`\u20BB7`），JavaScript 会理解成`\u20BB+7`。由于`\u20BB`是一个不可打印字符，所以只会显示一个空格，后面跟着一个`7`。

ES6 对这一点做出了改进，只要将码点放入大括号，就能正确解读该字符。

```javascript
"\u{20BB7}"
// "𠮷"

"\u{41}\u{42}\u{43}"
// "ABC"

let hello = 123;
hell\u{6F} // 123

'\u{1F680}' === '\uD83D\uDE80'
// true
```

上面代码中，最后一个例子表明，大括号表示法与四字节的 UTF-16 编码是等价的。

有了这种表示法之后，JavaScript 共有 6 种方法可以表示一个字符。

```javascript
'\z' === 'z'  // true
'\172' === 'z' // true
'\x7A' === 'z' // true
'\u007A' === 'z' // true
'\u{7A}' === 'z' // true
```

## 字符串的遍历器接口

ES6 为字符串添加了遍历器接口（详见《Iterator》一章），使得字符串可以被`for...of`循环遍历。

```javascript
for (let codePoint of 'foo') {
  console.log(codePoint)
}
// "f"
// "o"
// "o"
```

除了遍历字符串，这个**遍历器最大的优点是可以识别大于`0xFFFF`的码点，传统的`for`循环无法识别这样的码点。**

```javascript
let text = String.fromCodePoint(0x20BB7);

for (let i = 0; i < text.length; i++) {
  console.log(text[i]);
}
// " "
// " "

for (let i of text) {
  console.log(i);
}
// "𠮷"
```

上面代码中，**字符串`text`只有一个字符，但是`for`循环会认为它包含两个字符（都不可打印），而`for...of`循环会正确识别出这一个字符。**

## 5模板字符串

传统的 JavaScript 语言，输出模板通常是这样写的（下面使用了 jQuery 的方法）。

```javascript
$('#result').append(
  'There are <b>' + basket.count + '</b> ' +
  'items in your basket, ' +
  '<em>' + basket.onSale +
  '</em> are on sale!'
);
```

上面这种写法相当繁琐不方便，ES6 引入了模板字符串解决这个问题。

```javascript
$('#result').append(`
  There are <b>${basket.count}</b> items
   in your basket, <em>${basket.onSale}</em>
  are on sale!
`);
```

模板字符串（template string）是增强版的字符串，**用反引号（`）标识**。它可以当作**普通字符串使用，也可以用来定义多行字符串，或者在字符串中嵌入变量。**

```javascript
// 普通字符串
`In JavaScript '\n' is a line-feed.`

// 多行字符串
`In JavaScript this is
 not legal.`

console.log(`string text line 1
string text line 2`);

// 字符串中嵌入变量
let name = "Bob", time = "today";
`Hello ${name}, how are you ${time}?`
```

上面代码中的模板字符串，都是用反引号表示。**如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。**

```javascript
let greeting = `\`Yo\` World!`;
```

如果使用模板字符串表示多行字符串，**所有的空格和缩进都会被保留在输出之中。**

```javascript
$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`);
```

上面代码中，所有模板字符串的空格和换行，都是被保留的，比如`<ul>`标签前面会有一个换行。如果你不想要这个换行，**可以使用`trim`方法消除它。**

```javascript
$('#list').html(`
<ul>
  <li>first</li>
  <li>second</li>
</ul>
`.trim());
```

### 模板字符串中嵌入变量

模板字符串中嵌入变量，**需要将变量名写在`${}`之中。**

```javascript
function authorize(user, action) {
  if (!user.hasPrivilege(action)) {
    throw new Error(
      // 传统写法为
      // 'User '
      // + user.name
      // + ' is not authorized to do '
      // + action
      // + '.'
      `User ${user.name} is not authorized to do ${action}.`);
  }
}
```

大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。

```javascript
let x = 1;
let y = 2;

`${x} + ${y} = ${x + y}`
// "1 + 2 = 3"

`${x} + ${y * 2} = ${x + y * 2}`
// "1 + 4 = 5"

let obj = {x: 1, y: 2};
`${obj.x + obj.y}`
// "3"
```

**模板字符串之中还能调用函数。**

```javascript
function fn() {
  return "Hello World";
}

`foo ${fn()} bar`
// foo Hello World bar
```

如果大括号中的**值不是字符串，将按照一般的规则转为字符串**。比如，大括号中是一个对象，将默认调用对象的`toString`方法。

如果模板字符串中的变量没有声明，将报错。

```javascript
// 变量place没有声明
let msg = `Hello, ${place}`;
// 报错
```

**由于模板字符串的大括号内部，就是执行 JavaScript 代码**，因此如果大括号内部是一个字符串，将会原样输出。

```javascript
`Hello ${'World'}`
// "Hello World"
```

**模板字符串甚至还能嵌套。**

```javascript
const tmpl = addrs => `
  <table>
  ${addrs.map(addr => `
    <tr><td>${addr.first}</td></tr>
    <tr><td>${addr.last}</td></tr>
  `).join('')}
  </table>
`;
```

上面代码中，模板字符串的变量之中，又嵌入了另一个模板字符串，使用方法如下。

```javascript
const data = [
    { first: '<Jane>', last: 'Bond' },
    { first: 'Lars', last: '<Croft>' },
];

console.log(tmpl(data));
// <table>
//
//   <tr><td><Jane></td></tr>
//   <tr><td>Bond</td></tr>
//
//   <tr><td>Lars</td></tr>
//   <tr><td><Croft></td></tr>
//
// </table>
```

如果需要引用模板字符串本身，在需要时执行，可以写成函数。

```javascript
let func = (name) => `Hello ${name}!`;
func('Jack') // "Hello Jack!"
```

上面代码中，模板字符串写成了一个函数的返回值。执行这个函数，就相当于执行这个模板字符串了。

# 5字符串的新增方法

1. [String.fromCodePoint()](https://es6.ruanyifeng.com/#docs/string-methods#String.fromCodePoint())
2. [String.raw()](https://es6.ruanyifeng.com/#docs/string-methods#String.raw())
3. [实例方法：codePointAt()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：codePointAt())
4. [实例方法：normalize()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：normalize())
5. [实例方法：includes(), startsWith(), endsWith()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：includes(), startsWith(), endsWith())
6. [实例方法：repeat()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：repeat())
7. [实例方法：padStart()，padEnd()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：padStart()，padEnd())
8. [实例方法：trimStart()，trimEnd()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：trimStart()，trimEnd())
9. [实例方法：matchAll()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：matchAll())
10. [实例方法：replaceAll()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：replaceAll())
11. [实例方法：at()](https://es6.ruanyifeng.com/#docs/string-methods#实例方法：at())

## 3String.raw()

ES6 还为原生的 String 对象，提供了一个`raw()`方法。该方法返回一个斜杠都被转义（即斜杠前面再加一个斜杠）的字符串，往往用于模板字符串的处理方法。

```javascript
String.raw`Hi\n${2+3}!`
// 实际返回 "Hi\\n5!"，显示的是转义后的结果 "Hi\n5!"

String.raw`Hi\u000A!`;
// 实际返回 "Hi\\u000A!"，显示的是转义后的结果 "Hi\u000A!"
```

如果原字符串的斜杠已经转义，那么`String.raw()`会进行再次转义。

```javascript
String.raw`Hi\\n`
// 返回 "Hi\\\\n"

String.raw`Hi\\n` === "Hi\\\\n" // true
```

`String.raw()`方法可以作为处理模板字符串的基本方法，它会将所有变量替换，而且对斜杠进行转义，方便下一步作为字符串来使用。

`String.raw()`本质上是一个正常的函数，只是专用于模板字符串的标签函数。如果写成正常函数的形式，它的第一个参数，应该是一个具有`raw`属性的对象，且`raw`属性的值应该是一个数组，对应模板字符串解析后的值。

```javascript
// `foo${1 + 2}bar`
// 等同于
String.raw({ raw: ['foo', 'bar'] }, 1 + 2) // "foo3bar"
```

上面代码中，`String.raw()`方法的第一个参数是一个对象，它的`raw`属性等同于原始的模板字符串解析后得到的数组。

作为函数，`String.raw()`的代码实现基本如下。

```javascript
String.raw = function (strings, ...values) {
  let output = '';
  let index;
  for (index = 0; index < values.length; index++) {
    output += strings.raw[index] + values[index];
  }

  output += strings.raw[index]
  return output;
}
```

## 实例方法：includes(), startsWith(), endsWith()

传统上，JavaScript 只有`indexOf`方法，可以用来确定一个字符串是否包含在另一个字符串中。ES6 又提供了三种新方法。

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

```javascript
let s = 'Hello world!';

s.startsWith('Hello') // true
s.endsWith('!') // true
s.includes('o') // true
```

这三个方法都支持第二个参数，表示开始搜索的位置。

```javascript
let s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

上面代码表示，使用第二个参数`n`时，`endsWith`的行为与其他两个方法有所不同。它针对前`n`个字符，而其他两个方法针对从第`n`个位置直到字符串结束。

## 实例方法：repeat()

`repeat`方法返回一个新字符串，表示将原字符串重复`n`次。

```javascript
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```

参数如果是小数，会被取整。

```javascript
'na'.repeat(2.9) // "nana"
```

如果`repeat`的参数是负数或者`Infinity`，会报错。

```javascript
'na'.repeat(Infinity)
// RangeError
'na'.repeat(-1)
// RangeError
```

但是，如果参数是 0 到-1 之间的小数，则等同于 0，这是因为会先进行取整运算。0 到-1 之间的小数，取整以后等于`-0`，`repeat`视同为 0。

```javascript
'na'.repeat(-0.9) // ""
```

参数`NaN`等同于 0。

```javascript
'na'.repeat(NaN) // ""
```

如果`repeat`的参数是字符串，则会先转换成数字。

```javascript
'na'.repeat('na') // ""
'na'.repeat('3') // "nanana"
```

## 实例方法：padStart()，padEnd()

ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。

```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

上面代码中，`padStart()`和`padEnd()`一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。

如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。

```javascript
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```

如果用来补全的字符串与原字符串，两者的长度之和超过了最大长度，则会截去超出位数的补全字符串。

```javascript
'abc'.padStart(10, '0123456789')
// '0123456abc'
```

如果省略第二个参数，默认使用空格补全长度。

```javascript
'x'.padStart(4) // '   x'
'x'.padEnd(4) // 'x   '
```

`padStart()`的常见用途是为**数值补全指定位数**。下面代码生成 10 位的数值字符串。

```javascript
'1'.padStart(10, '0') // "0000000001"
'12'.padStart(10, '0') // "0000000012"
'123456'.padStart(10, '0') // "0000123456"
```

另一个用途是**提示字符串格式。**

```javascript
'12'.padStart(10, 'YYYY-MM-DD') // "YYYY-MM-12"
'09-12'.padStart(10, 'YYYY-MM-DD') // "YYYY-09-12"
```

# 6正则的扩展

1. [RegExp 构造函数](https://es6.ruanyifeng.com/#docs/regex#RegExp 构造函数)
2. [字符串的正则方法](https://es6.ruanyifeng.com/#docs/regex#字符串的正则方法)
3. [u 修饰符](https://es6.ruanyifeng.com/#docs/regex#u 修饰符)
4. [RegExp.prototype.unicode 属性](https://es6.ruanyifeng.com/#docs/regex#RegExp.prototype.unicode 属性)
5. [y 修饰符](https://es6.ruanyifeng.com/#docs/regex#y 修饰符)
6. [RegExp.prototype.sticky 属性](https://es6.ruanyifeng.com/#docs/regex#RegExp.prototype.sticky 属性)
7. [RegExp.prototype.flags 属性](https://es6.ruanyifeng.com/#docs/regex#RegExp.prototype.flags 属性)
8. [s 修饰符：dotAll 模式](https://es6.ruanyifeng.com/#docs/regex#s 修饰符：dotAll 模式)
9. [后行断言](https://es6.ruanyifeng.com/#docs/regex#后行断言)
10. [Unicode 属性类](https://es6.ruanyifeng.com/#docs/regex#Unicode 属性类)
11. [具名组匹配](https://es6.ruanyifeng.com/#docs/regex#具名组匹配)
12. [正则匹配索引](https://es6.ruanyifeng.com/#docs/regex#正则匹配索引)
13. [String.prototype.matchAll()](https://es6.ruanyifeng.com/#docs/regex#String.prototype.matchAll())

# 8函数的扩展

## 函数参数的默认值

ES6 允许**为函数的参数设置默认值**，即直接写在参数定义的后面。

```js
function log(x, y = 'World') {
  console.log(x, y);
}

log('Hello') // Hello World
log('Hello', 'China') // Hello China
log('Hello', '') // Hello[]()
```

有两个好处：首先，阅读代码的人，可以立刻意识到哪些参数是可以省略的，不用查看函数体或文档；其次，有利于将来的代码优化，即使未来的版本在对外接口中，彻底拿掉这个参数，也不会导致以前的代码无法运行。

**参数变量是默认声明的**，所以不能用`let`或`const`再次声明。

使用参数默认值时**，函数不能有同名参数。**

另外，一个容易忽略的地方是，**参数默认值不是传值的**，而是**每次都重新计算默认值表达式的值。**也就是说，参数默认值是**惰性求值**的。

```javascript
let x = 99;
function foo(p = x + 1) {
  console.log(p);
}

foo() // 100

x = 100;
foo() // 101
```

上面代码中，参数`p`的默认值是`x + 1`。这时，每次调用函数`foo`，都会重新计算`x + 1`，而不是默认`p`等于 100。

## 实例方法：trimStart()，trimEnd()

[ES2019](https://github.com/tc39/proposal-string-left-right-trim) 对字符串实例新增了`trimStart()`和`trimEnd()`这两个方法。它们的行为与`trim()`一致，`trimStart()`消除字符串头部的空格，`trimEnd()`消除尾部的空格。它们返回的都是新字符串，不会修改原始字符串。

```javascript
const s = '  abc  ';

s.trim() // "abc"
s.trimStart() // "abc  "
s.trimEnd() // "  abc"
```

上面代码中，`trimStart()`只消除头部的空格，保留尾部的空格。`trimEnd()`也是类似行为。

除了空格键，这两个方法对字符串头部（或尾部）的 tab 键、换行符等不可见的空白符号也有效。

浏览器还部署了额外的两个方法，`trimLeft()`是`trimStart()`的别名，`trimRight()`是`trimEnd()`的别名。

## 实例方法：matchAll()

`matchAll()`方法返回一个正则表达式在当前字符串的所有匹配，详见《正则的扩展》的一章。

## 实例方法：replaceAll()

历史上，字符串的实例方法`replace()`只能替换第一个匹配。

```javascript
'aabbcc'.replace('b', '_')
// 'aa_bcc'
```

上面例子中，`replace()`只将第一个`b`替换成了下划线。

如果要替换所有的匹配，不得不使用正则表达式的`g`修饰符。

```javascript
'aabbcc'.replace(/b/g, '_')
// 'aa__cc'
```

正则表达式毕竟不是那么方便和直观，[ES2021](https://github.com/tc39/proposal-string-replaceall) 引入了`replaceAll()`方法，可以一次性替换所有匹配。

```javascript
'aabbcc'.replaceAll('b', '_')
// 'aa__cc'
```

它的用法与`replace()`相同，返回一个新字符串，不会改变原字符串。

```javascript
String.prototype.replaceAll(searchValue, replacement)
```

上面代码中，`searchValue`是搜索模式，可以是一个字符串，也可以是一个全局的正则表达式（带有`g`修饰符）。

如果`searchValue`是一个不带有`g`修饰符的正则表达式，`replaceAll()`会报错。这一点跟`replace()`不同。

```javascript
// 不报错
'aabbcc'.replace(/b/, '_')

// 报错
'aabbcc'.replaceAll(/b/, '_')
```

上面例子中，`/b/`不带有`g`修饰符，会导致`replaceAll()`报错。

`replaceAll()`的第二个参数`replacement`是一个字符串，表示替换的文本，其中可以使用一些特殊字符串。

- `$&`：匹配的字符串。
- `$` `：匹配结果前面的文本。
- `$'`：匹配结果后面的文本。
- `$n`：匹配成功的第`n`组内容，`n`是从1开始的自然数。这个参数生效的前提是，第一个参数必须是正则表达式。
- `$$`：指代美元符号`$`。

下面是一些例子。

```javascript
// $& 表示匹配的字符串，即`b`本身
// 所以返回结果与原字符串一致
'abbc'.replaceAll('b', '$&')
// 'abbc'

// $` 表示匹配结果之前的字符串
// 对于第一个`b`，$` 指代`a`
// 对于第二个`b`，$` 指代`ab`
'abbc'.replaceAll('b', '$`')
// 'aaabc'

// $' 表示匹配结果之后的字符串
// 对于第一个`b`，$' 指代`bc`
// 对于第二个`b`，$' 指代`c`
'abbc'.replaceAll('b', `$'`)
// 'abccc'

// $1 表示正则表达式的第一个组匹配，指代`ab`
// $2 表示正则表达式的第二个组匹配，指代`bc`
'abbc'.replaceAll(/(ab)(bc)/g, '$2$1')
// 'bcab'

// $$ 指代 $
'abc'.replaceAll('b', '$$')
// 'a$c'
```

`replaceAll()`的第二个参数`replacement`除了为字符串，也可以是一个函数，该函数的返回值将替换掉第一个参数`searchValue`匹配的文本。

```javascript
'aabbcc'.replaceAll('b', () => '_')
// 'aa__cc'
```

上面例子中，`replaceAll()`的第二个参数是一个函数，该函数的返回值会替换掉所有`b`的匹配。

这个替换函数可以接受多个参数。第一个参数是捕捉到的匹配内容，第二个参数捕捉到是组匹配（有多少个组匹配，就有多少个对应的参数）。此外，最后还可以添加两个参数，倒数第二个参数是捕捉到的内容在整个字符串中的位置，最后一个参数是原字符串。

```javascript
const str = '123abc456';
const regex = /(\d+)([a-z]+)(\d+)/g;

function replacer(match, p1, p2, p3, offset, string) {
  return [p1, p2, p3].join(' - ');
}

str.replaceAll(regex, replacer)
// 123 - abc - 456
```

上面例子中，正则表达式有三个组匹配，所以`replacer()`函数的第一个参数`match`是捕捉到的匹配内容（即字符串`123abc456`），后面三个参数`p1`、`p2`、`p3`则依次为三个组匹配。

## 实例方法：at()

`at()`方法接受一个整数作为参数，返回参数指定位置的字符，支持负索引（即倒数的位置）。

```javascript
const str = 'hello';
str.at(1) // "e"
str.at(-1) // "o"
```

如果参数位置超出了字符串范围，`at()`返回`undefined`。

该方法来自数组添加的`at()`方法，目前还是一个第三阶段的提案，可以参考《数组》一章的介绍。




# 简明

[ECMAScript 6 简明教程](https://www.runoob.com/w3cnote/es6-concise-tutorial.html)

### 2. 箭头函数（Arrow Functions）

ES6 允许使用“箭头”（`=>`）定义函数。为什么叫Arrow Function？因为它的定义用的就是一个箭头： 可**省略3样**

- 如果函数返回的是一个表达式，那么可**省略curly braces{}和return 关键字**。 注意所以一般curly braces和return都是在一起出现的。箭头函数肯定是返回点东西的。只是有的时候省略了而已。有的时候如果明确返回的内容，也可以省略return

- 如果函数内部有多个语句，不是一个表达式，那么花括号会允许我们在函数中编写多个语句，但是我们需要显式地 return 来返回一些内容。

- 如果函数**只有一个参数**，那么可**省略小括号** args => expression

- 无参数且内部有**多个语句**的函数一般表示为 **() =>{return }**

  ```js
  （1）eg
  # 标准形式，创建匿名函数，赋值给变量hello
  let hello = function() {
      return "Hello World!";
  }
  # 一个表达式，省略curly braces{}和return 关键字
  let hello = () => "Hello World!";
  
  （2）eg
  let hello = (val) => "Hello " + val;
  # 有一个参数时可省略小括号
  let hello = val => "Hello " + val;
  ```

```
const outerFunction = () => {
  console.log("Outer function");

  const innerFunction = () => {
    console.log("Inner function");
  };

  // 不会立即执行 innerFunction
  return innerFunction;
};

const returnedFunction = outerFunction();

// 调用 innerFunction
returnedFunction();

```

在这个例子中，`outerFunction` 返回了一个箭头函数 `innerFunction`，但并没有立即执行。当 `outerFunction` 被调用时，它返回了 `innerFunction` 的引用，这个引用被赋给了 `returnedFunction`。然后，你可以在之后的代码中调用 `returnedFunction()` 来执行 `innerFunction`。

总的来说，箭头函数内部的函数不会在声明时立即执行，而是需要在适当的时机被调用才会执行。

### 3. 函数参数默认值

ES6 中允许你对函数参数设置默认值：

```
let getFinalPrice = (price, tax=0.7) => price + price * tax;
getFinalPrice(500); // 850
```

### 4. Spread / Rest 操作符

Spread / Rest 操作符指的是 ...，具体是 Spread 还是 Rest 需要看上下文语境。

当被用于迭代器中时，它是一个 Spread 操作符： 相当于**复制了**一个对象和新的数组，不使用和改变原来的数组和对象。

```
function foo(x,y,z) {
  console.log(x,y,z);
}
 
let arr = [1,2,3];
foo(...arr); // 1 2 3
```

当被用于函数传参时，是一个 Rest 操作符：**收集所有剩余**的参数到一个数组当中

```
function foo(...args) {
  console.log(args);
}
foo( 1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

### 5. 对象词法扩展

ES6 允许声明在对象字面量时使用简写语法，来初始化属性变量和函数的定义方法，并且允许在对象属性中进行计算操作：

```
function getCar(make, model, value) {
  return {
    // 简写变量
    make,  // 等同于 make: make
    model, // 等同于 model: model
    value, // 等同于 value: value
 
    // 属性可以使用表达式计算值
    ['make' + make]: true,
 
    // 忽略 `function` 关键词简写对象函数
    depreciate() {
      this.value -= 2500;
    }
  };
}
 
let car = getCar('Barret', 'Lee', 40000);
 
// output: {
//     make: 'Barret',
//     model:'Lee',
//     value: 40000,
//     makeBarret: true,
//     depreciate: [Function: depreciate]
// }
```

### 6. 二进制和八进制字面量

ES6 支持二进制和八进制的字面量，通过在数字前面添加 0o 或者0O 即可将其转换为八进制值：

```
let oValue = 0o10;
console.log(oValue); // 8
 
let bValue = 0b10; // 二进制使用 `0b` 或者 `0B`
console.log(bValue); // 2
```

### 7. 对象和数组解构

解构赋值是一种在 JavaScript 中从数组或对象中提取值, 并赋给变量的语法。这使得在使用数组和对象时更加方便。解构可以避免在对象赋值时产生中间变量.

```
function foo() {
  return [1,2,3];
}
let arr = foo(); // [1,2,3]
 
let [a, b, c] = foo(); // 声明变量的时候， 使用[]代表将数组的value提取出来，并赋值给定义的变量， 【】就代表destructuring
console.log(a, b, c); // 1 2 3
 ==============================================================================================================
const person = { name: 'John', age: 30, country: 'USA' };

// 对象解构
const { name, age, country } = person; //声明变量的时候，使用{}代表解构对象， 这些变量名字必须使用对象中定义字段的名字

console.log(name);    // 输出 'John'
console.log(age);     // 输出 30
console.log(country); // 输出 'USA'

```

### 8. 对象超类

ES6 允许在对象中使用 super 方法：

```
var parent = {
  foo() {
    console.log("Hello from the Parent");
  }
}
 
var child = {
  foo() {
    super.foo();
    console.log("Hello from the Child");
  }
}
 
Object.setPrototypeOf(child, parent);
child.foo(); // Hello from the Parent
             // Hello from the Child
```

### 9. 模板语法和分隔符

ES6 中有一种十分简洁的方法组装一堆字符串和变量。

- ${ ... } 用来渲染一个变量
- ` 作为分隔符

```
let user = 'Barret';
console.log(`Hi ${user}!`); // Hi Barret!
```

### 10. for...of VS for...in

for...of 用于遍历一个迭代器，如数组：

```
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname of nicknames) {
  console.log(nickname);
}
// 结果: di, boo, punkeye
```

for...in 用来遍历对象中的属性：

```
let nicknames = ['di', 'boo', 'punkeye'];
nicknames.size = 3;
for (let nickname in nicknames) {
  console.log(nickname);
}
Result: 0, 1, 2, size
```

### 11. Map 和 WeakMap

ES6 中两种新的数据结构集：Map 和 WeakMap。事实上每个对象都可以看作是一个 Map。

一个对象由多个 key-val 对构成，在 Map 中，任何类型都可以作为对象的 key，如：

```
var myMap = new Map();
 
var keyString = "a string",
    keyObj = {},
    keyFunc = function () {};
 
// 设置值
myMap.set(keyString, "value 与 'a string' 关联");
myMap.set(keyObj, "value 与 keyObj 关联");
myMap.set(keyFunc, "value 与 keyFunc 关联");
 
myMap.size; // 3
 
// 获取值
myMap.get(keyString);    // "value 与 'a string' 关联"
myMap.get(keyObj);       // "value 与 keyObj 关联"
myMap.get(keyFunc);      // "value 与 keyFunc 关联"
```

**WeakMap**

WeakMap 就是一个 Map，只不过它的所有 key 都是弱引用，意思就是 WeakMap 中的东西垃圾回收时不考虑，使用它不用担心内存泄漏问题。

另一个需要注意的点是，WeakMap 的所有 key 必须是对象。它只有四个方法 delete(key),has(key),get(key) 和set(key, val)：

```
let w = new WeakMap();
w.set('a', 'b'); 
// Uncaught TypeError: Invalid value used as weak map key
 
var o1 = {},
    o2 = function(){},
    o3 = window;
 
w.set(o1, 37);
w.set(o2, "azerty");
w.set(o3, undefined);
 
w.get(o3); // undefined, because that is the set value
 
w.has(o1); // true
w.delete(o1);
w.has(o1); // false
```

### 12. Set 和 WeakSet

Set 对象是一组不重复的值，重复的值将被忽略，值类型可以是原始类型和引用类型：

```
let mySet = new Set([1, 1, 2, 2, 3, 3]);
mySet.size; // 3
mySet.has(1); // true
mySet.add('strings');
mySet.add({ a: 1, b:2 });
```

可以通过 forEach 和 for...of 来遍历 Set 对象：

```
mySet.forEach((item) => {
  console.log(item);
    // 1
    // 2
    // 3
    // 'strings'
    // Object { a: 1, b: 2 }
});
 
for (let value of mySet) {
  console.log(value);
    // 1
    // 2
    // 3
    // 'strings'
    // Object { a: 1, b: 2 }
}
```

Set 同样有 delete() 和 clear() 方法。

**WeakSet**

类似于 WeakMap，WeakSet 对象可以让你在一个集合中保存对象的弱引用，在 WeakSet 中的对象只允许出现一次：

```
var ws = new WeakSet();
var obj = {};
var foo = {};
 
ws.add(window);
ws.add(obj);
 
ws.has(window); // true
ws.has(foo);    // false, foo 没有添加成功
 
ws.delete(window); // 从结合中删除 window 对象
ws.has(window);    // false, window 对象已经被删除
```

### 13. class类

ES6 中有 class 语法。值得注意是，这里的 class 不是新的对象继承模型，它只是原型链的语法糖表现形式。

    1. Constructor: The `constructor` method is executed when an instance of the class is created.If you want to define specific initialization logic or set properties  with default values when creating an instance of the class, you can  still use an explicit constructor. However, if you don't need any  special initialization for your class instances, you can omit the  constructor, and JavaScript will handle it for you.
     2. `showId` Method: This is an instance method that belongs to the class. When called on an instance of the `Task` class, it will log the number `23` to the console.
     3. `loadAll` Static Method: This is a static method of the class. Static methods are associated with the class itself rather than the instances of the class. In this case, the `loadAll` method logs "Loading all tasks.." to the console when called on the `Task` class directly, not on instances of the class
     4. In JavaScript, a class is indeed a type of function. However, when we  talk about functions inside a class, we are not referring to the class  itself being a function. Instead, we are talking about the methods  defined inside the class. 

函数中使用 static 关键词定义构造函数的的方法和属性：

```
class Task {
  constructor() {
    console.log("task instantiated!");
  }
 
  showId() {
    console.log(23);
  }
 
  static loadAll() {
    console.log("Loading all tasks..");
  }
}
 
console.log(typeof Task); // function
let task = new Task(); // "task instantiated!"
task.showId(); // 23
Task.loadAll(); // "Loading all tasks.."
```

创建一个Person类

````
class Person {
	//构造器方法，用来给instance初始化，这样在new初始化实例时，将参数传递到实例的属性上面。
	constructor(name,age){
	//构造器中this是类的实例对象，当时new的是p1，就是实例p1，这样就将传递过来的name和age参数，放进实例自身的属性上。可以通过操纵类的     //方法，统一操纵实例的属性。
		this.name = name
		this.age = age
	}
	//一般方法,speak 方法放在了类的原型对象上，供实例使用
	//通过Person实例调用speak时，speak中 this 就是Person实例
	speak(){
		console.log(`my name is ${this.name}, my age is ${this.age}`);
	}
}
//创建一个Person的实例对象
const p1 = new Person('tom',18)
const p2 = new Person('jerry',19)
//当读取了对象本身不存在的属性和方法，就去原型链上（prototype）找，也就是类上去找
p1.speak()
````

类中的继承和超集：

```
class Car {
  constructor() {
    console.log("Creating a new car");
  }
}
 
class Porsche extends Car {
  constructor() {
    super();
    console.log("Creating Porsche");
  }
}
 
let c = new Porsche();
// Creating a new car
// Creating Porsche
```

extends 允许一个子类继承父类，需要注意的是，子类的constructor 函数中需要执行 super() 函数，super帮你调用父类的构造器函数。如果子类和父类的constructor是一样的，就无需写构造器。当多出属性的时候，可以写一个super（）方法，表示继承的父类的属性和方法。

当然，你也可以在子类方法中调用父类的方法。

总结：

- **类的声明不会提升（hoisting)，如果你要使用某个 Class，那你必须在使用之前定义它，否则会抛出一个 ReferenceError 的错误**
- **在类中定义函数不需要使用 function 关键词**
- 类中的构造器不是必须写的，要对实例进行一些初始化操作，如添加指定属性时才写。
- 如果A类继承了B类，而且A类写了构造器，那么A类构造器中super方法是必须要调用的。
- 类中定义的方法，都是放在了类的原型对象上，供实例去使用。

### 14. Symbol

Symbol 是一种新的数据类型，它的值是唯一的，不可变的。ES6 中提出 symbol 的目的是为了生成一个唯一的标识符，不过你访问不到这个标识符：

```
var sym = Symbol( "some optional description" );
console.log(typeof sym); // symbol
```

注意，这里 Symbol 前面不能使用 new 操作符。

如果它被用作一个对象的属性，那么这个属性会是不可枚举的：

```
var o = {
    val: 10,
    [ Symbol("random") ]: "I'm a symbol",
};
 
console.log(Object.getOwnPropertyNames(o)); // val
```

如果要获取对象 symbol 属性，需要使用Object.getOwnPropertySymbols(o)。

### 15. 迭代器（Iterators）

迭代器允许每次访问数据集合的一个元素，当指针指向数据集合最后一个元素时，迭代器便会退出。它提供了 next() 函数来遍历一个序列，这个方法返回一个包含 done 和 value 属性的对象。

ES6 中可以通过 Symbol.iterator 给对象设置默认的遍历器，无论什么时候对象需要被遍历，执行它的 @@iterator 方法便可以返回一个用于获取值的迭代器。

数组默认就是一个迭代器：

```
var arr = [11,12,13];
var itr = arr[Symbol.iterator]();
 
itr.next(); // { value: 11, done: false }
itr.next(); // { value: 12, done: false }
itr.next(); // { value: 13, done: false }
 
itr.next(); // { value: undefined, done: true }
```

你可以通过 [Symbol.iterator]() 自定义一个对象的迭代器。

### 16. Generators

Generator 函数是 ES6 的新特性，它允许一个函数返回的可遍历对象生成多个值。

在使用中你会看到 * 语法和一个新的关键词 yield:

```
function *infiniteNumbers() {
  var n = 1;
  while (true){
    yield n++;
  }
}
 
var numbers = infiniteNumbers(); // returns an iterable object
 
numbers.next(); // { value: 1, done: false }
numbers.next(); // { value: 2, done: false }
numbers.next(); // { value: 3, done: false }
```

每次执行 yield 时，返回的值变为迭代器的下一个值。



