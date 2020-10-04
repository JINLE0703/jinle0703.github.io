---
title: 聊聊JavaScript类型
date: 2020-08-19 17:04:21
tags: js
---

JavaScript语言的每一个值都属于某一种数据类型。JavaScript语言规定了7种语言类型。语言类型广泛用于变量、函数参数、表达式、函数返回值等场合。根据最新的语言标准，这7种语言类型是：

<!-- more -->

1. Undefined；
2. Null；
3. Boolean；
4. String；
5. Number；
6. Symbol（ES6）；
7. Object；

### Undefined、Null

Undefined 类型表示未定义，它的类型只有一个值，就是 undefined。任何变量在赋值前是 Undefined 类型、值为 undefined，一般我们可以用全局变量 undefined（就是名为 undefined 的这个变量）来表达这个值，或者 void 运算来把任一一个表达式变成 undefined 值。

但是呢，因为 JavaScript 的代码 undefined 是一个变量，而并非是一个关键字，这是 JavaScript 语言公认的设计失误之一，所以为了避免无意中被篡改，建议使用 void 0 来获取 undefined 值。

Undefined 跟 null 有一定的表意差别，null表示的是：“定义了但是为空”。所以，在实际编程时，一般不会把变量赋值为  undefined，这样可以保证所有值为 undefined 的变量，都是从未赋值的自然状态。

Null 类型也只有一个值，就是 null，它的语义表示空值，与 undefined 不同，null 是 JavaScript 关键字，所以在任何代码中，都可以放心用 null 关键字来获取 null 值。

### Boolean

Boolean 类型有两个值， true 和 false，它用于表示逻辑意义上的真和假，同样有关键字 true 和 false 来表示两个值。

### String

String 用于表示文本数据。String 有最大长度是 2^53 - 1，这在一般开发中都是够用的。

因为String 的意义并非“字符串”，而是字符串的 UTF16 编码，字符串的操作 charAt、charCodeAt、length 等方法针对的都是 UTF16 编码。所以，字符串的最大长度，实际上是受字符串的编码长度影响的。

> 现行的字符集国际标准，字符是以 Unicode 的方式表示的，每一个 Unicode 的码点表示一个字符，理论上，Unicode 的范围是无限的。UTF是Unicode的编码方式，规定了码点在计算机中的表示方法，常见的有 UTF16 和 UTF8。 Unicode 的码点通常用 U+??? 来表示，其中 ??? 是十六进制的码点值。 0-65536（U+0000 - U+FFFF）的码点被称为基本字符区域（BMP）。

JavaScript 中的字符串是永远无法变更的，一旦字符串构造出来，无法用任何方式改变字符串的内容，所以字符串具有值类型的特征。

### Number

Number 类型表示我们通常意义上的“数字”。这个数字大致对应数学中的有理数，当然，在计算机中有一定的精度限制。

JavaScript 中的 Number 类型有 18437736874454810627 （即 2^64-2^53+3 ) 个值。

JavaScript 中的 Number 类型基本符合 IEEE 754-2008 规定的双精度浮点数规则，但是 JavaScript 为了表达几个额外的语言场景（比如不让除以0出错，而引入了无穷大的概念），规定了几个例外情况：

- NaN，非数字，占用了 9007199254740990，这原本是符合IEEE规则的数字；
- Infinity，无穷大；
- -Infinity，负无穷大。

另外，值得注意的是，JavaScript中有 +0 和 -0，在加法类运算中它们没有区别，但是除法的场合则需要特别留意区分，“忘记检测除以 -0，而得到负无穷大”的情况经常会导致错误，而区分 +0 和 -0 的方式，正是检测 1/x 是 Infinity 还是 -Infinity。

根据双精度浮点数的定义，Number 类型中有效的整数范围是 -0x1fffffffffffff 至 0x1fffffffffffff，所以 Number 无法精确表示此范围外的整数。

同样根据浮点数的定义，非整数的Number类型无法用 == （ ===也不行） 来比较

```js
console.log( 0.1 + 0.2 == 0.3); // false
```

正确的比较方法是使用JavaScript提供的最小精度值，检查等式左右两边差的绝对值是否小于最小精度，才是正确的比较浮点数的方法。

```js
console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON); // true
```

### Symbol

Symbol 是 ES6 中引入的新类型，它是一切非字符串的对象 key 的集合，在ES6 规范中，整个对象系统被用 Symbol 重塑。

Symbol 可以具有字符串类型的描述，但是即使描述相同，Symbol也不相等。

我们创建 Symbol 的方式是使用全局的 Symbol 函数。例如：

```js
var mySymbol = Symbol("my symbol");
```

### Object

Object 是 JavaScript 中最复杂的类型，也是 JavaScript 的核心机制之一。Object表示对象的意思，它是一切有形和无形物体的总称。

在 JavaScript 中，对象的定义是“属性的集合”。属性分为数据属性和访问器属性，二者都是 key - value 结构，key 可以是字符串或者  Symbol 类型。

提到对象，我们必须要提到一个概念：类。JavaScript 中的“类”仅仅是运行时对象的一个私有属性，而 JavaScript 中是无法自定义类型的。JavaScript 中的几个基本类型，都在对象类型中有一个“亲戚”。它们是：

