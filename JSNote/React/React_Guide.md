

# JSX

**J**avaScript **S**yntax e**X**tension, this extension allows developers to describe and create HTML elements by writing HTML markup code inside of JavaScript files. With React, you write declarative code(声明式编程)， which means you define the target HTML structure component, and don't have to handle DOM directly.  Only care what to do, don't have to worry how to do. For example, SQL is also declarative code. But JSX is a feature that's not supported by browsers. So before it reaches the browser, It's transformed behind the scenes(ReactDOM will be responsible for outputting those components' content on the screen) to code that does work in the browser.

Vite is a modern build tool used for web development, Vite can transform JSX (JavaScript XML) code. Vite is designed to handle JSX syntax commonly used in React applications. It has built-in support and configurations that allow developers to write JSX code directly within their Vue.js or React projects. That is why we could write JSX files without Babel in the overall project.

> JSX 的特点
>
> 1 **类 HTML 语法**： JSX 看起来像 HTML，但它实际上是 JavaScript 的一种语法扩展。在 React 应用中，使用 JSX 编写的标签会被编译成标准的 JavaScript 函数调用，如 const element = <h1>Hello, world!</h1>; 会被编译为const element = React.createElement('h1', null, 'Hello, world!');
>
> 2 JSX 支持将 HTML 属性映射到 React 组件的 props 中。属性名可以与 HTML 相同，但有些属性（如 `class`）在 JSX 中需要用 React 规范（如 `className`）来书写
>
> 3 JSX 不能直接在浏览器中运行，需要通过 Babel 等工具将其编译成普通的 JavaScript 函数。例如，上述的 JSX 代码会被编译成标准的 `React.createElement` 调用
>
> 为什么使用JSX
>
> **可读性**：使用类似 HTML 的语法让代码更直观，特别是在构建复杂的用户界面时。
>
> **组件化开发**：它非常适合 React 组件化开发模式，能够更好地将视图逻辑与界面结构结合。
>
> **增强开发体验**：JSX 更简洁，易于编写和维护，同时拥有与 JavaScript 完全相同的能力和动态性。

# Destructing Assignment

Destructing in JavaScript is a convenient way to extract values from objects or arrays and assign them to variables using a syntax that looks similar to the structure being extracted. It allows you to unpack values from arrays or properties from objects into distinct variables. It's a powerful feature used for various purposes like copying arrays,  merging arrays, passing function arguments, and creating shallow copies of objects.

是一种在 JavaScript 中从数组或对象中提取数据并赋值给变量的语法

```
// Array destructuring
const numbers = [1, 2, 3, 4, 5];
const [first, second, , fourth] = numbers;
现在，变量 first 的值是 1，second 的值是 2，fourth 的值是 4。这使得我们可以更方便地从数组中提取和使用特定位置的值。


// Object destructuring
const person = { name: 'John', age: 30, city: 'New York' };
const { name, age, city } = person;
```

# Spread Operator

The spread operator (`...`) in JavaScript is a syntax that allows an iterable (such as an array, string, or object expressions) to  be expanded or spread **into individual elements** or key-value pairs. 

## Array Spread 数组中的使用

将数组的元素展开为独立的值

```
const numbers = [1, 2, 3];
const newNumbers = [...numbers, 4, 5];

const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];

console.log(combined);  // Output: [1, 2, 3, 4, 5, 6]

```

## 在函数调用中的使用

展开数组中的元素，传递为函数的参数列表。

```
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
console.log(sum(...numbers));  // Output: 6

```

## **在对象中的使用**

## Object Spread (Shallow Copy):

展开对象的属性，常用于对象合并或克隆。

如果属性重复，后面的值会覆盖前面的值：

```
const person = { name: 'John', age: 30 };
const copiedPerson = { ...person };

console.log(copiedPerson);  // Output: { name: 'John', age: 30 }

const obj1 = { a: 1, b: 2 };
const obj2 = { b: 3, c: 4 };
const combined = { ...obj1, ...obj2 };  // { a: 1, b: 3, c: 4 }
```

结论：
    扩展运算符（spread operator）会创建对象的浅拷贝。
    对象的属性如果是引用类型（如对象或数组），它们还是会共享相同的引用地址。
    深拷贝需要递归地拷贝对象的所有层级，可以使用 JSON.parse(JSON.stringify()) ,但这种方法对于带有函数或不可序列化属性的对象不适用。更全面的方法是使用 lodash 的 _.cloneDeep() 或者现代框架中提供的深拷贝工具。

    简单来说，浅拷贝会让第一层级的属性引用地址变化，而深拷贝则会递归地让所有层级的属性引用地址都独立。
    
    JSON.stringify() 是一个 JavaScript 方法，用于将 JavaScript 对象或值转换为 JSON 字符串。它通常与 JSON.parse() 一起使用，后者用于将 JSON 字符串转换回 JavaScript 对象。
    
    JSON.parse() 是一个 JavaScript 方法，用于将 JSON 字符串解析为 JavaScript 对象。它通常与 JSON.stringify() 一起使用，后者用于将 JavaScript 对象转换为 JSON 字符串。

## Rest Operator

```
function foo(...args) {
  console.log(args);
}
foo( 1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

## Hoisted Function

Function declarations are hoisted, meaning you can use them before they are defined in the code. This allows you to use the function before it appears in the code.

Function declarations are often used for defining reusable functions that can be called from anywhere within their scope.

```
// Function call before declaration
sayHello(); // This works due to hoisting

// Function declaration 函数声明的创建方式，有一个明确的标识符名称
function sayHello() {
  console.log('Hello!');
}
```

**函数表达式** 有等号
```
// This won't work due to hoisting limitations with function expressions
sayHi(); // Error: sayHi is not a function

// Function expression 函数表达式，函数表达式是将函数赋值给变量，函数本身可以是匿名的
var sayHi = function() {
  console.log('Hi!');
};
匿名函数的特点是没有具体的函数名称，它们通常在需要时直接定义并使用，不会被其他代码引用。这样的函数可以方便地用作临时函数或者简短的逻辑片段，比如作为回调函数传递给其他函数，或在特定场景下使用。
箭头函数也可以是匿名的。它们使用 => 语法，可以更简洁地定义函数。箭头函数通常在回调函数或需要简短函数定义的地方使用。
```

# 回调函数

<https://chinese.freecodecamp.org/news/javascript-callback-functions/>

一个普通函数，可以传入数据，用相同的方法对不同的数据进行处理；而回调函数相当于在函数中传入一个算法（函数），用不同的算法对相同的数据进行处理。

**当函数作为参数被调用的时候，实际上就是将调用此函数的引用传递（指针）给了此参数，在函数内部需要被执行的地方再次调用此函数。因此称之为回调函数**。

```
function doSomethingAsync(callback) {
  setTimeout(() => {
    try {
      // 假设在这里发生了一个错误
      throw new Error("Something went wrong!");
    } catch (err) {
      // 捕捉错误并将其传递给回调函数
      callback(err);
    }
  }, 1000);
}

// 定义回调函数（一个算法）
function errorCallback(err) {
  console.error("Error:", err.message);
}

// 定义下一个回调函数（另一个算法）
function logCallback(err) {
  console.log("Error:", err.message);
}

doSomethingAsync(errorCallback);
doSomethingAsync(logCallback);
doSomethingAsync((errs) => {
  console.log("Error:", errs.message);
});
```

经过以上的调用，我们成功地延迟了errorCallback（）和logCallback（）函数的执行，而且是在只有抛出错误的情况下才会运行，因为这两个函数都是同步函数，top level的函数，也是hoisted function，如果不放在异步web api中调用，直接调用的话，是不会达到延迟运行的效果的。这种方式**使得回调函数能够在某些特定的条件下被触发执行**，使代码更加灵活，特别是在处理异步操作、事件处理或需要延迟执行的场景中。



当您将一个函数作为参数传递给另一个函数时，实际上是将函数的引用（指向函数在内存中的地址）传递给了该函数，而不是将函数的执行结果传递给了该函数。换句话说，函数参数接收到的是一个函数的指针，它指向内存中存储的函数代码，而不是实际的函数执行结果。函数作为参数传递时，传递的是函数的引用，而不是函数的执行结果。这意味着被传递的函数可以在**接收到参数的函数内部根据需要被调用**，而不是在传递时立即执行。

# Try...Catch

在 JavaScript 中，`try-catch` 语句用于捕获和处理在代码执行过程中可能发生的异常。使用 `try-catch` 可以防止程序因未处理的异常而崩溃，并允许你在发生错误时执行特定的处理逻辑。以下是一些常见的使用场景：

### 1. 异步操作中的错误处理
在处理异步操作时，特别是使用 `async/await` 语法时，可以使用 `try-catch` 来捕获和处理可能发生的错误。

```javascript
async function fetchData() {
    try {
        const response = await fetch('https://api.example.com/data');
        const data = await response.json();
        console.log(data);
    } catch (error) {
        console.error('Error fetching data:', error);
    }
}
```

# Component

Name starts with an uppercase character, Multi-word should be written in CamelCase(e.g."MyHeader"). 

The function must return an actual HTML that can be rendered('displayed on the screen') by React. Remember you're not calling these components like functions yourself in your code, instead you are using them as HTML elements, and under the hood, React will call the actual functions.

## Custom Components

Name starts with uppercase, wraps html elements or other custom components. its name usually starts with an uppercase letter by convention (e.g., `MyComponent`).  **components are JavaScript functions or classes that return JSX**, When the component is called (rendered), React executes that component function and processes the returned JSX（实际上就是react的virtual DOM）. React then analyzes the returned JSX to understand the structure of the  UI and creates a virtual representation of the DOM called the "virtual  DOM tree."  This virtual DOM is a lightweight copy of the actual DOM and contains React elements representing the UI.

React performs a process called "reconciliation" (对账) where it compares the previous virtual DOM with the newly generated one to identify the differences (diffing). React efficiently updates only the parts of the real DOM that have changed, minimizing unnecessary re-rendering and improving performance.

Eventually, React translates the processed JSX into actual HTML elements and updates the real DOM to reflect those changes, resulting in the  updated user interface being displayed in the browser.

```
export default function Header() {
  return (
    <header id="main-header">
      <div id="title">
        <img src={logoImg} alt="A restaurant" />
        <h1>ReactFood</h1>
      </div>
      <nav>
      </nav>
    </header>
  );
}

//call this component with self-clsoing tag, like <Header />

function App() {
  return (
    <UserProgressContextProvider>
      <CartContextProvider>
        <Header />
      </CartContextProvider>
    </UserProgressContextProvider>
  );
}
```

> There should be something followed after return keyword, can't restart a new line and without any code after return, or it will be unreachable because JavaScript would automatically add semicolon after return keyword, which result in unreachable or unreadable code.

## Dynamical value

Outputting dynamic value with **curly braces{}**, allow you to embed any **JavaScript expression** directly inside JSX

箭头函数也是一个JS expression，`(e) => errorTypeChange(e)` 是一个**箭头函数表达式**，返回的是 `undefined`。React 允许这种函数表达式作为 `onChange` 的值来触发事件处理。虽然没有明确返回值。

```
A JavaScript expression is any valid piece of code that produces a value. Expressions can be as simple as a variable name or a literal value (like 5 or "hello"), or they can be more complex, involving calculations, function calls, or logical evaluations
```

we can use that same syntax to load our images， import image into JavaScript file is not something you can normally do in JavaScript, but here would work, because of that same build process in React, would also transform it. such as import ".css".

## Making components reusable  with Props

如果一个组件我们想重复使用四次，但是每次使用的时候，应该填充不同的数据。React allows you to pass data to component via a concept called "Props", Therefore, React will pass a value for this props parameter to this function when it calls it. 所以在创建组件的时候，会携带一个参数，这个参数就是Props属性，Props是一个拥有**键值对的对象**。收集所有该组件的属性。

```
function Greeting(person) {
  return <h1>Hello, {person.name}! You are {person.age} years old.</h1>;
}

const element = <Greeting name="John" age={30} />;

解构赋值，此时的name也可以是一个对象，不一定是一个字符串变量
function Greeting({ name, age }) {
  return <h1>Hello, {name}! You are {age} years old.</h1>;
}
const element = <Greeting name="John" age={30} />;

```

![image-20231101115046617](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231101115046617.png)

- 通过以上过程 ，就将props对象参数传递给了函数组件CoreConcept()，然后就可以通过在函数内部使用props对象的属性，灵活控制此函数组件输出结构相同的，但包含内容不容的UI组件。
- 而且在调用组件的时候，通过Props，我们不仅仅只可以输入字符串，还可以输入数字，对象以及数组等内容。
- 当然还可以使用解构赋值，代替props对象。

![image-20231101182914328](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231101182914328.png)

## The special children Prop

Children is a prop that is not set with help of component attributes like others(title,description above just like a html attribute when rendered) . Instead this children prop here simply refers to the **content** **between your component** tags. so we don't set children like that.  组件中间的内容，就是组件的children属性。

**非自闭合标签：**

- 当组件需要包含内容或子组件时，需要使用成对的开闭标签，如 `<ComponentName>.如果放置内容，有children元素.</ComponentName>`。

![image-20231101183507457](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231101183507457.png)

## React Responding to Events（箭头函数）

In react, you could **add event listeners** to elements by adding a special attribute or called props to those HTML elements. Those HTML  elements (Native HTML elements) support many on-something props, such as 'onClick'. and value you should provide for any event should be a function. Then we can define this function.  

We could define a **function inside a component function**, this is allowed by JS. and these functions could be only called in this main component function. And the advantage of defining these event handler functions inside the component function is that they then have access to the component's props and state.

在React中，将函数引用直接赋值给事件处理函数时，如果这个函数不需要接受任何参数，那么直接赋值是合适的. 传递函数的引用而不是调用函数。这是因为传递引用允许 React 在适当的时机调用该函数。

```
function handleClick() {
  console.log("Button clicked!");
}

// 直接赋值给事件处理函数
<button onClick={handleClick}>Click me</button>
```
上述代码中，`handleClick` 不接受任何参数，因此可以直接赋值给 `onClick` 事件处理函数。

但是，如果你希望事件处理函数能够接收参数，例如其他自定义参数，那么需要使用箭头函数或者在事件处理函数内部使用闭包。**原因是在直接赋值的情况下，事件处理函数的调用是由React引擎进行的，并传递相关的事件对象作为参数，不会传递额外的参数**。而使用箭头函数或闭包可以提供更多的控制和灵活性，允许你传递**自定义参数**给事件处理函数。这样确保了事件处理程序在点击按钮时执行，而不是在组件渲染时立即执行。

```
function handleClickWithParameter(param) {
  console.log("Button clicked with parameter:", param);
}

// 使用箭头函数传递参数
<button onClick={() => handleClickWithParameter("CustomParam");}>Click me</button>

// 或者使用闭包传递参数, 在函数当中调用函数
<button onClick={function() { handleClickWithParameter("CustomParam"); }}>Click me</button>
```

我们使用了箭头函数和闭包，以便将自定义参数传递给事件处理函数。这样做可以提供更大的灵活性，根据需要传递不同的参数。

The event handler should pass the event object to the function if you want to access the event data and the value of the input element.

************

- **直接传递函数引用**：父组件将处理函数的引用传递给子组件，此时无需传递参数，而子组件在适当的时间调用该函数，并传递所需参数。
- **自定义参数**：如果你需要在事件处理函数中传递自定义参数（例如字符串 `'test'`），则使用箭头函数或闭包函数来封装调用。

# 闭包 closure

### 闭包的工作原理

当一个函数在内部定义了另一个函数，并且内部函数引用了外部函数的变量时，就形成了闭包。即便外部函数已经执行完毕，内部函数仍然能够访问和使用那些变量，因为 JavaScript 引擎会在内存中保留这些变量的引用。

闭包之所以称为“闭包”，是因为它“包住”了创建时的作用域，并允许内部函数在执行时“闭合”或“绑定”这个作用域。这使得即使外部函数已经结束，内部函数依然可以访问和操作它所“包裹”住的环境变量。

### 如果内部函数不引用外部变量，还算闭包吗？

不算！如果内部函数**不引用外部函数的变量**，即使在外部函数中定义了该内部函数，也不会形成闭包。这是因为没有引用外部变量，JavaScript 引擎不需要额外保留外部函数的作用域。

## Managing State & Hooks

import  { useState } from 'react'

this is so-called react hook. All these functions that starts with use in React projects are React Hooks. They are technically regular functions, but they must only be called **in side of React component functions** or inside of other React Hooks, and **only call them on the top level,** not inside of some other inner functions.

useState() does accept an argument, which is basically the default value you want React to store and you want react to use when this component is first be rendered, and also could be an object including all the values if there are more values you want to set. 然后会返回一个数组，我们通过数组解构去将这两个元素存储在两个变量当中。第二个变量将始终是一个函数，可以执行它用来更新这个状态。

![image-20231101210359610](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231101210359610.png)

Now with this useState Hook, we have a way of **telling React that this component function must be executed again**. and since the app component uses the others component inside of it. Those component function was also executed again.

Once state is updated, now what happens is that the component function which this state belongs will be executed again. React manages the association between state and components internally.  When state changes occur within a component, React knows which specific  component's function needs to be re-executed to reflect those changes in the UI. Each component's state is localized to that component and  doesn't affect or belong to any other components.

1. **当前状态的值（state）：** 第一个元素是当前状态的值，可以通过这个值读取或更新状态。
2. **更新状态的函数：** 第二个元素是一个用于更新状态的函数。当调用这个函数时，React 会重新渲染组件，并将新的状态值应用到组件中

## Working with Fragments

1. **React 组件返回值规则：** 在 React 中，每个组件的 `render` 方法或函数应该返回一个根元素。这是为了确保组件的输出是清晰、一致且易于处理的。通常，你的组件会有一个根元素包含所有的内容。

2. **Fragment 的作用：** 有时候你可能需要返回多个元素，但又不想引入多余的 DOM 节点。这时就可以使用 React 提供的 Fragment 组件。Fragment 允许你在不引入额外 DOM 节点的情况下返回多个元素。它是一个虚拟的包裹器，不会在最终渲染的 DOM 中留下任何痕迹。

3. **为什么不用 div：** 在以前的 React 版本中，如果你想返回多个元素，你必须使用一个实际的 HTML 元素（通常是 `<div>`）作为根元素。然而，这可能会引入不必要的 DOM 层级，有时可能不符合语义化或样式的要求。所以，为了解决这个问题，React 引入了 Fragment，让你可以更干净地组织你的组件输出。

   ```
   import React, { Fragment } from 'react';
   
   const MyComponent = () => {
     return (
       <Fragment>
         <p>Paragraph 1</p>
         <p>Paragraph 2</p>
       </Fragment>
     );
   };
   ```

## 收集剩余参数

```
export default function Section({title, children, ...props}) {//收集参数到js对象中，然后在此组件中以props的名称使用该对象
	return (
	  <section {...props}>    //在props对象上使用spread操作符，将其展开散播
	    <h2>{title}</h2>
	    {children}
	  </section>
	)
}
```

## Updating State Based On Old State（箭头函数）

When updating your state based on the previous value of that state, you should pass **a function to that state updating function** instead of that just new state value you wanna have. This is a strong recommendation by the React team.  Because this function which you pass here will be called by react and it **will automatically get the current state value(not updated already) as the input**. That is previous value here as an input. So we could accept a parameter that could be named editing. should then return the new state you wanna set.

主要因为state的更新并不是即时的，所以有时直接更新有可能获取的并不是当前最新状态值。

```
this.setState({ count: this.state.count + 1 });
this.setState({ count: this.state.count + 1 });
```

通过传递一个回调函数给 `setIsEditing`，你可以确保状态更新基于最新的状态值：

![image-20231103084014043](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231103084014043.png)

# Update object Immutably

"Immutably" 是 "immutable"  [ɪˈmjuːtəbl]（不可变的）一词的副词形式。在编程领域，不可变性是指一旦创建了一个对象，就不要更改其内容或状态。相反，任何修改操作都将返回一个新的对象，而不是直接修改原始对象。

推荐使用不可变更新的原因是为了**确保 React 能够正确检测到状态变化**，从而触发必要的重新渲染。这种方式不仅能让代码更具可预测性，还可以避免由直接修改状态导致的渲染问题。

利用展开运算符，创建新的对象，是一种shallow copy, 创建了一个新的对象。对于浅拷贝后的对象来说，在第一层级上，新创建的对象与原始对象是独立的，但对于嵌套对象或数组等引用类型的属性，它们还是会共享相同的引用地址。

```
const originalObject = {
  a: 1,
  nestedObj: {
    b: 2
  }
};

// 浅拷贝
const shallowCopy = { ...originalObject };

// 修改浅拷贝后的对象中的属性
shallowCopy.a = 5;
shallowCopy.nestedObj.b = 10;

console.log(originalObject.a); // 输出 1，原始对象的属性不受影响
console.log(originalObject.nestedObj.b); // 输出 10，原始对象的嵌套对象属性受到影响
```

```
const array = [1, 2, 3, 4, 5];
const removed = array.splice(1, 2); // 从索引 1 的位置开始删除 2 个元素
console.log(array); // 输出: [1, 4, 5]
console.log(removed); // 输出: [2, 3] 被删除的元素
```

如果直接修改了状态，而没有创建新的状态对象，React  就难以判断什么时候进行重新渲染。React在比较状态变化时，通常会比较对象的引用（即内存地址），而不是对象的内容。如果状态对象的引用没有变化，React 可能会认为状态没有发生变化，从而不触发重新渲染。如果对象的引用发生了变化，那么React就会认为state发生了变化，从而触发重新渲染。

![image-20231103223426480](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231103223426480.png)

综上：如果更新状态的时候，逻辑复杂，需要进行判断的时候，我们可以使用回到函数，然后将修改好的内容添加到一个新的对象当中

## Lifting State Up

父组件可以通过props给子组件传递信息，例如父组件通过子组件的props属性传递一个会改变状态的函数过去，那么在子组件当中，我们可以通过这个函数改变状态，从而触发父组件的更新。

```
      <AnimatePresence>
          {sideVariationVisible && (
            <SideNewVariation//子组件
              setFilterParams={setFilterParams}//在父组件中调用了子组件，并传递了setFilterParams函数作为属性给子组件
            />
          )}
        </AnimatePresence>
```

![image-20231106091415932](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231106091415932.png)

## Vanilla CSS（原生的）: Advantages & Disadvantages.

![image-20231111151952833](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231111151952833.png)

so what can we do about that if we wanna achieve some scoping, one solution would be to switch to **inline styles**.

![image-20231111160134496](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231111160134496.png)

Therefore simply is that as a React developer, you should always look for opportunities like this. where you can either outsource reusable components that have a certain styling applied to them.

# Debugging React APPs

> Using the browser debugger & breakpoints
>
> 在source里面，可以看到这个项目基本上产生了浏览器可以使用的代码。同时可以在右侧中设置断点

![image-20240306211641445](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20240306211641445.png)

> react devtools， 这样就会新增两个标签，一个是Components，一个是Profiler。可以仔细查看React性能以及如何改进和优化项目。

> Use StrictMode, which is also a component  provided by React. Can help us find certain problems in our app. For example, one of the most important things is that the StrictMode component does is that it will execute every component functions twice instead of just once. It will help you catch potential errors immediately.

>尝试一下webstorm自带的debug功能

# Refs

组件内的标签可以定义ref属性来标识自己，或者叫reference变量关联自己，和原生里面的id很是类似。而不必通过像在传统的 JavaScript 中一样使用 `document.getElementById`才可以获取关联到一个元素。

If we have some inputs in a form, usually we manage what the user enters by simply keeping track of it with the state. with every keystroke, we update our state. so with every keystroke, we update the value we get by the user and we store it in our state. That is a  perfectly fine way of managing this.

But updating a state with every keystroke(按键)， when we only need it when we submit the form, sounds a bit redundant to me, that is a scenario where 'references' could help us, though they are not limited to that.

ref 也是react组件的一个属性，像key属性一样，是一个特殊的属性。这个属性接收一个ref的变量作为输入，这样这个输入组件在最后连接到这个ref值。这种方式非常适合需要在组件中**保存可变数据而不希望重新渲染**的场景.

```
import React, { useRef } from 'react';

const MyComponent = () => {
  const inputRef = useRef(null);//返回一个object

  const handleFocusClick = () => {
    // 在按钮点击时将输入框聚焦
    inputRef.current.focus();
  };

  return (
    <div>
      {/* 使用 ref 关联 input 元素 */}
      <input type="text" ref={inputRef} />//
      {/* 点击按钮将聚焦在输入框 */}
      <button onClick={handleFocusClick}>Focus Input</button>
    </div>
  );
};

export default MyComponent;

```

使用refs，我们可以在最终呈现的HTML元素和其他的JS代码之间建立连接。

Like all React hooks,  useRef is only usable inside of functional components. **what does useRef return and what value does it take**?

It will return an object, which always has a 'current ' prop, and the 'current' prop holds the actual value that ref is connecting with. 

What is the actual value is ? it is really the actual DOM node. which you could now manipulate and do all kinds of things with. 

所以如果console.log(returnRef.current.value), 就可以看到input中输入的值。returnRef.current就是input元素本身。



We could connect a ref to HTML element by going to that element,  and add a special 'ref' prop here, just like the 'key' prop, that is a built-in prop which you can add to any HTML element in React. That is to say, you can connect any HTML element to one of your references. You will very often do that for inputs, because you wanna fetch input data, then a connection will be established. 所以当程序运行到引用的 ref的时候，它就会将存储在useRef当中的值设置为native DOM 的input节点元素。**简单理解就是将HTML元素关联到一个变量上**。这样在其他的JS代码中，就可以对这个关联的变量监测，或者在发生变化的时候有什么动作。



Now, in general, the question is not whether refs or state is better. You can use either of the two.

![image-20231127221717445](C:\Users\simon.cheng\AppData\Roaming\Typora\typora-user-images\image-20231127221717445.png)

# Modal

我们通常使用模态框（Modal）来实现类似于对话框（Dialog）的交互界面。模态框是一种覆盖(overlay)在原始页面内容之上的组件，用于显示重要的信息、收集用户输入或进行某些操作。在 React 中，可以使用各种库（如 Material-UI、React-Bootstrap、Ant Design 等）来创建模态框。以下是一个使用 React Hooks 实现简单模态框的示例.

```
import { useEffect, useRef } from 'react';
import { createPortal } from 'react-dom';

export default function Modal({ children, open, onClose, className = '' }) {
  const dialog = useRef();

  useEffect(() => {
    const modal = dialog.current;

    if (open) {
      modal.showModal();
    }

    return () => modal.close();
  }, [open]);

  return createPortal(
    <dialog ref={dialog} className={`modal ${className}`} onClose={onClose}>
      {children}
    </dialog>,
    document.getElementById('modal')
  );
}
```

# Portal

Portal will help us write cleaner HTML code. Modal basically is an overlay on your page. It is an overlay to the entire page. which should not be deeply nested in our native html code, and it is better to be a direct child of the body next to that root div. this is something you can achieve with portals.

1. `Modal` 组件使用 `ReactDOM.createPortal()` 将 `children` 渲染到 `modal-root` 节点中。
2. `App` 组件中包含一个 `modal-root` 的空 `div`，用于作为 `Modal` 组件的渲染目标。
3. 无论 `Modal` 组件在组件树的哪个位置，它都会被渲染到 `modal-root` 节点下。

```
import React from 'react';
import ReactDOM from 'react-dom';或者 import { createPortal } from 'react-dom'

// Modal 组件
const Modal = ({ children }) => {
  const modalRoot = document.getElementById('modal-root');

  // 使用 createPortal 将子节点渲染到 modal-root 节点上
  return modalRoot ? ReactDOM.createPortal(
    children,
    modalRoot
  ) : null;
};

// 父组件
const App = () => (
  <div>
    <h1>My App</h1>
    <Modal>
      <div>
        <h2>This is a modal dialog</h2>
        <p>Some content here...</p>
      </div>
    </Modal>
    <div id="modal-root"></div> {/* 此处定义 modal-root 节点 */}
  </div>
);

export default App;

```

# useEffect-副作用

>组件函数不可以被转换为async 函数，因为react不允许这样做，原因在于，React 需要同步渲染组件树，如果组件函数是异步的，会破坏这种同步性。但是可以在组件函数里面写async函数的。
>
>
>
>在组件中进行异步操作（如数据获取）时，如果**直接在组件的渲染过程中**(组件的生命周期事件中，挂载-更新-卸载的过程)调用异步函数，并在异步操作完成后更新state，可能会导致无限循环渲染。原因是状态更新会触发重新渲染，而重新渲染会再次调用异步操作，从而形成循环==end up with infinite loop。如果用户交互事件是事件驱动的，例如点击按钮触发，就不需要使用useEffect。
>
>
>
>为了避免无限循环，在**异步操作中，更新state之后**，不要再次触发相同的异步操作，也就不会形成无限循环。所以如果确保异步操作仅在特定事件（如按钮点击）时触发，而不是在每次组件渲染时，直接调用异步函数，使用async函数其实也是可以的。
>
>
>
>所以 **useEffect** 钩子派上用场. 这样组件首先渲染，然后在useEffect调用异步api，无论是否改变了state，页面是否会被重新渲染，只要依赖项不改变，那么这个异步函数就不会再次被调用，那么也不会造成无限循环。其实也是确保**异步操作仅在特定条件下触发或调用**，而不是在每次组件渲染时都触发。
>
>useEffect(() => {
>
>},[])
>
>
>
>**允许我们在组件渲染之后执行，可以确保异步函数只在需要时调用，从而避免无限循环**
>
>如果我们不使用 `useEffect` 钩子，而是等待异步函数操作完成后再渲染页面，理论上是可行的，但实际操作中可能并不理想。这样做会导致一些问题，特别是用户体验和代码维护方面的问题。
>
>组件内部直接等待异步操作完成再渲染页面，会导致用户界面（UI）在数据加载完成之前无法渲染。用户会看到一个空白或静态页面，直到数据加载完成，这会严重影响用户体验。
>
>定义依赖数组控制effect function 何时执行，其实也就是一个控制条件而已, 确保异步操作仅在特定条件下触发或调用。

we would use the effect to solve an infinite loop. use effect needs two arguments, and the first argument is a function(usually an anonymous function) that should warp your side effect code, and the second argument is an array of dependencies of that effect function.

The idea behind useEffect is that this function which you pass as a first argument to useEffect **will be executed by React after every component execution**. If the second argument is an empty array, the effect function would **only execute once**. if you define the dependencies array, then React will take a look at the dependencies specified there, and it will only execute this effect function again if the dependency values change. If you omit the array, will cause an infinite loop.

```
示例（⚠️ 错误用法）：

useEffect(() => {
  fetchData().then(data => {
    setData(data); // 每次设置 state 导致 re-render，又触发 useEffect -> 无限循环
  });
}, [data]); // ⚠️ 错：你不应该监听你刚刚设置的 state 本身
```

如果提供了依赖数组，并且其中的依赖项发生了变化，那么 React 会先调用副作用函数的**清理函数**，然后再调用新的副作用函数。执行清理函数之后再执行新的副作用函数的主要目的是确保新的副作用函数执行时处于一个干净的状态。如果不执行清理函数，而是直接执行新的副作用函数，可能会导致之前的副作用产生的一些未清理的状态或资源泄露影响到新的副作用函数的正确执行，例如取消订阅、清除定时器、清除副作用产生的临时数据等。这样可以避免出现未经处理的副作用，从而提高了应用的健壮性。

异步操作是副作用的一种常见形式，因为它们通常不会立即返回结果，而是在一段时间后返回结果。这包括网络请求、定时器、事件监听器等。异步操作的结果可能会影响App的状态或行为，因此它们被视为副作用。

除了异步操作之外，还有许多其他类型的副作用，例如：

1. 修改全局状态（例如 Redux 或 Context API 中的状态）。
2. 订阅外部事件（例如 DOM 事件、WebSocket 事件等）。
3. 执行 DOM 操作（例如添加或删除元素、修改样式等）。
4. 读写本地存储或浏览器缓存。
5. 向服务器发送数据（例如表单提交、WebSocket 消息等）。

# Context API

如果一些数据，不仅仅是在单个组件里面需要的数据，在其他一些组件当中也需要用到的话，那就要在一个很高级别的父组件中进行管理，这也叫 lifting state up，比如app组件，因为该组件可以通过调用其他组件的属性传递共享数据。当然我们可以在app组件里管理这些共享数据，但意味着我们会写很多code在app组件当中，这通常不是我们想看到的，因此我们通常使用上下文特性以更通用的集中方式管理。

上下文特性允许我们以一种可重用的方式，将数据传播到所有需要它的组件当中。

1. Commonly create a store folder under src for context management, this is just a convention because we can think it as a state store for the entire application， and then create a file called a component name under this folder as you like, such as CartContext.jsx.
2. The Context feature offered by React in the end allows us to spread data into all the components that need it in a very easy and reusable way. 
3. but it is just about spreading data to components, not about changing any values and triggering any component updates. 当然传播的数据可以是有状态的，因此应该由context provider component来管理不断变化的数据。 所以如果我们在上下文provider里面，包含着触发状态更新的动作，并且何种动作对应如何更新状态。那么这样我们就可以将状态更新也进行传递，传递到其他的components当中。
4. 使用createContext创建的上下文是一个对象，具有provider属性。这个provider属性是一个可以输出的component，并且其component中间应包含需要访问此上下文的所有其他组件，这样创建的这个上下文就可以应用在其他的组件当中了。
5. use children prop to wrap our custom component，我们通常使用children属性去包裹那些需要访问此上下文的组件。
6. 现在我们就可以在CartContextProvider的component里面添加一些逻辑，去控制state的更新。

```
import { createContext } from 'react';

//CartContext 实际上是一个包含React组件的对象， 所以C开头大写
//createContext里面对象（数字，字符串，对象，数组）被用作初始值，提供给React中有被这个上下文包含的所有组件
//设置了默认值，程序在输入CartContext会更好的自动完成，设置这个默认上下文值使开发人员的生活更轻松
const CartContext = createContext({
    items: [],
    addItem: (item) => {},
    removeItem: (id) => {},
});

function cartReducer(state,action) {}

export function CartContextProvider({ children }){
	//第一个参数是state，第二个参数是分派触发函数，然后会产生一个新的状态。使用减速器reducer，并为状态设置初始值
	//因为如果状态还没有被更新，这个初始值将被使用
    const [cartState, dispatchCartAction] = userReducer(cartReducer, { items:[] });
    
    //以下，从userReducer获取dispatchCartAction函数，并使用这个函数调用动作，然后cartReducer的action就会执行这个动作，{ type: 'ADD_ITEM', item }就是我们传递给action参数的值。
    function addItem(Item) {
    	dispatchCartAction({ type: 'ADD_ITEM', item });
    }
    
    function removeItem(Item) {
    	dispatchCartAction({ type: 'REMOVE_ITEM', id });
    }

	const cartContext = {
		items: cart.items,
		addItem,
		removeItem
	}
//使用createContext创建的上下文是一个具有provider属性的对象，这个provider属性是一个可以输出的组件，并且其中间应包含需要访问此上下文的所有组件，这样创建的这个上下文就可以应用在其他的组件当中。
//必须响购物车上下文provider添加一个value属性
    return <CartContext.Provider value={cartContext}>{children}</CartContext.Provider>
}

//both CartContextProvider and CartContext will be exported by belowing
export default CartContext;
```



```
// in app component
function App() {
  return (
      <CartContextProvider>
        <Header />
        <Meals />
        <Cart />
        <Checkout />
      </CartContextProvider>
  );
}
```

# useReducer  **/rɪˈdjuːsər/**

useReducer, 其实就是一个state减法器，是一个更通用和灵活的状态管理工具。如果我们在context中，管理要分享出去的state，以及state更新函数，是一个很复杂的事情。这样导致上下文组件函数有点难以阅读。我们可以使用React提供的另一个状态管理的钩子useReducer ，而不是再使用useState管理状态。

`useReducer` 是 React 提供的一个 Hook，用于在函数组件中管理复杂的状态逻辑。它是 `useState` 的替代方案，适用于包含多个子值或需要依赖于先前状态的状态更新逻辑。

### `useReducer` 的基本用法

`useReducer` 接受三个参数：
1. **reducer**：一个纯函数，接收当前状态和一个描述状态变化的动作，返回新的状态。
2. **initialState**：初始状态。
3. **init**（可选）：一个惰性初始化函数，用于计算初始状态。

`useReducer` 返回一个包含当前状态和 `dispatch` 函数的数组。`dispatch` 函数用于分发动作，触发状态更新。

### 示例代码

以下是一个使用 `useReducer` 的示例，展示如何管理复杂的状态逻辑：

#### 定义 `reducer` 函数

```javascript
const initialState = { count: 0 };

function reducer(state, action) {
  switch (action.type) {
    case 'increment':
      return { count: state.count + 1 };
    case 'decrement':
      return { count: state.count - 1 };
    case 'reset':
      return initialState;
    default:
      throw new Error();
  }
}
```

#### 使用 `useReducer` 在组件中管理状态

```javascript
import React, { useReducer } from 'react';

const Counter = () => {
  const [state, dispatch] = useReducer(reducer, initialState);

  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
      <button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
      <button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
    </div>
  );
};

