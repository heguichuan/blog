---
title: Web Worker 使用教程
tags: worker
---

# Web Worker 使用教程

## 一、参考链接

[软一峰 Web Worker 使用教程](http://www.ruanyifeng.com/blog/2018/07/web-worker.html)

## 二、注意事项

1.  **同源限制** ， 分配给 Worker 线程运行的脚本文件，必须与主线程的脚本文件同源。
2.  **DOM 限制** ， Worker 线程所在的全局对象，与主线程不一样，无法读取主线程所在网页的 DOM 对象，也无法使用`document`、`window`、`parent`这些对象。但是，Worker 线程可以`navigator`对象和`location`对象。
3.  **通信联系** ， Worker 线程和主线程不在同一个上下文环境，它们不能直接通信，必须通过消息完成。
4.  **脚本限制** ， Worker 线程不能执行`alert()`方法和`confirm()`方法，但可以使用 `XMLHttpRequest`对象发出`AJAX`请求。
5.  **文件限制** ， Worker 线程无法读取本地文件，即不能打开本机的文件系统（`file://`），它所加载的脚本，必须来自网络。

## 三、基本用法

### 主线程

采用`new`命令，调用`Worker()`构造函数，新建一个 Worker 线程。 构造函数的参数是一个脚本文件 。如果下载没有成功（比如 404 错误），Worker 就会默默地失败。

> ```javascript
> var worker = new Worker("work.js");
>
> worker.onerror(function(event) {
>     doSomething();
> });
> worker.postMessage("Hello World");
> worker.postMessage({ method: "echo", args: ["Work"] });
>
> worker.onmessage = function(event) {
>     console.log("Received message " + event.data);
>     doSomething();
> };
>
> worker.terminate();
> ```

### Worker 线程

Worker 线程内部需要有一个监听函数，监听`message`事件。 `self`代表子线程自身，即子线程的全局对象

> ```javascript
> self.addEventListener(
>     "message",
>     function(e) {
>         self.postMessage("You said: " + e.data);
>     },
>     false
> );
> // 也可以使用self.onmessage
> // self.close() 用于在 Worker 内部关闭自身。
> ```

Worker 内部如果要加载其他脚本，有一个专门的方法`importScripts()`。

> ```javascript
> importScripts("script1.js");
> importScripts("script1.js", "script2.js");
> ```

### 通信

主线程与 Worker 之间的通信内容，可以是文本，也可以是对象。需要注意的是，这种通信是拷贝关系，即是传值而不是传址，Worker 对通信内容的修改，不会影响到主线程。

### 二进制数据

拷贝方式发送二进制数据，会造成性能问题 。 JavaScript 允许主线程把二进制数据直接转移给子线程，但是一旦转移，主线程就无法再使用这些二进制数据了 。这种转移数据的方法，叫做[Transferable Objects](http://www.w3.org/html/wg/drafts/html/master/infrastructure.html#transferable-objects)。

```javascript
// Transferable Objects 格式
worker.postMessage(arrayBuffer, [arrayBuffer]);

// 例子
var ab = new ArrayBuffer(1);
worker.postMessage(ab, [ab]);
```

### API

```javascript
var worker = new Worker(jsUrl, options);
```

-   worker.onerror：指定 error 事件的监听函数。
-   worker.onmessage：指定 message 事件的监听函数，发送过来的数据在`Event.data`属性中。
-   worker.onmessageerror：指定 messageerror 事件的监听函数。发送的数据无法序列化成字符串时，会触发这个事件。
-   worker.postMessage()：向 Worker 线程发送消息。
-   worker.terminate()：立即终止 Worker 线程。
-   self.name： Worker 的名字。该属性只读，由构造函数指定。
-   self.onmessage：指定`message`事件的监听函数。
-   self.onmessageerror：指定 messageerror 事件的监听函数。发送的数据无法序列化成字符串时，会触发这个事件。
-   self.close()：关闭 Worker 线程。
-   self.postMessage()：向产生这个 Worker 线程发送消息。
-   self.importScripts()：加载 JS 脚本。

## 四、worker-farm

在项目中使用过一次[worker-farm](https://www.npmjs.com/package/worker-farm)，感觉还是比较简单。

自己使用体验，放到 callback 里面的代码是不会多线程执行的，child.js 里面定义的代码才是真的多线程异步执行。

_child.js_:

```javascript
module.exports = function(inp, callback) {
    callback(null, inp + " BAR (" + process.pid + ")");
};
```

main file:

```javascript
var workerFarm = require("worker-farm"),
    workers = workerFarm(require.resolve("./child")),
    ret = 0;
for (var i = 0; i < 10; i++) {
    workers("#" + i + " FOO", function(err, outp) {
        console.log(outp);
        if (++ret == 10) workerFarm.end(workers);
    });
}
```

output something like the following:

```bash
#1 FOO BAR (8546)
#0 FOO BAR (8545)
#8 FOO BAR (8545)
#9 FOO BAR (8546)
#2 FOO BAR (8548)
#4 FOO BAR (8551)
#3 FOO BAR (8549)
#6 FOO BAR (8555)
#5 FOO BAR (8553)
#7 FOO BAR (8557)
```
