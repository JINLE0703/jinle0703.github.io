---
title: js设计模式
date: 2020-10-04 22:08:45
tags:
---

<!-- more -->

## 工厂模式

```js
class Product {
    constructor(name) {
        this.name = name;
    }
    init() {
        alert('init');
    }
    fn1() {
        alert('fn1');
    }
}

class Creator {
    create(name) {
        return new Product(name);
    }
}

let createor = new Creator();
let p = createor.create('zhangsan');
p.init();
p.fn1();
```

#### 场景

jQuery - $('div')、React.createElement、vue异步组件

```js
class jQuery {
	...
}
window.$ = function(selector) {
	return new jQuery(selector);
}
```

## 单例模式

一个类只有一个实例，JS无法完全实现

```js
class SingleObject {
    login() {
        console.log('login...')
    }
}
SingleObject.getInstance = (function () {
    let instance
    return function () {
        if (!instance) {
            instance = new SingleObject();
        }
        return instance
    }
})()

// 测试
let obj1 = SingleObject.getInstance()
obj1.login()
let obj2 = SingleObject.getInstance()
obj2.login()
console.log(obj1 === obj2)
```

#### 场景

jQuery 只有一个 $、模拟登陆框

## 适配器模式

将旧接口和使用者进行分离

```js
class Adaptee {
    specificRequest() {
        return '德国标准插头'
    }
}
class Target {
    constructor() {
        this.adaptee = new Adaptee()
    }
    request() {
        let info = this.specificRequest()
        return `${info} - 转换器 - 中国标准插头`
    }
}

// test
let target = new Target()
let res = target.request()
```

#### 场景

封装旧接口、vue computed

## 装饰器模式

为对象添加新功能，不改变其原有结构和功能

```js
class Circle {
    draw() {
        console.log('draw')
    }
}
class Decorater {
    constructor(circle) {
        this.circle = circle
    }
    draw() {
        this.circle.draw()
        this.setRedBorder(circle)
    }
    setRedBorder(circle) {
        console.log('setRedBorder')
    }
}

// test
let circle = new Circle()
circle.draw()   // draw

let dec = new Decorater(circle)
dec.draw()  // draw setRedBorder
```

#### 场景

ES7装饰器、mixin

```js
function readonly(target, name, descriptor){
  descriptor.writable = false;
  return descriptor;
}

class Person {
    constructor() {
        this.first = 'A'
        this.last = 'B'
    }

    @readonly
    name() { return `${this.first} ${this.last}` }
}

var p = new Person()
console.log(p.name())
p.name = function () {} // 这里会报错，因为 name 是只读属性
```

## 代理模式

代理类和目标类分离，隔离开目标类和使用者

```js
class RealImg {
    constructor(fileName) {
        this.fileName = fileName;
        this.loadFromDisk();    // 初始化
    }
    display() {
        console.log('display...');
    }
    loadFromDisk() {
        console.log('loading...');
    }
}

class ProxyImg {
    constructor(fileName) {
        this.realImg = new RealImg(fileName);
    }
    display() {
        this.realImg.display();
    }
}

// test
const p = new ProxyImg('1.png');
p.display();
```

#### 场景

网页事件代理、jQuery $.proxy、ES6 Proxy

```js
// 明星
let star = {
    name: '张XX',
    age: 25,
    phone: '13910733521'
}

// 经纪人
let agent = new Proxy(star, {
    get: function (target, key) {
        if (key === 'phone') {
            // 返回经纪人自己的手机号
            return '18611112222'
        }
        if (key === 'price') {
            // 明星不报价，经纪人报价
            return 120000
        }
        return target[key]
    },
    set: function (target, key, val) {
        if (key === 'customPrice') {
            if (val < 100000) {
                // 最低 10w
                throw new Error('价格太低')
            } else {
                target[key] = val
                return true
            }
        }
    }
})
```

## 观察者模式

主题和观察者分离，不是主动触发而是被动监听

```js
// 主题，接收状态变化，触发每个观察者
class Subject {
    constructor() {
        this.state = 0
        this.observers = []
    }
    getState() {
        return this.state
    }
    setState(state) {
        this.state = state
        this.notifyAllObservers()
    }
    attach(observer) {
        this.observers.push(observer)
    }
    notifyAllObservers() {
        this.observers.forEach(observer => {
            observer.update()
        })
    }
}

// 观察者，等待被触发
class Observer {
    constructor(name, subject) {
        this.name = name
        this.subject = subject
        this.subject.attach(this)
    }
    update() {
        console.log(`${this.name} update, state: ${this.subject.getState()}`)
    }
}
```

#### 场景

网页事件绑定、Promise、jQuery callbacks、nodejs 自定义事件

## 迭代器模式

顺序遍历有序集合

```js
class Iterator {
    constructor(conatiner) {
        this.list = conatiner.list
        this.index = 0
    }
    next() {
        if (this.hasNext()) {
            return this.list[this.index++]
        }
        return null
    }
    hasNext() {
        if (this.index >= this.list.length) {
            return false
        }
        return true
    }
}

class Container {
    constructor(list) {
        this.list = list
    }
    getIterator() {
        return new Iterator(this)
    }
}

// 测试代码
let container = new Container([1, 2, 3, 4, 5])
let iterator = container.getIterator()
while(iterator.hasNext()) {
    console.log(iterator.next())
}
```

####  场景

jQuery each、ES6 Iterator(for...of...)

## 状态模式

```js
class State {
    constructor(color) {
        this.color = color
    }
    handle(context) {
        console.log(`turn to ${this.color} light`)
        context.setState(this)
    }
}

class Context {
    constructor() {
        this.state = null
    }
    setState(state) {
        this.state = state
    }
    getState() {
        return this.state
    }
}

// 测试代码
let context = new Context()

let green = new State('greed')
let yellow = new State('yellow')
let red = new State('red')

// 绿灯亮了
green.handle(context)
console.log(context.getState())
// 黄灯亮了
yellow.handle(context)
console.log(context.getState())
// 红灯亮了
red.handle(context)
console.log(context.getState())
```

#### 场景

有限状态机、写一个简单的 Promise