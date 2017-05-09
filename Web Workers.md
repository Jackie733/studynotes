# Web Workers

Web Worker为Web内容在后台线程中运行脚本提供了一种简单的方法。线程可以执行任务而不干扰用户界面。此外，他们可以使用XMLHttpRequest执行 I/O  (尽管responseXML和通道属性总是为空)。一旦创建， 一个worker 可以将消息发送到创建它的JavaScript代码, 通过将消息发布到该代码指定的事件处理程序 (反之亦然)。

## Web Workers API

一个worker是使用一个构造函数创建的一个对象(e.g. [`Worker()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker/Worker)) 运行一个命名的JavaScript文件 - 这个文件包含将在工作线程中运行的代码; workers 运行在另一个全局上下文中,不同于当前的[`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window). 因此，使用 [`window`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window)快捷方式获取当前全局的范围 (而不是[`self`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/self)) 在一个 [`Worker`](https://developer.mozilla.org/zh-CN/docs/Web/API/Worker) 内将返回错误。

## 关于线程安全

Worker接口会生成真正的操作系统级别的线程，如果你不太小心，那么并发(concurrency)会对你的代码产生有趣的影响。然而，对于 web worker 来说，与其他线程的通信点会被很小心的控制，这意味着你很难引起并发问题。你没有办法去访问非线程安全的组件或者是 DOM，此外你还需要通过序列化对象来与线程交互特定的数据。所以你要是不费点劲儿，还真搞不出错误来。

## 生成worker

创建一个新的 worker 十分简单。你所要做的就是调用 Worker() 构造函数，指定一个要在 worker 线程内运行的脚本的 URI，如果你希望能够收到 worker 的通知，可以将 worker 的 onmessage 属性设置成一个特定的事件处理函数。

```javascript
var myWorker = new Worker("my_task.js");

myWorker.onmessage = function (oEvent) {
  console.log("Called back by the worker!\n");
};

//或者，也可以使用addEventListener():
var myWorker = new Worker("my_task.js");

myWorker.addEventListener("message",function (oEvent) {
  console.log("Called back by the worker!\n");
},false);

myWorker.postMessage("");//start the worker.
```

例子中的第一行创建了一个新的 worker 线程。第三行为 worker 设置了 message事件的监听函数。当 worker 调用自己的 postMessage() 函数时就会调用这个事件处理函数。最后，第七行启动了 worker 线程。

## 传递数据

在主页面与 worker 之间传递的数据是通过**拷贝**，而不是共享来完成的。传递给 worker 的对象需要经过序列化，接下来在另一端还需要反序列化。页面与 worker **不会共享同一个实例，**最终的结果就是在每次通信结束时生成了数据的**一个副本。**大部分浏览器使用[结构化拷贝](https://developer.mozilla.org/en/DOM/The_structured_clone_algorithm)来实现该特性。

在往下进行之前，出于教学的目的，让我们创建一个名为 emulateMessage() 的函数，它将模拟在从 worker到主页面(反之亦然)的通信过程中，变量的「*拷贝而非共享*」行为：

```javascript
function emulateMessage (vVal) {
    return eval("(" + JSON.stringify(vVal) + ")");
}

// Tests

// test #1
var example1 = new Number(3);
alert(typeof example1); // object
alert(typeof emulateMessage(example1)); // number

// test #2
var example2 = true;
alert(typeof example2); // boolean
alert(typeof emulateMessage(example2)); // boolean

// test #3
var example3 = new String("Hello World");
alert(typeof example3); // object
alert(typeof emulateMessage(example3)); // string

// test #4
var example4 = {
    "name": "John Smith",
    "age": 43
};
alert(typeof example4); // object
alert(typeof emulateMessage(example4)); // object

// test #5
function Animal (sType, nAge) {
    this.type = sType;
    this.age = nAge;
}
var example5 = new Animal("Cat", 3);
alert(example5.constructor); // Animal
alert(emulateMessage(example5).constructor); // Object
```

拷贝而并非共享的那个值称为 *消息*。再来谈谈 worker，你可以使用 postMessage() 将消息传递给主线程或从主线程传送回来。`message` 事件的 `data` 属性就包含了从 worker 传回来的数据。

**example.html**: (主页面):

```javascript
var myWorker = new Worker("my_task.js");

myWorker.onmessage = function (oEvent) {
  console.log("Worker said : " + oEvent.data);
};

myWorker.postMessage("ali");
```

**my_task.js** (worker):

```javascript
postMessage("I\'m working before postMessage(\'ali\').");

onmessage = function (oEvent) {
  postMessage("Hi " + oEvent.data);
};
```

**注意：** 通常来说，后台线程 – 包括 worker – **无法操作 DOM。** 如果后台线程需要修改 DOM，那么它应该将消息发送给它的创建者，让创建者来完成这些操作。

如你所见，`worker` 与主页面之间传输的 消息 **始终是 **「**JSON 消息**」，即使它是一个原始类型的值。所以，你完全可以传输 [`JSON`](https://developer.mozilla.org/en-US/docs/JSON) 数据 和/或 任何能够序列化的数据类型：

```javascript
postMessage({"cmd": "init", "timestamp": Date.now()});
```

### 传递数据的例子

#### 例子 #1： 创建一个通用的 「异步 [`eval()`](https://developer.mozilla.org/en-US/docs/JavaScript/Reference/Global_Objects/eval)」

下面这个例子介绍了，如何在 worker 内使用  `eval() `来按顺序执行**异步的**任何种类的 JavaScript 代码：

```javascript
// Syntax: asyncEval(code[, listener])

var asyncEval = (function () {

  var aListeners = [], oParser = new Worker("data:text/javascript;charset=US-ASCII,onmessage%20%3D%20function%20%28oEvent%29%20%7B%0A%09postMessage%28%7B%0A%09%09%22id%22%3A%20oEvent.data.id%2C%0A%09%09%22evaluated%22%3A%20eval%28oEvent.data.code%29%0A%09%7D%29%3B%0A%7D");

  oParser.onmessage = function (oEvent) {
    if (aListeners[oEvent.data.id]) { aListeners[oEvent.data.id](oEvent.data.evaluated); }
    delete aListeners[oEvent.data.id];
  };


  return function (sCode, fListener) {
    aListeners.push(fListener || null);
    oParser.postMessage({
      "id": aListeners.length - 1,
      "code": sCode
    });
  };

})();

//示例使用
// asynchronous alert message...
asyncEval("3 + 2", function (sMessage) {
    alert("3 + 2 = " + sMessage);
});

// asynchronous print message...
asyncEval("\"Hello World!!!\"", function (sHTML) {
    document.body.appendChild(document.createTextNode(sHTML));
});

// asynchronous void...
asyncEval("(function () {\n\tvar oReq = new XMLHttpRequest();\n\toReq.open(\"get\", \"http://www.mozilla.org/\", false);\n\toReq.send(null);\n\treturn oReq.responseText;\n})()");
```

#### 例子 #2：传输 JSON 的高级方式和创建一个交换系统

如果你需要传输非常复杂的数据，还要同时在主页与 Worker 内调用多个方法，那么可以考虑创建一个类似下面的系统。

**example.html** (the main page):

```html
<!doctype html>
<html>
<head>
<meta charset="UTF-8"  />
<title>MDN Example - Queryable worker</title>
<script type="text/javascript">
  /*
    QueryableWorker instances methods:
     * sendQuery(queryable function name, argument to pass 1, argument to pass 2, etc. etc): calls a Worker's queryable function
     * postMessage(string or JSON Data): see Worker.prototype.postMessage()
     * terminate(): terminates the Worker
     * addListener(name, function): adds a listener
     * removeListener(name): removes a listener
    QueryableWorker instances properties:
     * defaultListener: the default listener executed only when the Worker calls the postMessage() function directly
  */
  function QueryableWorker (sURL, fDefListener, fOnError) {
    var oInstance = this, oWorker = new Worker(sURL), oListeners = {};
    this.defaultListener = fDefListener || function () {};
    oWorker.onmessage = function (oEvent) {
      if (oEvent.data instanceof Object && oEvent.data.hasOwnProperty("vo42t30") && oEvent.data.hasOwnProperty("rnb93qh")) {
        oListeners[oEvent.data.vo42t30].apply(oInstance, oEvent.data.rnb93qh);
      } else {
        this.defaultListener.call(oInstance, oEvent.data);
      }
    };
    if (fOnError) { oWorker.onerror = fOnError; }
    this.sendQuery = function (/* queryable function name, argument to pass 1, argument to pass 2, etc. etc */) {
      if (arguments.length < 1) { throw new TypeError("QueryableWorker.sendQuery - not enough arguments"); return; }
      oWorker.postMessage({ "bk4e1h0": arguments[0], "ktp3fm1": Array.prototype.slice.call(arguments, 1) });
    };
    this.postMessage = function (vMsg) {
      //I just think there is no need to use call() method
      //how about just oWorker.postMessage(vMsg);
      //the same situation with terminate
      //well,just a little faster,no search up the prototye chain
      Worker.prototype.postMessage.call(oWorker, vMsg);
    };
    this.terminate = function () {
      Worker.prototype.terminate.call(oWorker);
    };
    this.addListener = function (sName, fListener) {
      oListeners[sName] = fListener;
    };
    this.removeListener = function (sName) {
      delete oListeners[sName];
    };
  };

  // your custom "queryable" worker
  var oMyTask = new QueryableWorker("my_task.js" /* , yourDefaultMessageListenerHere [optional], yourErrorListenerHere [optional] */);

  // your custom "listeners"

  oMyTask.addListener("printSomething", function (nResult) {
    document.getElementById("firstLink").parentNode.appendChild(document.createTextNode(" The difference is " + nResult + "!"));
  });

  oMyTask.addListener("alertSomething", function (nDeltaT, sUnit) {
    alert("Worker waited for " + nDeltaT + " " + sUnit + " :-)");
  });
</script>
</head>
<body>
  <ul>
    <li><a id="firstLink" href="javascript:oMyTask.sendQuery('getDifference', 5, 3);">What is the difference between 5 and 3?</a></li>
    <li><a href="javascript:oMyTask.sendQuery('waitSomething');">Wait 3 seconds</a></li>
    <li><a href="javascript:oMyTask.terminate();">terminate() the Worker</a></li>
  </ul>
</body>
</html>
```

**my_task.js** (the worker):

```javascript
// your custom PRIVATE functions

function myPrivateFunc1 () {
  // do something
}

function myPrivateFunc2 () {
  // do something
}

// etc. etc.

// your custom PUBLIC functions (i.e. queryable from the main page)

var queryableFunctions = {
  // example #1: get the difference between two numbers:
  getDifference: function (nMinuend, nSubtrahend) {
      reply("printSomething", nMinuend - nSubtrahend);
  },
  // example #2: wait three seconds
  waitSomething: function () {
      setTimeout(function() { reply("alertSomething", 3, "seconds"); }, 3000);
  }
};

// system functions

function defaultQuery (vMsg) {
  // your default PUBLIC function executed only when main page calls the queryableWorker.postMessage() method directly
  // do something
}

function reply (/* listener name, argument to pass 1, argument to pass 2, etc. etc */) {
  if (arguments.length < 1) { throw new TypeError("reply - not enough arguments"); return; }
  postMessage({ "vo42t30": arguments[0], "rnb93qh": Array.prototype.slice.call(arguments, 1) });
}

onmessage = function (oEvent) {
  if (oEvent.data instanceof Object && oEvent.data.hasOwnProperty("bk4e1h0") && oEvent.data.hasOwnProperty("ktp3fm1")) {
    queryableFunctions[oEvent.data.bk4e1h0].apply(self, oEvent.data.ktp3fm1);
  } else {
    defaultQuery(oEvent.data);
  }
};
```

这是一个非常合适的方法，用于切换 主页-worker - 或是相反的 - 之间的消息。

### 通过转让所有权（可转让对象）来传递数据

Google Chrome 17 与 Firefox 18 包含另一种性能更高的方法来将特定类型的对象([可转让对象](http://dev.w3.org/html5/spec/common-dom-interfaces.html#transferable-objects)) 传递给一个 worker/从 worker 传回 。可转让对象从一个上下文转移到另一个上下文而不会经过任何拷贝操作。这意味着当传递大数据时会获得极大的性能提升。如果你从 C/C++ 世界来，那么把它想象成按照引用传递。然而与按照引用传递不同的是，一旦对象转让，那么它在原来上下文的那个版本将不复存在。该对象的所有权被转让到新的上下文内。例如，当你将一个 [ArrayBuffer ](https://developer.mozilla.org/en/JavaScript_typed_arrays/ArrayBuffer)对象从主应用转让到 Worker 中，原始的 `ArrayBuffer` 被清除并且无法使用。它包含的内容会(完整无差的)传递给 Worker 上下文。

```javascript
// Create a 32MB "file" and fill it.
var uInt8Array = new Uint8Array(1024*1024*32); // 32MB
for (var i = 0; i < uInt8Array .length; ++i) {
  uInt8Array[i] = i;
}

worker.postMessage(uInt8Array.buffer, [uInt8Array.buffer]);
```

## 生成subworker

如果需要的话 Worker 能够生成更多的 Worker。这样的被称为 subworker，它们必须托管在与父页面相同的源内。同理，subworker 解析 URI 时会相对于父 worker 的地址而不是自身的页面。这使得 worker 容易监控它们的依赖关系。

## 嵌入式worker

目前没有一种「官方」的方法能够像<script>元素一样将 worker 的代码嵌入的网页中。但是如果一个<script>元素没有 src 特性，并且它的type 特性没有指定成一个可运行的 mime-type，那么它就会被认为是一个数据块元素，并且能够被 JavaScript 使用。「数据块」是 HTML5 中一个十分常见的特性，它可以携带几乎任何文本类型的数据。所以，你能够以如下方式嵌入一个 worker：

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8" />
<title>MDN Example - Embedded worker</title>
<script type="text/js-worker">
  // 该脚本不会被 JS 引擎解析，因为它的 mime-type 是 text/js-worker。
  var myVar = "Hello World!";
  // 剩下的 worker 代码写到这里。
</script>
<script type="text/javascript">
  // 该脚本会被 JS 引擎解析，因为它的 mime-type 是 text/javascript。
  function pageLog (sMsg) {
    // 使用 fragment：这样浏览器只会进行一次渲染/重排。
    var oFragm = document.createDocumentFragment();
    oFragm.appendChild(document.createTextNode(sMsg));
    oFragm.appendChild(document.createElement("br"));
    document.querySelector("#logDisplay").appendChild(oFragm);
  }
</script>
<script type="text/js-worker">
  // 该脚本不会被 JS 引擎解析，因为它的 mime-type 是 text/js-worker。
  onmessage = function (oEvent) {
    postMessage(myVar);
  };
  // 剩下的 worker 代码写到这里。
</script>
<script type="text/javascript">
  // 该脚本会被 JS 引擎解析，因为它的 mime-type 是 text/javascript。

  // 在过去...：
  // 我们使用 blob builder
  // ...但是现在我们使用 Blob...:
  var blob = new Blob(Array.prototype.map.call(document.querySelectorAll("script[type=\"text\/js-worker\"]"), function (oScript) { return oScript.textContent; }),{type: "text/javascript"});

  // 创建一个新的 document.worker 属性，包含所有 "text/js-worker" 脚本。
  document.worker = new Worker(window.URL.createObjectURL(blob));

  document.worker.onmessage = function (oEvent) {
    pageLog("Received: " + oEvent.data);
  };

  // 启动 worker.
  window.onload = function() { document.worker.postMessage(""); };
</script>
</head>
<body><div id="logDisplay"></div></body>
</html>
```

## 超时与间隔

Worker 能够像主线程一样使用超时与间隔。这会十分有用，比如说，如果你想让 worker 线程周期性而并非不间断的运行代码。

查看 [`setTimeout()`](https://developer.mozilla.org/en/DOM/window.setTimeout)， `clearTimeout()`， `setInterval()，` 与 [`clearInterval()` ](https://developer.mozilla.org/en/DOM/window.clearInterval)了解详情。还可以查看： [JavaScript 定时器](https://developer.mozilla.org/en-US/docs/JavaScript/Timers)。

## 终止worker

如果你想立即终止一个运行中的 worker，可以调用 worker 的 `terminate()方法：`

```javascript
myWorker.terminate();
```

worker 线程会被立即杀死，不会留下任何机会让它完成自己的操作或清理工作。

Workers 也可以调用自己的 `nsIWorkerScope.close()` 方法来关闭自己：

```javascript
self.close();
```

## 处理错误

当 worker 出现运行时错误时，它的 `onerror` 事件处理函数会被调用。它会收到一个实现了 `ErrorEvent` 接口名为 `error`的事件。该事件不会冒泡，并且可以被取消；为了防止触发默认动作，worker 可以调用错误事件的 `preventDefault() 方法。`

错误事件拥有下列三个它感兴趣的字段：

- `message`

  可读性良好的错误消息。

- `filename`

  发生错误的脚本文件名。

- `lineno`

  发生错误时所在脚本文件的行号。

## 访问navigator对象

Workers 可以在它的作用域内访问 `navigator` 对象。它含有如下能够识别浏览器的字符串，就像在普通脚本中做的那样：

- `appName`
- `appVersion`
- `platform`
- `userAgent`

## 引入脚本与库

Worker 线程能够访问一个全局函数，`importScripts()` ，该函数允许 worker 将脚本或库引入自己的作用域内。你可以不传入参数，或传入多个脚本的 URI 来引入；以下的例子都是合法的：

```javascript
importScripts();                        /* 什么都不引入 */
importScripts('foo.js');                /* 只引入 "foo.js" */
importScripts('foo.js', 'bar.js');      /* 引入两个脚本 */
```

浏览器将列出的脚本加载并运行。每个脚本中的全局对象都能够被 worker 使用。如果脚本无法加载，将抛出 `NETWORK_ERROR` 异常，接下来的代码也无法执行。而之前执行的代码(包括使用 [`window.setTimeout()`](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout) 延迟执行的代码)却依然能够使用。`importScripts()` **之后**的函数声明依然能够使用，因为它们始终会在其他代码之前运行。

**注意：** 脚本的下载顺序不固定，但执行时会按照你将文件名传入到 `importScripts()` 中的顺序。这是同步完成的；直到所有脚本都下载并运行完毕， `importScripts()` 才会返回。

## 例子

### 在后台执行运算

worker 的一个优势在于能够执行处理器密集型的运算而不会阻塞 UI 线程。在下面的例子中，worker 用于计算斐波那契数。

#### JavaScript代码

下面的 JavaScript 代码保存在「fibonacci.js」文件中，与下一节的 HTML 文件关联。

```javascript
var results = [];

function resultReceiver(event) {
  results.push(parseInt(event.data));
  if (results.length == 2) {
    postMessage(results[0] + results[1]);
  }
}

function errorReceiver(event) {
  throw event.data;
}

onmessage = function(event) {
  var n = parseInt(event.data);

  if (n == 0 || n == 1) {
    postMessage(n);
    return;
  }

  for (var i = 1; i <= 2; i++) {
    var worker = new Worker("fibonacci.js");
    worker.onmessage = resultReceiver;
    worker.onerror = errorReceiver;
    worker.postMessage(n - i);
  }
 };
```

worker 将属性 `onmessage` 设置为一个函数，当 worker 对象调用 `postMessage()` 时该函数会接收到发送过来的信息。(注意，这么使用并不等同于定义一个同名的全局*变量* ，或是定义一个同名的*函数*。`var onmessage` 与 `function onmessage` 将会定义与该名字相同的全局属性，但是它们不会注册能够接收从创建 worker 的网页发送过来的消息的函数。) 这会启用递归，生成自己的新拷贝来处理计算的每一个循环。

#### HTML代码

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8"  />
    <title>Test threads fibonacci</title>
  </head>
  <body>

  <div id="result"></div>

  <script language="javascript">

    var worker = new Worker("fibonacci.js");

    worker.onmessage = function(event) {
      document.getElementById("result").textContent = event.data;
      dump("Got: " + event.data + "\n");
    };

    worker.onerror = function(error) {
      dump("Worker error: " + error.message + "\n");
      throw error;
    };

    worker.postMessage("5");

  </script>
  </body>
</html>
```

网页创建了一个 `div` 元素，ID 为 `result` ， 用它来显示运算结果，然后生成 worker。在生成 worker 后，`onmessage` 处理函数配置为通过设置 `div` 元素的内容来显示运算结果，然后 `onerror` 处理函数被设置为 [转储](https://developer.mozilla.org/en/Debugging_JavaScript#dump()) 错误信息。

最后，向 worker 发送一条信息来启动它。

### 在后台运行web I/O

你可以在 [在扩展中使用 worker](https://developer.mozilla.org/En/Using_workers_in_extensions) 这篇文章中找到相关例子。

### 划分任务给多个worker

当多核系统流行开来，将复杂的运算任务分配给多个 worker 来运行已经变得十分有用，这些 worker 会在多处理器内核上运行这些任务。

### 在worker内创建worker

前面斐波那契那个例子已经证明了事实上 worker 能够[生成额外的 workers](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Workers_API/Using_web_workers#Spawning_subworkers)。这使得创建递归程序越发的简单了。