- Number；
- String；
- Boolean；
- Symbol；

**所以，我们必须认识到 3 与 new Number(3) 是完全不同的值，它们一个是 Number 类型， 一个是对象类型。**

Number、String 和 Boolean，三个构造器是两用的，当跟 new 搭配时，它们产生对象，当直接调用时，它们表示强制类型转换。

Symbol 函数比较特殊，直接用 new 调用它会抛出错误，但它仍然是 Symbol 对象的构造器。

## 类型判断

JavaScript 中类型判断一共有四种方法

### typeof

 typeof 是一个操作符，其右侧跟一个一元表达式，并返回这个表达式的数据类型。返回的结果用该类型的字符串(全小写字母)形式表示，包括 number，string，boolean，undefined，object，function，symbol等。

<% asset_img typeof对照表.png image %>

### instanceof

instanceof 用来判断A是否为B的实例，表达式为：A instanceof B，如果 A 是 B 的实例，则返回 true，否则返回 false。instanceof 检测的是原型，内部机制是通过判断对象的原型链中是否有类型的原型。

```js
{} instanceof Object; //true
[] instanceof Array;  //true
[] instanceof Object; //true
"123" instanceof String; //false
new String(123) instanceof String; //true
```

### constructor

当一个函数F被定义时，JS 引擎会为 F 添加 prototype 原型，然后在 prototype 上添加一个 constructor 属性，并让其指向F的引用，F 利用原型对象的 constructor 属性引用了自身，当F作为构造函数创建对象时，原型上的 constructor 属性被遗传到了新创建的对象上，从原型链角度讲，构造函数 F 就是新对象的类型。这样做的意义是，让对象诞生以后，就具有可追溯的数据类型。

```js
''.constructor === String; // true
new Number(1).constructor === Number; // true
```

### Object.prototype.toString()

toString() 是 Object 的原型方法，调用该方法，默认返回当前对象的 [[Class]]。这是一个内部属性，其格式为 [object Xxx] ，其中 Xxx 就是对象的类型。 对于 Object 对象，直接调用 toString() 就能返回 [object Object] ，而对于其他对象，则需要通过 call、apply 来调用才能返回正确的类型信息。

```
Object.prototype.toString.call(''); // [object String]
Object.prototype.toString.call(1); // [object Number]
```

## 类型转换

因为JS是弱类型语言，所以类型转换发生非常频繁，大部分我们熟悉的运算都会先进行类型转换。

其中最为臭名昭著的是JS中的“ == ”运算，因为试图实现跨类型的比较，它的规则复杂到几乎没人可以记住，它属于设计失误，并非语言中有价值的部分，很多实践中推荐禁止使用 == ，而要求程序员进行显式地类型转换后，用 === 比较。

<% asset_img 类型转换规则表.jpg image %>

### String To Number

需要注意的是，在不传入第二个参数的情况下，parseInt 只支持16进制前缀“0x”，而且会忽略非数字字符，也不支持科学计数法。而parseFloat则直接把原字符串作为十进制来解析，它不会引入任何的其他进制。

**多数情况下，Number 是比 parseInt 和 parseFloat 更好的选择。**

### Number To String

在较小的范围内，数字到字符串的转换是完全符合你直觉的十进制表示。当Number绝对值较大或者较小时，字符串表示则是使用科学计数法表示的。这个算法细节繁多，我们从感性的角度认识，它其实就是保证了产生的字符串不会过长。

### 装箱转换

每一种基本类型 Number、String、Boolean、Symbol 在对象中都有对应的类，所谓装箱转换，正是把基本类型转换为对应的对象，它是类型转换中一种相当重要的种类。

前文提到，全局的 Symbol 函数无法使用 new 来调用，但我们仍可以利用装箱机制来得到一个 Symbol 对象，我们可以利用一个函数的call方法来强迫产生装箱。

```js
var symbolObject = Object(Symbol("a"));

console.log(typeof symbolObject); // object
console.log(symbolObject instanceof Symbol); // true
console.log(symbolObject.constructor == Symbol); // true
```

**装箱机制会频繁产生临时对象，在一些对性能要求较高的场景下，我们应该尽量避免对基本类型做装箱转换。**

每一类装箱对象皆有私有的 Class 属性，这些属性可以用 **Object.prototype.toString** 获取

```js
var symbolObject = Object(Symbol("a"));

console.log(Object.prototype.toString.call(symbolObject)); // [object Symbol]
```

**在 JavaScript 中，没有任何方法可以更改私有的 Class 属性，因此 Object.prototype.toString 是可以准确识别对象对应的基本类型的方法，它比 instanceof 更加准确。**

### 拆箱转换

在 JavaScript 标准中，规定了 **ToPrimitive** 函数，它是对象类型到基本类型的转换（即，拆箱转换）。

------

除了这七种语言类型，还有一些语言的实现者更关心的规范类型。

- List 和 Record： 用于描述函数传参过程。
- Set：主要用于解释字符集等。
- Completion Record：用于描述异常、跳出等语句执行过程。
- Reference：用于描述对象属性访问、delete等。
- Property Descriptor：用于描述对象的属性。
- Lexical Environment 和 Environment Record：用于描述变量和作用域。
- Data Block：用于描述二进制数据。