---
title: 什么是ES6
date: 2020-08-20 11:11:57
tags: js
---

ES6 是新一代的 JS 语言标准，对分 JS 语言核心内容做了升级优化，规范了 JS 使用标准，新增了 JS 原生方法，使得 JS 使用更加规范，更加优雅，更适合大型应用的开发。ES6 泛指下一代JS语言标准，包含 ES2015、ES2016、ES2017、ES2018 等。现阶段在绝大部分场景下，ES2015 默认等同 ES6。ES5 泛指上一代语言标准。ES2015 可以理解为 ES5 和 ES6 的时间分界线。

<!-- more -->

ES6新增的一些特性：

```
1）let声明变量和const声明常量，两个都有块级作用域
ES5中是没有块级作用域的，并且var有变量提升，在let中，使用的变量一定要进行声明

2）箭头函数
ES6中的函数定义不再使用关键字function()，而是利用了()=>来进行定义

3）模板字符串
模板字符串是增强版的字符串，用反引号（`）标识，可以当作普通字符串使用，也可以用来定义多行字符串

4）解构赋值
ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值

5）for of循环
for...of循环可以遍历数组、Set和Map结构、某些类似数组的对象、对象，以及字符串

6）import、export导入导出
ES6标准中，Js原生支持模块(module)。将JS代码分割成不同功能的小块进行模块化，将不同功能的代码分别写在
不同文件中，各模块只需导出公共接口部分，然后通过模块的导入的方式可以在其他地方使用

7）set数据结构
Set数据结构，类似数组。所有的数据都是唯一的，没有重复的值。它本身是一个构造函数

8）... 展开运算符
可以将数组或对象里面的值展开；还可以将多个值收集为一个变量

9）修饰器 @
decorator是一个函数，用来修改类甚至于是方法的行为。修饰器本质就是编译时执行的函数

10）class 类的继承
ES6中不再像ES5一样使用原型链实现继承，而是引入Class这个概念

11）async、await
使用 async/await, 搭配promise,可以通过编写形似同步的代码来处理异步流程, 提高代码的简洁性和可读性
async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成

12）promise
Promise是异步编程的一种解决方案，比传统的解决方案（回调函数和事件）更合理、强大

13）Symbol
Symbol是一种基本类型。Symbol 通过调用symbol函数产生，它接收一个可选的名字参数，该函数返回的symbol是唯一的

