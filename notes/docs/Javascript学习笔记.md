---
title: "Javascript学习笔记"
date: 2025-06-15  # 可选，手动指定日期
layout: default      # 可选，指定布局（如 `post` 或 `default`）
categories: [javascript]  # 可选，添加分类
---
### 一、基础语法与核心概念

#### 1. 变量与作用域
- **变量声明**：
  - `let` 和 `const` 具有块级作用域，即它们的作用范围被限制在最近的一对花括号 `{}` 内。
  - `var` 具有函数作用域，它的作用范围是整个函数。
```javascript
// let 和 const 的块级作用域
{
  let blockScopedLet = 'I am block scoped with let';
  const blockScopedConst = 'I am block scoped with const';
}
// 下面这两行会报错，因为变量超出了作用域
// console.log(blockScopedLet); 
// console.log(blockScopedConst); 

// var 的函数作用域
function varScopeExample() {
  var functionScopedVar = 'I am function scoped with var';
}
// 下面这行会报错，因为变量超出了函数作用域
// console.log(functionScopedVar); 
```
- **作用域链**：JavaScript 采用词法作用域，变量的查找是从当前作用域开始，逐级向上查找，直到全局作用域。
```javascript
function outerFunction() {
  let outerVariable = 'I am from outer function';
  function innerFunction() {
    console.log(outerVariable); // 可以访问外层函数的变量
  }
  innerFunction();
}
outerFunction();
```
- **垃圾回收**：JavaScript 采用标记清除算法进行垃圾回收，当一个对象没有任何引用指向它时，它会被标记为可回收的，然后在适当的时候被回收。
```javascript
function createObject() {
  let obj = { key: 'value' };
  return obj;
}
let myObj = createObject();
myObj = null; // 释放对对象的引用，对象可能会被垃圾回收
```

#### 2. 数据类型与操作
- **原始类型**：JavaScript 有 7 种原始类型，分别是 `string`、`number`、`boolean`、`null`、`undefined`、`bigint` 和 `symbol`。
```javascript
let str = 'Hello, World!'; // string
let num = 42; // number
let bool = true; // boolean
let n = null; // null
let u; // undefined
let big = 123456789012345678901234567890n; // bigint
let sym = Symbol('unique'); // symbol
```
- **引用类型**：常见的引用类型有 `Object`、`Array`、`Function`、`Date` 等。
```javascript
let obj = { name: 'John', age: 30 }; // Object
let arr = [1, 2, 3]; // Array
function func() { console.log('I am a function'); } // Function
let date = new Date(); // Date
```
- **类型转换**：JavaScript 有隐式和显式的类型转换。`===` 是严格相等运算符，会同时比较值和类型。
```javascript
// 隐式转换
let result = 5 + '5'; // 结果是 '55'，数字 5 被隐式转换为字符串
// 显式检查
let num1 = 5;
let str1 = '5';
console.log(num1 === str1); // false，值相同但类型不同
```

#### 3. 函数与执行机制
- **函数定义**：可以使用声明式、表达式或箭头函数来定义函数。
```javascript
// 声明式函数
function declarationFunction() {
  console.log('I am a declaration function');
}
// 函数表达式
let expressionFunction = function() {
  console.log('I am an expression function');
};
// 箭头函数
let arrowFunction = () => {
  console.log('I am an arrow function');
};
```
- **高阶函数**：`map`、`filter` 和 `reduce` 是常见的高阶函数，它们接受一个函数作为参数。
```javascript
let numbers = [1, 2, 3];
// map 方法
let squared = numbers.map(num => num * num);
console.log(squared); // [1, 4, 9]
// filter 方法
let evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2]
// reduce 方法
let sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 6
```
- **`this` 动态绑定**：可以使用 `call`、`apply` 和 `bind` 方法来显式绑定 `this`。
```javascript
let person = {
  name: 'John',
  sayHello: function() {
    console.log(`Hello, my name is ${this.name}`);
  }
};
let anotherPerson = { name: 'Jane' };
// 使用 call 方法
person.sayHello.call(anotherPerson); // Hello, my name is Jane
// 使用 apply 方法
person.sayHello.apply(anotherPerson); // Hello, my name is Jane
// 使用 bind 方法
let boundFunction = person.sayHello.bind(anotherPerson);
boundFunction(); // Hello, my name is Jane
```

