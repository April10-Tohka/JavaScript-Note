# 1. 同步任务和异步任务
> **同步任务**是那些没有被引擎挂起、在主线程上排队执行的任务。只有前一个任务执行完毕，才能执行后一个任务。
> **异步任务**是那些被引擎放在一边，不进入主线程、而进入任务队列的任务。只有引擎认为某个异步任务可以执行了（比如 Ajax 操作从服务器得到了结果），**该任务（采用回调函数的形式）才会进入主线程执行**。排在异步任务后面的代码，不用等待异步任务结束会马上运行，也就是说，异步任务不具有“堵塞”效应。

> **异步任务会在同步任务之后执行**


# 2.任务队列和事件循环
## 2.1 任务队列
> JavaScript 运行时，除了一个正在运行的主线程，引擎还提供一个任务队列（task queue），里面是各种需要当前程序处理的异步任务。

>等到同步任务全部执行完，就会去看任务队列里面的异步任务。如果满足条件，那么异步任务就重新进入主线程开始执行，这时它就变成同步任务了。等到执行完，下一个异步任务再进入主线程开始执行。一旦任务队列清空，程序就结束执行。

>异步任务的写法通常是回调函数。一旦异步任务重新进入主线程，就会执行对应的回调函数。如果一个异步任务没有回调函数，就不会进入任务队列，也就是说，不会重新进入主线程，因为没有用回调函数指定下一步的操作。
```javascript
new Promise((resolve,reject)=>{
    //ajax网络请求
    if(网络请求成功){resolve()};
    else{reject}
}).then((value)=>{
    //执行回调函数
    // 比如将结果渲染到页面
})
```

## 2.2 事件循环


# 3. setTimeout和setInterval
```javascript
setTimeout(() => {
    console.log("2s后输出2");
}, 2000);
console.log(1);
console.log(3);

//1
//3
//2s后输出2
```
## 3.1 运行机制
`setTimeout`和`setInterval`的运行机制是，将操作的代码移出本轮事件循环，等到下一轮事件循环，再检查是否到了指定时间。如果到了，就执行对应代码，没有则继续等待
`setTimeout`和`setInterval`指定的回调函数，必须等待本轮事件循环的所有同步任务全部执行完毕后，才会执行。
**`setTimeout`和`setInterval`指定的任务，一定会按照预定时间执行的**

>如果`setTimeout`和`setInterval`后面的同步任务运行时间特别长，过了预定时间2s还没有结束，被推迟的sometask只能等待，等到verylongtask执行完毕后，才轮到sometask执行

```javascript
setTimeout(() => {
    sometask()
}, 2000);
verylongtask();
```

## 3.2 setTimeout(f,0)
>预定时间设置为0，**setTimeout(f,0)不会立即执行**
setTimeout会等到本轮事件循环的同步任务全部执行完毕后，在下一次事件循环一开始才会执行
```javascript
setTimeout(()=>{
    console.log(1)
},0)
console.log(2);
//2
//1
```

# 4. Promise
## 4.1 概述
Promise 对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用（proxy），充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像在写同步操作的流程，而不必一层层地嵌套回调函数

## 4.2 Promise()构造函数
new Promise()会返回一个promise实例，具有以下属性方法
* `state`  最初是pending  resolve调用后变为fulfilled reject调用后变为rejected 
* `result` 最初是undefined resolve(value)变为value  reject(error)变为error
* `then()`指定下一步的回调函数

**Promise对象被解析：pending->fulfilled/rejected**
```javascript
new Promise((resolve,reject)=>{
    //要进行异步操作的代码
    这里的代码 在创建Promise对象时就会执行，而不是Promise对象被解析时执行
})  
```

## 4.3 Promise API

### 4.3.1 Promise.resolve/reject
>`Promise.resolve(value)` 返回一个promise，state=fulfilled  result=value

```javascript
let p1=Promise.resolve(88);
p1.then((value)=>{console.log(value)})//88
```

>`Promise.reject(error)`返回一个promise，state=rejected result=error **(很少用到)**

### 4.3.2 Promise.all

## 4.4 then()方法
>**当promise对象被解析（pending->fulfilled/rejected）才会调用**

>`then()`会返回一个新的promise
```javascript
let p=new Promise(resolve=>{
    resolve();
})
//p1来接收then()返回的新的promise
let p1=p.then(()=>{})
p1===p //false
```

>* then(func),func返回的值将作为这个then返回的promise的result
>* 下一个then(func)的func的参数取自于上一个then()返回的promise的result

**注意**
[查看的博客](https://blog.csdn.net/m0_60909419/article/details/124110062?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-0-124110062-blog-119408889.235^v38^pc_relevant_anti_t3_base&spm=1001.2101.3001.4242.1&utm_relevant_index=1)
`then()`返回的promise对象`p1`的状态取决于回调函数的执行结果
回调函数的执行结果
* 1. **回调函数返回非promise对象**,则`p1`的状态为`fulfilled`,`result`为**返回的结果**
![](https://mp-0d3b7191-4034-400e-bd11-c48edcc2d836.cdn.bspapp.com/Note_Image/返回非promise.jpg)
* 2. **回调函数返回promise对象**，则`p1`的状态取决于return返回这个promise对象的内部的状态，内部为`resolve`,p1则为`fulfilled`；内部为`reject`，p1为`rejected`
![](https://mp-0d3b7191-4034-400e-bd11-c48edcc2d836.cdn.bspapp.com/Note_Image/reject下.jpg)

![](https://mp-0d3b7191-4034-400e-bd11-c48edcc2d836.cdn.bspapp.com/Note_Image/resolve下.jpg)
* 3. **抛出错误**，p1的状态为`rejected`,`result`为抛出的值
![](https://mp-0d3b7191-4034-400e-bd11-c48edcc2d836.cdn.bspapp.com/Note_Image/抛出错误.jpg)


## 4.5 微任务
Promise的回调函数属于异步任务，会在同步任务全都执行完后执行
但不是正常的异步任务，是**微任务**
>**区别**
>* 正常的异步任务是在下一次事件循环才执行，而微任务是追加在本次事件循环。
>* 微任务的执行时间一定早于正常的异步任务
```javascript
setTimeout(() => {
    console.log(1);
    
}, 0);
new Promise((resolve)=>{
    resolve(2);
    
}).then((value)=>{console.log(value)});
console.log(3);

//3
//2
//1
```

## 4.5 微任务队列
异步任务需要适当的管理。为此，ECMA 标准规定了一个内部队列 PromiseJobs，通常被称为“微任务队列
**规范**所描述:
* 队列（queue）是先进先出的：首先进入队列的任务会首先运行。
* 只有在 JavaScript 引擎中没有其它任务在运行时，才开始执行任务队列中的任务。

**简单的说**,当一个 promise 准备就绪时，它的 `.then/catch/finally` 处理程序就会被放入队列中：但是它们不会立即被执行。当 JavaScript 引擎执行完当前的代码，它会从队列中获取任务并执行它。