14）Proxy代理
使用代理（Proxy）监听对象的操作，然后可以做一些相应事情
```

### let、const、var

var 声明的变量会挂载在 window 上，而 let 和 const 声明的变量不会

var 声明变量存在变量提升，let 和 const 不存在变量提升

```
ES6明确规定，如果区块中存在let命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。
凡是在声明之前就使用这些变量，就会报错。所以在代码块内，使用let命令声明变量之前，该变量都是不可
用的。这在语法上，称为“暂时性死区”
```

let 和 const 声明形成块作用域，var 只有全局作用域和函数作用域概念

同一作用域下 let 和 const 不能声明同名变量，而var可以

const 变量一旦被赋值，就不能再改变

### String的升级优化

**优化部分：**

ES6 新增了字符串模板，在拼接大段字符串时，用反斜杠(``)取代以往的字符串相加的形式，能保留所有空格和换行，使得字符串拼接看起来更加直观，更加优雅。

**升级部分:**

ES6 在 String 原型上新增了 includes() 方法，用于取代传统的只能用 indexOf 查找包含字符的方法 ( indexOf 返回 -1 表示没查到不如includes 方法返回 false 更明确，语义更清晰)

此外还新增了 startsWith(), endsWith(), padStart(),padEnd(),repeat() 等方法，可方便的用于查找，补全字符串。

### Array的升级优化

**优化部分：**

数组解构赋值：ES6 可以直接以 let [a,b,c] = [1,2,3] 形式进行变量赋值，在声明较多变量时，不用再写很多 let(var)，且映射关系清晰，且支持赋默认值。

扩展运算符：ES6 新增的扩展运算符 (...)，可以轻松的实现数组和松散序列的相互转化，可以取代 arguments 对象和 apply 方法，轻松获取未知参数个数情况下的参数集合。

（尤其是在 ES5 中，arguments 并不是一个真正的数组，而是一个类数组的对象，但是扩展运算符的逆运算却可以返回一个真正的数组）。

扩展运算符还可以轻松方便的实现数组的复制和解构赋值（let a = [2,3,4]; let b = [...a]）。

**升级部分:**

ES6 在 Array 原型上新增了 find() 方法，用于取代传统的只能用 indexOf 查找包含数组项目的方法,且修复了 indexOf 查找不到 NaN 的bug([NaN].indexOf(NaN) === -1)

此外还新增了 copyWithin(), includes(), fill(),flat() 等方法，可方便的用于字符串的查找，补全，转换等。

### Number的升级优化

**优化部分：**

ES6 在 Number 原型上新增了 isFinite()，isNaN() 方法，用来取代传统的全局 isFinite()，isNaN() 方法检测数值是否有限、是否是 NaN。

ES5 的 isFinite()，isNaN() 方法都会先将非数值类型的参数转化为 Number 类型再做判断，这其实是不合理的，造成 isNaN('NaN') === true 的奇怪行为--'NaN'是一个字符串，但是isNaN却说这就是NaN。

而 Number.isFinite() 和 Number.isNaN() 则不会有此类问题 (Number.isNaN('NaN') === false)。（isFinite() 同上）

**升级部分:**

ES6 在 Math 对象上新增了 Math.cbrt()，trunc()，hypot() 等等较多的科学计数法运算方法，可以更加全面的进行立方根、求和立方根等等科学计算。

### Object的升级优化

**优化部分：**

1. 对象属性变量式声明：ES6 可以直接以变量形式声明对象属性或者方法，比传统的键值对形式声明更加简洁，更加方便，语义更加清晰。尤其在对象解构赋值或者模块输出变量时，这种写法的好处体现的最为明显。

```js
let [apple, orange] = ['red appe', 'yellow orange'];
let myFruits = {apple, orange};
// let myFruits = {apple: 'red appe', orange: 'yellow orange'};
```

2. 对象的解构赋值：ES6 对象也可以像数组解构赋值那样，进行变量的解构赋值

3. 对象的扩展运算符(...)：ES6 对象的扩展运算符和数组扩展运算符用法本质上差别不大，毕竟数组也就是特殊的对象。对象的扩展运算符一个最常用也最好用的用处就在于可以轻松的取出一个目标对象内部全部或者部分的可遍历属性，从而进行对象的合并和分解。

4. super 关键字：ES6 在 Class 类里新增了类似 this 的关键字 super。同 this 总是指向当前函数所在的对象不同，super 关键字总是指向当前函数所在对象的原型对象。

**升级部分:**

1. ES6 在 Object 原型上新增了 is() 方法，做两个目标对象的相等比较，用来完善`===`方法。`===`方法中`NaN === NaN //false`其实是不合理的，Object.is 修复了这个小 bug。(Object.is(NaN, NaN) // true)

2. ES6 在 Object 原型上新增了 assign() 方法，用于对象新增属性或者多个对象合并。

```js
const target = { a: 1 };
const source1 = { b: 2 };
const source2 = { c: 3 };
Object.assign(target, source1, source2);
target // {a:1, b:2, c:3}
```

3. ES6 在 Object 原型上新增了 getOwnPropertyDescriptors() 方法，此方法增强了 ES5 中 getOwnPropertyDescriptor() 方法，可以获取指定对象所有自身属性的描述对象。结合 defineProperties() 方法，可以完美复制对象，包括复制 get 和 set 属性。

4. ES6 在 Object 原型上新增了 getPrototypeOf() 和 setPrototypeOf() 方法，用来获取或设置当前对象的 prototype 对象。这个方法存在的意义在于，ES5中获取设置 prototype 对像是通过 `__proto__` 属性来实现的，然而 `__proto__` 属性并不是 ES 规范中的明文规定的属性，只是浏览器各大产商“私自”加上去的属性，只不过因为适用范围广而被默认使用了，再非浏览器环境中并不一定就可以使用，所以为了稳妥起见，获取或设置当前对象的 prototype 对象时，都应该采用 ES6 新增的标准用法。

5. ES6 在 Object 原型上还新增了 Object.keys()，Object.values()，Object.entries() 方法，用来获取对象的所有键、所有值和所有键值对数组。

### Function的升级优化

**优化部分：**

1. 箭头函数（核心）：箭头函数是 ES6 核心的升级项之一，箭头函数里没有自己的 this ，这改变了以往 JS 函数中最让人难以理解的 this 运行机制。

   箭头函数内的 this 指向的是函数定义时所在的对象，而不是函数执行时所在的对象。

   ES5 函数里的 this 总是指向函数执行时所在的对象，这使得在很多情况下this的指向变得很难理解，尤其是非严格模式情况下，this 有时候会指向全局对象，这甚至也可以归结为语言层面的bug之一。

   ES6 的箭头函数优化了这一点，它的内部没有自己的 this，这也就导致了 this 总是指向上一层的this，如果上一层还是箭头函数，则继续向上指，直到指向到有自己this的函数为止，并作为自己的 this。

   箭头函数不能用作构造函数，因为它没有自己的 this，无法实例化；

   也是因为箭头函数没有自己的 this，所以箭头函数 内也不存在 arguments 对象。（可以用扩展运算符代替）