### 二、核心进阶特性

#### 1. 异步编程
- **回调函数**：使用 `setTimeout` 等函数时会使用回调函数，但可能会导致回调地狱。
```javascript
setTimeout(() => {
  console.log('First timeout');
  setTimeout(() => {
    console.log('Second timeout');
    setTimeout(() => {
      console.log('Third timeout');
    }, 1000);
  }, 1000);
}, 1000);
```
- **Promise**：`Promise` 可以实现链式调用，避免回调地狱。
```javascript
function asyncOperation() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Async operation completed');
    }, 1000);
  });
}
asyncOperation()
 .then(result => {
    console.log(result);
    return 'Chained operation';
  })
 .then(result => {
    console.log(result);
  })
 .catch(error => {
    console.error(error);
  });
```
- **Async/Await**：`async/await` 提供了同步化的写法，使异步代码更易读。
```javascript
function asyncOperation() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Async operation completed');
    }, 1000);
  });
}
async function main() {
  try {
    let result = await asyncOperation();
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}
main();
```

#### 2. 闭包与模块化
- **闭包本质**：闭包是函数和其词法环境的组合，可以实现私有变量。
```javascript
function createCounter() {
  let count = 0; // 闭包保护的私有变量
  return () => count++;
}
let counter = createCounter();
console.log(counter()); // 0
console.log(counter()); // 1
```
- **模块化方案**：ES Modules 使用 `import` 和 `export` 来实现模块化。
```javascript
// module.js
export function sayHello() {
  console.log('Hello from module');
}
// main.js
import { sayHello } from './module.js';
sayHello();
```

#### 3. 面向对象与原型
- **构造函数 vs `class` 语法糖**：`class` 是构造函数的语法糖。
```javascript
// 构造函数
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayHello = function() {
    console.log(`Hello, my name is ${this.name}`);
  };
}
let person1 = new Person('John', 30);
person1.sayHello();

// class 语法糖
class PersonClass {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  sayHello() {
    console.log(`Hello, my name is ${this.name}`);
  }
}
let person2 = new PersonClass('Jane', 25);
person2.sayHello();
```
- **原型链继承**：可以使用 `Object.create()` 和 `prototype` 来实现原型链继承。
```javascript
let animal = {
  eat() {
    console.log('Animal is eating');
  }
};
let rabbit = Object.create(animal);
rabbit.eat(); // Animal is eating
```

### 三、浏览器环境 API

#### 1. DOM 操作
- **节点查询**：可以使用 `querySelector` 和 `getElementById` 来查询 DOM 节点。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <div id="myDiv">This is a div</div>
  <script>
    // 使用 getElementById
    let divById = document.getElementById('myDiv');
    console.log(divById.textContent);
    // 使用 querySelector
    let divByQuery = document.querySelector('#myDiv');
    console.log(divByQuery.textContent);
  </script>
</body>
</html>
```
- **事件委托**：事件委托可以高效处理动态元素。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
</head>
<body>
  <ul id="list">
    <li>Item 1</li>
    <li>Item 2</li>
    <li>Item 3</li>
  </ul>
  <script>
    let list = document.getElementById('list');
    list.addEventListener('click', function(event) {
      if (event.target.tagName === 'LI') {
        console.log(`Clicked on ${event.target.textContent}`);
      }
    });
  </script>
</body>
</html>
```

#### 2. BOM 对象
- **`window`**：`window` 是浏览器的全局对象，包含 `location` 和 `history` 等属性。
```javascript
// 获取当前页面的 URL
console.log(window.location.href);
// 前进到历史记录中的下一页
// window.history.forward();
```
- **`fetch`**：`fetch` 是现代网络请求 API，替代了 XMLHttpRequest。
```javascript
fetch('https://jsonplaceholder.typicode.com/todos/1')
 .then(response => response.json())
 .then(data => console.log(data))
 .catch(error => console.error(error));
```