export default Counter;
```

### 解释

1. **定义 `reducer` 函数**：
   - `reducer` 函数接收当前状态 `state` 和一个动作 `action`，根据动作的类型返回新的状态。
   - 在这个示例中，`reducer` 函数处理三种动作类型：`increment`、`decrement` 和 `reset`。

2. **使用 `useReducer`**：
   - `const [state, dispatch] = useReducer(reducer, initialState);`
   - `useReducer` 返回当前状态 `state` 和一个 `dispatch` 函数。
   - `dispatch` 函数用于分发动作，触发状态更新。

3. **在组件中使用 `dispatch`**：
   - 在按钮的 `onClick` 事件中调用 `dispatch` 函数，分发不同的动作。
   - 例如，点击 "Increment" 按钮时，分发 `{ type: 'increment' }` 动作，触发状态更新。

### 适用场景

`useReducer` 适用于以下场景：
- 状态逻辑复杂且包含多个子值。
- 状态更新逻辑依赖于先前的状态。
- 需要在不同组件之间共享状态更新逻辑。

### 总结

`useReducer` 是 React 提供的一个 Hook，用于在函数组件中管理复杂的状态逻辑。它通过 `reducer` 函数和 `dispatch` 函数提供了一种清晰的方式来处理状态更新，适用于包含多个子值或需要依赖于先前状态的状态更新逻辑。



# Redux /rɪˈdʌks/

`redux` 是一个用于 JavaScript 应用程序状态管理的库，而 `useReducer` 是 React 中的一个 hook，用于管理局部状态。它们之间存在以下区别：

1. **Redux**：
   - Redux 是一个独立的状态管理库，用于管理整个应用程序的状态。
   - 它具有一个全局的状态树（单一的状态存储），包含整个应用的状态。
   - Redux 通过 actions 和 reducers 来修改状态。Actions 是描述状态变化的对象，Reducers 是根据 actions 更新状态的函数。
   - Redux 提供了一种机制来处理复杂的状态更新和数据流，包括中间件、异步 action 等。
2. **useReducer**：
   - `useReducer` 是 React 提供的一个 hook，用于在函数式组件中管理局部状态。
   - 它允许你将状态逻辑封装在组件内部，并提供了一种能够处理复杂状态逻辑的方式。
   - `useReducer` 接受一个 reducer 函数和初始状态作为参数，并返回一个包含当前状态和 dispatch 函数的数组。
   - 当调用 dispatch 函数时，会触发 reducer 的执行，并传入当前状态和 action 对象，从而更新状态。

虽然 `useReducer` 可以用于管理局部状态，但它通常用于处理较为复杂的局部状态逻辑。而 Redux 更适用于管理全局状态和应对大型应用中的复杂状态管理需求。在一些情况下，`useReducer` 可以替代 Redux，但并不意味着它们完全相同或互相替代。选择使用 Redux 还是 `useReducer` 取决于项目的规模、复杂性和个人偏好。

# useCallBack

在 React 中，组件卸载后再次加载，使用 `useCallback` 和不使用 `useCallback` 可能会有一些区别，具体取决于组件卸载后的状态和使用方式。

## 使用 useCallback：

- **组件卸载后重新加载**：如果在组件卸载后重新加载，使用 `useCallback` 返回的函数引用可能会保持不变，取决于其依赖项。

- **依赖项未发生变化**：如果 `useCallback` 返回的函数的依赖项在重新加载时没有发生变化，那么它会继续返回相同的函数引用。如果依赖项 `dependency1` 或 `dependency2` 在重新渲染时不发生变化，`memoizedCallback` 函数的引用将保持不变，即使组件重新渲染，函数引用也不会改变。

- **依赖项发生变化**：如果依赖项发生了变化，`useCallback` 将返回一个新的函数引用。否则，它会返回缓存的回调函数实例，这样就可以优化代码，减少内存的引用。

  ```
  const memoizedCallback = useCallback(
    () => {
      // ...function logic
    },
    [dependency1, dependency2]
  );
  
  ```

## 不使用 useCallback：

- **组件卸载后重新加载**：如果组件卸载后重新加载，未使用 `useCallback` 的函数引用会重新创建，即使函数逻辑相同也会创建新的引用。
- **每次重新加载都创建新函数**：在每次重新加载时都会创建新的函数引用，除非函数逻辑或声明发生了变化。

总的来说，`useCallback` 在组件卸载后重新加载时，如果依赖项保持不变，函数引用可能会保持一致。而不使用 `useCallback` 的情况下，函数引用会在每次重新加载时都重新创建。

# 自定义钩子

useEffect，useState都是定义好的hook。

- Only call hooks inside of component functions, and you may also use them inside of other Hooks.
- Only call Hooks on the top level in component functions, must not nested in if statements or nested functions or loops.

The idea behind building custom Hooks is always to wrap and reuse code that goes into your component functions.

自定义钩子函数的特点是它们可以使用React的Hooks API，比如 `useState`、`useEffect` 等。而自定义的标准函数是不可以的。

自定义钩子函数的命名通常以 "use" 开头，例如 `useCustomHook`。

通过这种方式，它提供了一种有效的方式来组织和重用React组件中的复杂逻辑。这些自定义Hook可以被多个组件复用，使得组件之间的逻辑关联更加清晰和可维护。

React的函数组件和类组件之间的一个重要区别是，函数组件通过Hooks API可以使用和管理状态，以及在组件生命周期内执行副作用。这使得函数组件能够拥有类似于类组件的功能，同时保持代码简洁和逻辑清晰。

# Tricks

> 在 React 中，给列表元素提供一个唯一的标识是为了帮助 React 有效地更新 DOM。这个唯一的标识通常使用 `key` 属性来指定。虽然不是绝对必需，但是在使用列表渲染时，给每个列表项提供一个唯一的 `key` 通常是一个良好的实践。



>template literal feature provided by JS by using backticks, which allow us to create a string where parts of that string are dynamic.在模板字符串中，`${}` 包裹的部分表示需要被解析和替换的表达式
>
>`string text ${expression} string text`



> strickMode, every component gets rendered twice by react during development to help us catch potential bugs and errors

