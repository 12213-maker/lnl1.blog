---
title: JS 的同步异步操作 和 事件循环机制
---

# JS 的同步异步操作 和 事件循环机制

总所周知 ,  js 是单线程 , 因此只能依次处理任务 , 只有当前任务完成了之后才可以进行下一个任务 , 这无疑降低了 js  的性能 , 降低了用户的使用体验 , 为了解决这一问题 , js引入了异步操作

### 有哪些是异步操作

1. ``回调函数`` 的方式，使用回调函数的方式有一个缺点是，多个回调函数嵌套的时候会造成回调函数地狱，上下两层的回调函数间的代码耦合度太高，不利于代码的可维护。
2. ``Promise``的方式，使用 Promise 的方式可以将嵌套的回调函数作为链式调用。但是使用这种方法，有时会造成多个 then 的链式调用，可能会造成代码的语义不够明确。
3. ``generator`` 的方式，它可以在函数的执行过程中，将函数的执行权转移出去，在函数外部还可以将执行权转移回来。
4. ``async 函数`` 的方式，async 函数是 generator 和 promise 实现的一个自动执行的语法糖，它内部自带执行器，当函数内部执行到一个 await 语句的时候，如果语句返回一个 promise 对象，那么函数将会等待 promise 对象的状态变为 resolve 后再继续向下执行。因此可以将异步逻辑，转化为同步的顺序来书写，并且这个函数可以自动执行。

### promise的详解

1. ``promise``对象的状态不受外界的影响 , pending fulfilled Rejected
2. ``resolve ``的作用是把promise的状态由 pending -> fulfilled , 并将异步操作的结果作为参数传递出去
3. ``reject`` 的作用是把promise的状态由 pending -> rejected , 并将异步操作的结果作为参数传递出去
4. ``then``方法会自动返回一个promise对象 , 可以链式调用then , 在then里面你可以自己返回promise实例 , 或者then自动返回promise实例加上你的参数

### promise身上方法 : resolved , rejected , all ,race , finally

#### all   

promise.all()该方法用于将多个Promise实例，包装成一个新的Promise实例。这样当遇到发送多个请求并根据请求顺序获取和使用数据的场景，就可以使用Promise.all来解决

````js
let p1 = new Promise((resolve, reject) => {
    resolve('成功了')
    })
    
    let p2 = new Promise((resolve, reject) => {
    resolve('success')
    })
    
    let p3 = Promise.reject('失败')
    
    Promise.all([p1, p2]).then((result) => {
    console.log(result) //['成功了', 'success']
    }).catch((error) => {
    console.log(error)
    })
    
    Promise.all([p1,p3,p2]).then((result) => {
    console.log(result)
    }).catch((error) => {
    console.log(error) // 失败了，打出 '失败'
    })
````



#### race : 

race 第一个执行完的promise 决定race状态  , 那么race方法有什么实际作用呢？当要做一件事，超过多长时间就不做了，可以用这个方法来解决

````js
let promise1 = new Promise((resolve)=>{
    setTimeout(() => {
        resolve('成功执行')
    }, 1500);
})
let promise2 = new Promise((resolve)=>{
    setTimeout(() => {
        resolve('取消执行')
    }, 1000);
})
//超过一秒就不执行 promise1
Promise.race([promise1,promise2]).then(res=>{console.log(res);})
````



#### finally : 

不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的。

````js
let promise2 = new Promise((resolve,rejected)=>{
    if(1==2)resolve('执行成功')
    rejected('执行失败')
}).then((data)=>{console.log(data);},(data)=>{console.log(data);})
.finally(()=>{console.log('我就是要执行');})
````



### 下面来看这道题

````js

console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
````

这道题涉及到了 js 的运行机制 , 事件循环 , 同步异步等知识点 

输出是 : 1，7，6，8，2，4，3，5，9，11，10，12

做这道题的方法就是 : 先把同步的代码执行完 , 再把异步操作放到event table中 ,event queue 里面 宏任务和微任务队列排好之后( ``注意微任务队列和宏任务队列不是一个队列`` ) , 先执行完微任务才能执行微任务 

