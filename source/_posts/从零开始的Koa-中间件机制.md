---
title: '从零开始的Koa: 中间件机制'
date: 2021-04-16 12:38:35
tags: 技术
index_img: https://z3.ax1x.com/2021/04/16/cRfHDe.png
---
[Koa源码](https://github.com/koajs/koa)

Koa 是一个 web 框架，由 Express 幕后的原班人马打造， 致力于成为 web 应用和 API 开发领域中的一个更小、更富有表现力、更健壮的基石。 通过利用 async 函数，Koa 帮你丢弃回调函数，并有力地增强错误处理。 Koa 并没有捆绑任何中间件， 而是提供了一套优雅的方法，帮助您快速而愉快地编写服务端应用程序。

和Express相比，Koa没有内置任何中间件，甚至连路由、日志都没有。这些能力需要开发者自己编码或使用第三方包。Koa只是提供了一套级联中间件的方式(koa-compose)。因此，Koa是一个极小的框架，内置的概念只有5个：

- Application：应用
- Middleware：中间件
- Context：上下文
- Request：请求
- Response：响应

Koa总共的代码还没过2000行，非常符合“小而美”的定义

那么下面请欣赏《如何用8行代码起一个简单的Koa服务》：

```
const Koa = require('koa');
const app = new Koa();

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);Copy
```

起好了，然后呢？他只是个会返回Hello world的小笨蛋而已，我现在想让它干点别的事情。

### 从简单的中间件讲起

Koa 中间件是一个很核心的概念，但是它的用法很简单，直接app.use()：

```
const Koa = require('koa');
const app = new Koa();

app.use(async (ctx, next) => {
  console.log(1);
  await next();
  console.log(2);
});

app.use(async (ctx, next) => {
  console.log(3);
  await next();
  console.log(4);
});

app.use(async (ctx, next) => {
  console.log(5);
  await next();
  console.log(6);
});

app.use(async ctx => {
  ctx.body = 'Hello World';
});

app.listen(3000);Copy
```

返回的结果还是Helloworld，而打印结果会像这样：

```
// 1
// 3
// 5
// 6
// 4
// 2Copy
```

上述中间件执行顺序如下图所示：

[![cRfifx.png](https://z3.ax1x.com/2021/04/16/cRfifx.png)](https://z3.ax1x.com/2021/04/16/cRfifx.png)

Koa的实例app是这样通过 app.use()方法为应用添加中间件的

```
  constructor() {
   // ...
   this.middleware = []
   // ...
 }
  
 use(fn) {
   // 检查fn
   if (typeof fn !== 'function') throw new TypeError('middleware must be a function!');
   // ...
   this.middleware.push(fn);
   return this;
}Copy
```

我们可以看出，调用app.use()方法后，应用会依次将中间件放入应用的属性middleware数组里，那么在哪里用到了middleware呢？答案就在最后一行的app.listen()

### 深入探索app.listen()做了什么

遇事不决，直接读源代码

```
// Application.js  
  listen(...args) {
    // ...
    const server = http.createServer(this.callback());
    return server.listen(...args);
  }Copy
```

[![cRfkp6.png](https://z3.ax1x.com/2021/04/16/cRfkp6.png)](https://z3.ax1x.com/2021/04/16/cRfkp6.png)

出现了！所有Node服务端框架用到的核心模块：http

而http.createServer()的参数，也就是this.callback()的返回值，一定是一个函数，它长这个样子：

```
(req, res) => {
  // Do something
}Copy
```

那我们循着这个逻辑看源码里的callback函数

```
callback() {
  const fn = compose(this.middleware);
  // ...
  const handleRequest = (req, res) => {
    const ctx = this.createContext(req, res);
    return this.handleRequest(ctx, fn);
  };
  return handleRequest;
}Copy
```

callback()函数做了两件事：

1. 用compose函数处理middleware数组
2. 返回handleRequest给http.createServer作为参数，这样每收到一个请求，其实内部就会执行一次this.handleRequest

#### 先说说handleRequest

```
  handleRequest(ctx, fnMiddleware) {
    const res = ctx.res;
    res.statusCode = 404;
    const onerror = err => ctx.onerror(err);
    const handleResponse = () => respond(ctx);
    onFinished(res, onerror);
    return fnMiddleware(ctx).then(handleResponse).catch(onerror);
}Copy
```

这段代码做了三件事情：

- 错误处理：新建onerror函数
- onFinished监听response执行完成，来做一些资源清理工作
- 执行传入的fnMiddleware
  fnMiddleware就是compose处理中间件数组返回的结果，根据后面的then可以知道，他是一个返回Promise对象的函数。在resolve后会开始执行respond逻辑。

#### 【核心】compose函数对middleware做的处理

先把compose的核心逻辑贴在下边，因为比较冗长难懂，接下来将会拆开讲

```
// 可以不看🙈
function compose (middleware) {
  if (!Array.isArray(middleware)) throw new TypeError('Middleware stack must be an array!')
  for (const fn of middleware) {
    if (typeof fn !== 'function') throw new TypeError('Middleware must be composed of functions!')
  }
  
 return function (context, next) {
    let index = -1
    return dispatch(0)
    function dispatch (i) {
      if (i <= index) return Promise.reject(new Error('next() called multiple times'))
      index = i
      let fn = middleware[i]
      if (i === middleware.length) fn = next
      if (!fn) return Promise.resolve()
      try {
        return Promise.resolve(fn(context, dispatch.bind(null, i + 1)));
      } catch (err) {
        return Promise.reject(err)
      }
    }
  }
}Copy
```

乍一看还是混乱的，让我们简化一下逻辑：

```
return function (context, next) {
  // last called middleware #
  let index = -1
  return dispatch(0)
  // i表示预期想要执行哪个中间件
  function dispatch (i) {
  // 先省略      
  }
}Copy
```

刚刚提到的fnMiddleware其实就是这个函数，每一次请求都会执行这里。它的参数context是上下文对象，next表示所有中间件走完之后，最后执行的一个函数。函数里标识了一个索引index， ——用于标识「上一次执行到了哪个中间件」

有了这几个概念之后，接下来就仔细看dispatch的实现，其中，i是即将执行中间件的索引

```
function dispatch (i) {     
   // 校验预期执行的中间件（索引i），其索引是否在已经执行的中间件（索引index）之后
   if (i <= index) return Promise.reject(new Error('next() called multiple times'))     
   // 校验通过，状态改变，预期执行 => 已经执行
   index = i    
   // 取预期执行的中间件函数
   let fn = middleware[i]      
   // 校验预期执行的中间件索引是否已经越界，是则说明中间件已经全部执行完毕
   if (i === middleware.length) fn = next // 越界则使用闭包的next   
   // 校验fn，如果fn是undefined就直接返回一个reolved的Promise对象
   if (!fn) return Promise.resolve()
   try {
     // 递归调用，对中间件的执行结果包裹一层Promise.resolve
     return Promise.resolve(fn(context, dispatch.bind(null, i + 1)));
   } catch (err) {
     return Promise.reject(err)
   }
 }Copy
```

有些晦涩，所以我们结合上文的🌰来模拟一下一次请求的处理过程

```
app.use(function one(ctx, next) {
  console.log(1);
  next();
  console.log(2);
});

app.use(function two(ctx, next){
  console.log(3);
  next();
  console.log(4);
});

app.use(function three(ctx, next){
  console.log(5);
  next();
  console.log(6);
});Copy
```

刚才说到，首先执行的是dipatch(0)，索引为0的是即将执行的中间件，此时index为-1，我们可以顺利的走过第一行代码的验证，并将index更新，以供后续逻辑使用

```
if (i <= index) return Promise.reject(new Error('next() called multiple times'))
index = iCopy
```

然后用fn变量来保存「即将执行」的中间件，fn是middleware[0]

```
let fn = middleware[i] // function one
// 下面的两个判断都通过了
if (i === middleware.length) fn = next
if (!fn) return Promise.resolve()Copy
```

接下来的逻辑：

```
// 原代码
try {
   // 对中间件的执行结果包裹一层Promise.resolve
  return Promise.resolve(fn(context, dispatch.bind(null, i + 1)));
} catch (err) {
  return Promise.reject(err)
}

// 细分成三行
try {
  const next = dispatch.bind(null, i + 1); // next为函数dispatch(1)
  const fnResult = fn(context, next); // 相当于 fnResult = one(context, dispath(1))
  return Promise.resolve(fnResult);
} catch (err) {
  return Promise.reject(err)
}Copy
```

可以看到这段代码做了三件事：

1. 定义了next函数，且绑定了执行上下文和第一个参数为i+1，下一个执行的函数就是它
2. 执行fn函数，在i为0的情况下，即第一个中间件
3. 三是对第一个中间件执行的结果进行了Promise包装，确保返回值是Promise对象，并完成了错误的处理。

第一个中间件长这个样子：

```
function one(ctx, next) => {
  console.log('1');
  next();
  console.log('2');
}Copy
```

执行`one(context, dispatch(1))`就很好分析了，先`console.log(1)`，然后执行next，这一步相当于执行dispatch(1)，而dispath(1)又重新走了之前的逻辑。这样形成了递归调用，每个中间件函数所传入的next都封装了“要执行的下一个中间件”。

然后执行`dispatch(1)`，实际执行了中间件two，`console.log(3)`，next调用了中间件three···
如果我们打几个断点，可以看出来实际调用的函数堆栈和上文分析的相符合：

[![cRfN7j.png](https://z3.ax1x.com/2021/04/16/cRfN7j.png)](https://z3.ax1x.com/2021/04/16/cRfN7j.png)

到了这里，下一个该执行的函数就是dispatch(3)了，这个时候

```
index = i // index = 3
let fn = middleware[i] // 发生越界
// i为3，middleware长度为3，fn赋值为闭包的next，是fnMiddleware执行时所传入的第二个参数
if (i === middleware.length) fn = next // 如果看了compose函数你会发现next是undefined
// fn是undefined，直接返回Promise
if (!fn) return Promise.resolve()Copy
```

next()返回的是resolved的promise对象，没有新的中间件函数入栈了，那么接下来就是出栈的过程，three继续执行console.log(6)然后是dispatch(2)结束，然后是console.log(4)···

此时我们再回头看handleRequest，当fnMiddleware设置的then回调执行的时候，所有的中间件已经执行完毕了

```
  handleRequest(ctx, fnMiddleware) {
    const res = ctx.res;
    res.statusCode = 404;
    const onerror = err => ctx.onerror(err);
    const handleResponse = () => respond(ctx);
    onFinished(res, onerror);
    return fnMiddleware(ctx).then(handleResponse).catch(onerror);
}Copy
```

### 引入异步

刚才的🌰里，引入的中间件并没有async/await的出现，那么引入async await会怎样呢？

```
app.use(async function one(ctx, next) {
  console.log(1);
  await next();
  console.log(2);
});


app.use(async function two(ctx, next) {
  return new Promise(resolve => {
      setTimeout(() =>{
        ctx.body = 'Hello World';
        resolve();
      }, 100)
  })
});Copy
```

一样的逻辑，当one中间件执行next，到了这一步

```
try {
  const next = dispatch.bind(null, i + 1); // next为函数dispatch(1)
  const fnResult = fn(context, next); // 相当于 fnResult = one(context, dispath(1))
  // fnResult 将在100ms后变为Resolved
  return Promise.resolve(fnResult); // Promise.resolve包裹住了一层promise对象
} catch (err) {
  return Promise.reject(err)
}Copy
```

如果Promise.resolve()接受的参数还是个Promise，那么外部的Promise会等待该内部的Promise变成resolved之后，才变成resolved

中间件执行的过程中，one中间件带有await语句，会等待two中间件执行完毕后继续执行，而实现的关键点是Promise.resolve()，如此一来便实现了洋葱圈模型：

[![cRfwhq.png](https://z3.ax1x.com/2021/04/16/cRfwhq.png)](https://z3.ax1x.com/2021/04/16/cRfwhq.png)

### 总结陈词

去年的[Stateofjs](https://2020.stateofjs.com/en-US/technologies/back-end-frameworks/)问卷调查结果显示，Koa的使用率和满意度都在逐年下滑（数据来源基本在海外，国内是另外一副景象，Koa反倒很受欢迎）

[![cRff41.png](https://z3.ax1x.com/2021/04/16/cRff41.png)](https://z3.ax1x.com/2021/04/16/cRff41.png)

出现这样的情况的原因比较容易理解。在Koa框架里写代码其实就是在写中间件，Koa只是为Node.js 的 http 模块基础上提供了一个异步 http server 的模型，它任何功能比如路由、静态文件、测试、日志、进程管理等等，都需要搭配第三方包或手动编写。

Koa没有中间件编写规范，而复杂业务需要很多第三方中间件，导致难以维护。开发者要持续关注项目中使用的第三方包，非常心累（2019年就出了一个Koa-Router项目被卖掉的事件https://www.infoq.cn/article/SsX4YwffzjG*WKVwAdxV）

因此，为了统一代码结构、目录结构和稳定成熟的应用模块加持等方面，阿里在Koa的基础上开发了egg.js框架。Koa的优势是很明显的，中间件的写法非常有效率，高自由度带来的高扩展性更是给足了定制化的空间。在大型业务居多的企业里，有技术人员专门维护第三方插件也可以补足中间件混乱不规范的短板。因此Koa是一个优秀的技术选型。