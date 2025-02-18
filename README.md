
# 事件循环 Event Loop
**`事件循环（Event Loop）是 JavaScript 实现异步非阻塞的关键机制。它不断地循环检查任务队列，并决定下一步执行哪个任务。理解宏任务（macrotask）和微任务（microtask）在事件循环中的执行顺序对于编写高性能的 JavaScript 代码至关重要。`** 

## 宏任务
**`宏任务： 宏任务是由宿主环境（例如浏览器或 Node.js）发起的任务。常见的宏任务包括：`**
* **`setTimeout`**  
* **`setInterval`**  
* **`setImmediate (Node.js) `** 
* **`I/O 操作`** 
* **`UI 渲染`**  
* **`script (首次执行的脚本)`** 

## 微任务
**`微任务： 微任务是由 JavaScript 引擎自身发起的任务。常见的微任务包括：`**
* **`Promise.then`**  
* **`MutationObserver`**  
* **`process.nextTick (Node.js) `** 
* **`queueMicrotask`** 

## 执行顺序
**`事件循环的每一次循环称为一个 "tick"。在一个 tick 中，事件循环会按照以下顺序执行任务：`** 

**`1. 执行栈： 首先执行栈中的所有同步代码会被执行完毕`**

**`2. 微任务队列：接下来事件循环会检查微任务队列，并执行队列中所有的微任务。只有当微任务队列为空时，才会进入下一个宏任务。如果在执行微任务的过程中，又产生了新的微任务，那么新的微任务会加入到队列末尾，并在当前 tick 中继续执行`**

**`3. 宏任务队列：当微任务队列为空时，事件循环会从宏任务队列中取出一个任务执行`**

**`4. 渲染：浏览器会更新渲染页面`**

**`5. 事件循环会不断重复以上步骤`**

## 总结

**`事件循环的执行顺序是：`**

**`1. 执行栈中的同步代码`**

**`2. 执行所有可执行的微任务`**

**`3. 执行一个宏任务`**

**`4. 更新渲染`**

**`5. 重复以上步骤`**

## 如果在执行微任务的过程中，又产生了新的微任务，那么新的微任务会加入到微任务队列的末尾，并且在当前 tick 中继续执行

**`示例`**
```javascript
console.log("script start);

Promise.resolve().then(function() {
  console.log("promise1");

  Promise.resolve().then(function() {
    console.log("promise2");
  });
});

setTimeout(function() {
  console.log("setTimeout");
}, 0);

console.log("script end");
```

**`输出结果`**
```
script start
script end
promise1
promise2
setTimeout
```




# 输入 URL 到页面渲染的整个流程

## DNS

DNS 的作用就是通过域名查询到具体的 IP。

## TCP 
在这一部分中，可以详细说下 TCP 的握手情况以及 TCP 的一些特性。

## HTTP
假设服务端会响应一个 HTML 文件。

## 浏览器渲染原理

# 浏览器渲染原理
## 浏览器接收到 HTML 文件并转换为 DOM 树
当我们打开一个网页时，浏览器都会去请求对应的 HTML 文件。虽然平时我们写代码时都会分为 js、css、html 文件，也就是字符串，但是计算机硬件是不理解这些字符串的，所以在网络中传输的内容其实是 **`0`** 和 **`1`** 这些 **字节数据**。当浏览器接收到这些字节数据以后，它会将这些 **字节数据转换为字符串**，也就是我们写的代码。

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/1.png?raw=true)

当数据转换为字符串以后，浏览器会先将这些字符串通过词法分析转换为 **标记**（token），这一过程在词法分析中叫做 **标记化**（tokenization）。

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/2.png?raw=true)

那么什么是标记呢？这其实属于编译原理这一块的内容了。简单来说，标记还是字符串，是构成代码的 **最小单位**。这一过程会将代码分拆成一块块，并给这些内容打上标记，便于理解这些最小单位的代码是什么意思。

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/3.png?raw=true)

当结束话标记化后，这些标记会紧接着转换为 Node，最后这些 Node 会根据不同 Node 之前的联系构建为一颗 DOM 树。

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/4.png?raw=true)

以上就是浏览器从网络中接收到 HTML 文件然后一系列的转换过程。

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/5.png?raw=true)

当然，在解析 HTML 文件的时候，浏览器还会遇到 CSS 和 JS 文件，这时候浏览器也会去下载并解析这些文件，接下来就让我们先来学习浏览器如何解析 CSS 文件。

## 将 CSS 文件转换为 CSSOM 树
其实转换 CSS 到 CSSOM 树的过程和上一小节的过程是极其类似的

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/6.png?raw=true)

在这一过程中，浏览器会确定下每一个节点的 **样式** 到底是什么，并且这一过程其实是 **很消耗资源** 的。因为样式你可以自行设置给某个节点，也可以通过继承获得。在这一过程中，浏览器得 **递归** CSSOM 树，然后确定具体的元素到底是什么样式。

## 生成渲染树
当我们生成 DOM 树和 CSSOM 树以后，就需要将这两棵树组合为渲染树。

![](https://github.com/WqhForGitHub/browser-basics/blob/main/static/7.png?raw=true)

在这一过程中，不是简单的将两者合并就行了。渲染树只会包括 **需要显示的节点** 和这些节点的样式信息，如果某个节点是 **`display: none`** 的，那么就不会在渲染树中显示。

当浏览器生成渲染树以后，就会根据渲染树来进行布局（也可以叫做回流），然后调用 GPU 绘制，合成图层，显示在屏幕上。


