---
title: express和koa2中间件原理
date: 2020-10-22 00:47:51
tags: nodejs
---

koa2 的中间件模式与 express 的是不一样的，koa 是洋葱型，express 是直线型，其原因可能是 koa2 支持 async/await 导致的，也致 koa2 的代码会比 express 简洁许多

<!-- more -->

## express中间件原理

app.use 用来注册中间件，先收集起来

遇到 http 请求，根据 path 和 method 判断触发条件

实现next 机制，即上一个通过 next 触发下一个

```js
const http = require('http');
const slice = Array.prototype.slice;

class LikeExpress {
    constructor() {
        // 存放中间件的列表
        this.routes = {
            all: [],
            get: [],
            post: []
        }
    }

    // 注册中间件通用方法
    register(path) {
        const info = {};
        if (typeof path === 'string') {
            info.path = path;
            info.stack = slice.call(arguments, 1);
        } else {
            info.path = '/';
            info.stack = slice.call(arguments, 0);
        }
        return info;
    }

    use() {
        const info = this.register(...arguments);
        this.routes.all.push(info);
    }

    get() {
        const info = this.register(...arguments);
        this.routes.get.push(info);
    }

    post() {
        const info = this.register(...arguments);
        this.routes.post.push(info);
    }

    match(method, url) {
        let stack = [];
        if (url === '/favicon.ico') {
            return stack;
        }

        // 获取 routes
        let curRoutes = [];
        curRoutes = curRoutes.concat(this.routes.all);
        curRoutes = curRoutes.concat(this.routes[method]);

        curRoutes.forEach(routeInfo => {
            if (url.indexOf(routeInfo.path) === 0) {
                stack = stack.concat(routeInfo.stack);
            }
        });
    }

    // 核心 next 机制
    handle(req, res, stack) {
        const next = () => {
            // 拿第一个中间件
            const middleware = stack.shift();
            if (middleware) {
                // 执行中间件函数
                middleware(req, res, next);
            }
        }
        next();
    }

    callback() {
        return (req, res) => {
            res.json = (data) => {
                res.setHeader('Content-type', 'application/json');
                res.end(
                    JSON.stringify(data)
                );
            }
            const url = req.url;
            const method = req.method.toLowerCase();

            const resultList = this.match(method, url);
            this.handle(req, res, resultList);
        }
    }

    listen(...args) {
        const server = http.createServer(this.callback());
        server.listen(...args);
    }
}

// 工厂函数
module.exports = () => {
    return new LikeExpress()
}
```

## koa2中间件原理

洋葱圈模型

```js
const http = require('http');

// 组合中间件
function compose(middlewareList) {
    return function (ctx) {
        function dispatch(i) {
            const fn = middlewareList[i];
            try {
                return Promise.resolve(
                    fn(ctx, dispatch.bind(null, i + 1))
                );
            } catch (err) {
                return Promise.reject(err);
            }
        }
        return dispatch(0);
    }
}

class LikeKoa2 {
    constructor() {
        this.middlewareList = [];
    }

    use(fn) {
        this.middlewareList.push(fn);
        return this;    // 链式调用
    }

    createContext(req, res) {
        const ctx = {
            req,
            res
        }
        return ctx;
    }

    handleRequest(ctx, fn) {
        return fn(ctx);
    }

    callback() {
        const fn = compose(this.middlewareList);
        return (req, res) => {
            const ctx = this.createContext(req, res);
            return this.handleRequest(ctx, fn);
        }
    }

    listen(...args) {
        const server = http.createServer(this.callback());
        server.listen(...args);
    }
}

module.exports = LikeKoa2
```