#### 3. 存储机制
- **`localStorage`**：用于持久化存储数据，除非手动删除，否则数据不会过期。
```javascript
// 存储数据
localStorage.setItem('name', 'John');
// 获取数据
let name = localStorage.getItem('name');
console.log(name);
// 删除数据
localStorage.removeItem('name');
```
- **`sessionStorage`**：用于会话级存储，当会话结束（关闭浏览器窗口）时，数据会被清除。
```javascript
// 存储数据
sessionStorage.setItem('message', 'Hello, session!');
// 获取数据
let message = sessionStorage.getItem('message');
console.log(message);
```
- **IndexedDB**：用于结构化数据存储。
```javascript
// 打开数据库
let request = window.indexedDB.open('myDatabase', 1);
request.onsuccess = function(event) {
  let db = event.target.result;
  console.log('Database opened successfully');
};
request.onupgradeneeded = function(event) {
  let db = event.target.result;
  let objectStore = db.createObjectStore('myObjectStore', { keyPath: 'id' });
  objectStore.add({ id: 1, name: 'John' });
};
```

### 四、工程化与最佳实践

#### 1. 代码质量
- **ESLint 规则**：ESLint 可以强制代码风格一致性。
```bash
# 安装 ESLint
npm install eslint --save-dev
# 初始化 ESLint 配置
npx eslint --init
```
- **目录结构规范**：推荐的目录结构如下：
```
src/
├── components/  # 组件化组织
├── utils/       # 工具函数
└── services/    # API服务层
```

#### 2. 安全实践
- **输入消毒**：防止 XSS 攻击，转义 `<script>` 标签。
```javascript
function sanitizeInput(input) {
  return input.replace(/<script>/gi, '&lt;script&gt;').replace(/<\/script>/gi, '&lt;/script&gt;');
}
let userInput = '<script>alert("XSS attack")</script>';
let sanitizedInput = sanitizeInput(userInput);
console.log(sanitizedInput);
```
- **CSP 策略**：限制资源加载源。
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'">
</head>
<body>
  <!-- 页面内容 -->
</body>
</html>
```
- **API 防护**：使用 HTTPS 和 JWT 认证。
```javascript
// 假设这是一个使用 JWT 认证的 API 请求
let token = 'your_jwt_token';
fetch('https://api.example.com/data', {
  headers: {
    'Authorization': `Bearer ${token}`
  }
})
 .then(response => response.json())
 .then(data => console.log(data))
 .catch(error => console.error(error));
```

#### 3. 性能优化
- **防抖/节流**：用于高频事件处理。
```javascript
// 防抖函数
function debounce(func, delay) {
  let timer;
  return function() {
    let context = this;
    let args = arguments;
    clearTimeout(timer);
    timer = setTimeout(() => {
      func.apply(context, args);
    }, delay);
  };
}
// 节流函数
function throttle(func, limit) {
  let inThrottle;
  return function() {
    let context = this;
    let args = arguments;
    if (!inThrottle) {
      func.apply(context, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```
- **虚拟 DOM**：React 和 Vue 的核心优化原理。
```javascript
// 这是一个简化的虚拟 DOM 示例
class VNode {
  constructor(tag, props, children) {
    this.tag = tag;
    this.props = props;
    this.children = children;
  }
}
let vnode = new VNode('div', { id: 'myDiv' }, []);
```
- **代码分割**：使用动态 `import()` 实现代码分割。
```javascript
// 在需要的时候动态加载模块
button.addEventListener('click', async () => {
  const { sayHello } = await import('./module.js');
  sayHello();
});
```

### 五、现代工具链

#### 1. 开发环境
- **编辑器**：推荐使用 VS Code，并安装 ESLint 和 Prettier 插件。
- **调试工具**：可以使用 Chrome DevTools 进行性能分析。

#### 2. 框架生态
- **三大框架**：React、Vue 和 Angular 各有特点。
- **元框架**：Next.js 用于服务器端渲染（SSR），Nuxt.js 用于静态站点生成。

#### 3. 构建工具
- **打包器**：Webpack 和 Vite 是常用的打包器。
```bash
# 安装 Webpack
npm install webpack webpack-cli --save-dev
# 安装 Vite
npm install vite --save-dev
```
- **测试工具**：Jest 用于单元测试，Cypress 用于端到端测试（E2E）。
```bash
# 安装 Jest
npm install jest --save-dev
# 安装 Cypress
npm install cypress --save-dev
```