2. 函数默认赋值：ES6 之前，函数的形参是无法给默认值得，只能在函数内部通过变通方法实现。ES6 以更简洁更明确的方式进行函数默认赋值。

   ```js
   function es6Fuc (x, y = 'default') {
       console.log(x, y);
   }
   es6Fuc(4) // 4, default
   ```

**升级部分:**

ES6 新增了双冒号运算符，用来取代以往的 bind，call 和 apply。(浏览器暂不支持，Babel 已经支持转码)

```js
foo::bar;
// 等同于
bar.bind(foo);

foo::bar(...arguments);
// 等同于
bar.apply(foo, arguments);
```

### Symbol

Symbol 是 ES6 引入的第七种数据类型。

所有 Symbol() 生成的值都是独一无二的，可以从根本上解决对象属性太多导致属性名冲突覆盖的问题。

对象中 Symbol() 属性不能被 for...in 遍历，但是也不是私有属性。

### Set

Set 是 ES6 引入的一种类似 Array 的新的数据结构，Set 实例的成员类似于数组 item 成员，

区别是 Set 实例的成员都是唯一，不重复的。这个特性可以轻松地实现数组去重。

### Map

Map 是 ES6 引入的一种类似 Object 的新的数据结构。

Map 可以理解为是 Object 的超集，打破了以传统键值对形式定义对象，对象的 key 不再局限于字符串，也可以是 Object。可以更加全面的描述对象的属性。

### Proxy

Proxy 是 ES6 新增的一个构造函数，可以理解为 JS 语言的一个代理，用来改变 JS 默认的一些语言行为，包括拦截默认的 get/set 等底层方法，使得 JS 的使用自由度更高，可以最大限度的满足开发者的需求。

比如通过拦截对象的 get/set 方法，可以轻松地定制自己想要的 key 或者 value。

### Promise

Promise 是 ES6 引入的一个新的对象，他的主要作用是用来解决 JS 异步机制里，回调机制产生的“回调地狱”。

它并不是什么突破性的 API，只是封装了异步回调形式，使得异步回调可以写的更加优雅，可读性更高，而且可以链式调用。

### Class

ES6 的 class 可以看作只是一个 ES5 生成实例对象的构造函数的语法糖。

它参考了 java 语言，定义了一个类的概念，让对象原型写法更加清晰，对象实例化更像是一种面向对象编程。Class 类可以通过 extends 实现继承。

它和 ES5 构造函数的不同点：

类的内部定义的所有方法，都是不可枚举的：

```js
///ES5
function ES5Fun (x, y) {
  this.x = x;
  this.y = y;
}
ES5Fun.prototype.toString = function () {
   return '(' + this.x + ', ' + this.y + ')';
}
var p = new ES5Fun(1, 3);
p.toString();
Object.keys(ES5Fun.prototype); //['toString']

//ES6
class ES6Fun {
  constructor (x, y) {
    this.x = x;
    this.y = y;
  }
  toString () {
    return '(' + this.x + ', ' + this.y + ')';
  }
}

Object.keys(ES6Fun.prototype); //[]
```

ES6 的 class 类必须用 new 命令操作，而 ES5 的构造函数不用 new 也可以执行；

ES6 的 class 类不存在变量提升，必须先定义 class 之后才能实例化，不像 ES5 中可以将构造函数写在实例化之后；

ES5 的继承，实质是先创造子类的实例对象 this，然后再将父类的方法添加到 this 上面。

ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到 this 上面（所以必须先调用 super 方法），然后再用子类的构造函数修改 this。

### module、export、import

module、export、import 是 ES6 用来统一前端模块化方案的设计思路和实现方案。

export、import 的出现统一了前端模块化的实现方案，整合规范了浏览器/服务端的模块化方法，

之后用来取代传统的 AMD/CMD、requireJS、seaJS、commondJS 等等一系列前端模块不同的实现方案，使前端模块化更加统一规范，JS也能更加能实现大型的应用程序开发。

import 引入的模块是静态加载（编译阶段加载）而不是动态加载（运行时加载）。

import 引入 export 导出的接口值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。