# ECMAScript 2020 标准

# ECMAScript 2020 新增规范
> ECMAScript 在2020年新增的一些语法规范，方便我们的日常开发，并且也对一些语法上的Bug做出了一些修复

## [动态 import() (Dynamic import())](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/import)
> 动态的import()使用方式，不需要使用import ... from ... 的方式来导入一个文件，可以直接 import(...)来来导入一个文件或是一个包，这个特性出现之前可以通过安装一个[插件-babel-plugin-syntax-dynamic-import](https://babeljs.io/docs/en/babel-plugin-syntax-dynamic-import)就能够实现了。
> 在React中使用 React.lazy，就需要使用动态 import() 的方式导入文件。

```javascript
import('./index.js')  // 这种导入，返回的是一个Promise对象，我们可以使用Promise的一个方法进行操作

async function func() {
  const { NUMBER } = await import('./module.js');

  console.log(NUMBER); // 10
}

func();

```

## [**空值合并运算符 (**Nullish coalescing)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
> 在我们之前的使用 ||, && 逻辑运算符的时候，会出现0, ""为假值(false)的情况，但是 ?? 运算符就解决了这种情况，他不会吧0, ""当作假值来进行处理

```javascript
0 ?? 10 // 0
null ?? 10 // 10
undefined ?? 10 // 10
"" ?? 10 // ""

0 || 10 // 10
"" || 10 // 10
```

## [可选链操作符 (Optional chaining)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/%E5%8F%AF%E9%80%89%E9%93%BE)
> 在访问对象的属性时，访问的这个属性通常可能会是不存在的，所以我们需要判断这个属性是否存在，通常都是使用运算符进行判断，但是可选链操作符就相对来说简单一点

**可选链操作符在赋值操作时是会失效的**
```javascript
let obj = {
	name: {
		height: 70
	},
};

// 常规的判断
let age = obj.name && obj.name.age

// 使用可选两次操作符
let age = obj.name?.age
```

## [BigInt](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
> 在几乎所有的编程语言中，在处理大整型数时都会出现精度缺失的情况，在ES中也是这样的，一旦操作的整型数值操作了 2-1 的话就会出现计算错误的情况。使用 BigInt就能够解决这一情况

**在使用BigInt来计算整型时，要在数值的后面加上 n 来进行使用** 

```javascript
let num = 9007199254740991;
num + 10 // 9007199254741000

let num = 9007199254741000n
num + 10n // 9007199254741010n

let num = BigInt(9007199254741010);
num + 10n // 9007199254741020n

let num = BigInt("9007199254741010");
num + 10m // 9007199254741020n

// 使用类型判断时候
Object.prototype.toString.call(BigInt("9007199254741010")) // "[object BigInt]"
```

## [String.matchAll](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/matchAll)
> 该方法可以说是 String.match 的进阶版， match 返回的是一个匹配的数组，而matchAll返回的是一个迭代器对象，可以使用 next() 方法进行控制

```javascript
let str = "#JavaScript is full of #surprises. Both good and bad ones #TIL";
let next = str.matchAll(/#\(w)+/g);
next.next();

```

## [Promise.allSettled](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled)
> 该`**Promise.allSettled()**`方法返回一个在所有给定的promise已被决议或被拒绝后决议的promise，并带有一个对象数组，每个对象表示对应的promise结果。其操作方式有点类似于 Promise.all()，但是传入 all() 方法中的其中一个Promise执行错误就会返回错误。有点类似 Array.every()

```javascript
const promise1 = Promise.resolve(3);
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 100, 'foo'));
const promises = [promise1, promise2];

Promise.allSettled(promises).
  then((results) => results.forEach((result) => console.log(result.status)));

// expected output:
// "fulfilled"
// "rejected"
```

## globalThis
> 在不同的JavaScript运行环境中，获取全局的this对象都是不一样的，浏览器环境是 window，Nodejs环境是global等等，使用globalThis你就能够在不同的运行环境下获取到该运行环境的全部this对象了

```javascript

var getGlobal = function () { 
  if (typeof self !== 'undefined') { return self; } 
  if (typeof window !== 'undefined') { return window; } 
  if (typeof global !== 'undefined') { return global; } 
  throw new Error('unable to locate global object'); 
}; 


// 浏览器
globalThis === window  // true

// Nodejs
globalThis === global  // true

// 
globalThis === self    // true
```

## import.meta
> 这个属性能够获取到 script 中 url 属性，我们可以在 src 中添加一些参数，进行全局使用

```html
<script type="module" src="./index?name=xhh&id=10"></script>
```

```js
const { url } = import.meta;
```
