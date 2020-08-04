#  手写Promise

按照[Promise/A+](https://promisesaplus.com/)规范，手写一个Promise

Promise的相关性质在这里就不做赘述，直接开搞！

##  Promise的基本框架搭建

###  Promise代码基本结构

实例化Promise对象时传入一个函数作为执行器`executor`，其有两个参数（`resolve`和`reject`）分别将结果变为成功态和失败态。其中包含3个属性，分别为`state`、`value`、`reason`。

`state`：保存Promise对象的状态，可以为：等待状态 `pending`、成功状态`resolved`、失败状态`rejected`。

并且要求Promise在实例化后立即执行，因此，基本结构可以写成：

```js
function MyPromise(executor) {
    var _this = this;
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    
    function resolve(value){
        //some code
    }
    
    function reject(reason){
        //some code
    }
    //这样的话，实例化一个MyPromise对象时，直接运行executor函数
    executor(resolve,reject)
}
```

###  then方法定义在原型上

每一个Promise实例都有一个`then`方法，用来处理异步返回的结果，因此将`then`方法定义在原型上。

```js
MyPromise.prototype.then = function(onFulfilled,onRejected){
    //some code
}
```

此时，Promise的基本框架已经搭好，接下来对其功能进行完善。

##  实现Promise的状态改变

我们知道`resolve`函数在异步操作成功时，将Promise对象的状态从“未完成”变为“成功”，并将异步操作的结果作为参数传递出去。`reject`函数：在异步操作失败时将Promise对象的状态从“未完成”变为“失败”，并将异步操作报出的错误作为参数传递出去。并且状态的改变只能是`pending->resolved`和`pending->rejected`，以及变化后状态固定。

```js
function resolve(value){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.value = value;
        _this.state = 'resolved'
    }
}

function reject(reason){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.reason = reason;
        _this.state = 'rejected'
    }
}
```

##  基本实现then方法

因为当Promise状态发生改变后，都会调用`then`方法，不论成功或失败。这个函数的参数都是可选的，当参数不为函数则忽略。如果传入的参数为函数则执行。

```js
MyPromise.prototype.then = function(onFulfilled,onRejected){
    if(this.state === 'resolved'){
        if(typeof onFulfilled === 'function'){
            onFulfilled(this.value)
        }
    }
    if(this.state === 'rejected'){
        if(typeof onRejected === 'function'){
            onRejected(this.reason)
        }
    }
}
```

##  完善Promise的功能

目前为止，已经可以实现Promise的基本功能，但仍有某些性质需要完善，比如异步运行，链式调用，返回新的Promise等，接下来便是对Promise的逐步完善。

###  实现异步执行

现在的MyPromise还不支持异步代码。什么意思呢？来举例说明

```js
let p1 = new MyPromise((resolve,reject) => {
    resolve(1)
});
p1.then(data => console.log(data))

let p2 = new MyPromise((resolve,reject) => {
    setTimeout(() => {
        resolve(1)
    },0)
});
p2.then(data => console.log(data))  //没有输出
```

由于代码的执行顺序是先执行同步代码再执行异步代码，`then()`方法在`state`属性改变前就已执行，因此上方代码中的`p2`没有输出。

鉴于这个原因，我们参考**发布订阅者模式**。MyPromise的`state`作为发布者，各个`onFulfilled`、`onRejected`作为订阅者。调用`then()`时，先将`onFulfilled`、`onRejected`函数保存起来。当异步操作完成，`state`状态变化，通知订阅者并调用。

因此，对MyPromise进行改进。

首先，在MyPromise的构造函数中新增两个属性，`onFulfilledFunc`和`onRejectedFunc`分别用来保存`onFulfilled`和`onRejected`函数，因为可能有多个函数，所以这两个属性均为数组。

```js
function MyPromise(executor) {
    var _this = this;
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledFunc = [];//保存状态变为成功时调用的回调函数
    this.onRejectedFunc = [];//保存状态变为失败时调用的回调函数
    // other code...
}
```

其次，调用`then()`时将`onFulfilled`和`onRejected`函数放入对应的数组中。

```js
MyPromise.prototype.then = function(onFulfilled,onRejected){
    //Promise中有异步代码
    if(this.state === 'pending'){
        if(typeof onFulfilled === 'function'){
            this.onFulfilledFunc.push(onFulfilled)//保存
        }
        if(typeof onRejected === 'function'){
            this.onRejectedFunc.push(onRejected)//保存
        }
    }
    //Promise中为同步代码,立即执行
    if(this.state === 'resolved'){
        if(typeof onFulfilled === 'function'){
            onFulfilled(this.value)
        }
    }
    //Promise中位同步代码，立即执行
    if(this.state === 'rejected'){
        if(typeof onRejected === 'function'){
            onRejected(this.reason)
        }
    }
}
```

最后，当异步操作完成，改变`state`状态，并调用对应数组中所有函数。

```js
function MyPromise(executor) {
    // other code...
    function resolve(value){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.value = value;
        _this.state = 'resolved';
        _this.onFulfilledFunc.forEach(fn => fn(value))
    }
}

function reject(reason){
    //当状态为pending时才可以改变状态
    if(_this.state === 'pending'){
        _this.reason = reason;
        _this.state = 'rejected';
         _this.onRejectedFunc.forEach(fn => fn(reason))
    }
}
}
```

此时，已经支持异步操作。

```js
//测试
let p1 = new MyPromise((resolve,reject) => {
    resolve(1)
});
p1.then(data => console.log(data))  //1

let p2 = new MyPromise((resolve,reject) => {
    setTimeout(() => {
        resolve(2)
    },0)
});
p2.then(data => console.log(data))  //2
```

###  保证then方法返回一个新的Promise

使用Promise进行异步编程的最重要的原因就是其规避了嵌套地狱，改为了链式调用。要想实现链式调用，那么`then()`返回一个Promise对象是前提。

规定onFulfilled,onRejected都是可选参数，如果他们不是函数，必须被忽略

- onFulfilled返回一个普通的值，成功时直接等于 `value => value`
- onRejected返回一个普通的值，失败时如果直接等于 value => value，则会跑到下一个then中的onFulfilled中，所以直接扔出一个错误`reason => throw err`

```js
MyPromise.prototype.then = function (onFulfilled, onRejected) {
    // onFulfilled如果不是函数，就忽略onFulfilled，直接返回value
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value;
    // onRejected如果不是函数，就忽略onRejected，直接扔出错误
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };
    let promise2 = new Promise((resolve, reject) => {
    //some code
    }
    return promise2;
};
```

###  实现Promise的链式调用

Promise链式调用的规则为：

1. 每个`then`方法都返回一个新的Promise对象
2. 如果`then`方法中显式的返回了一个Promise对象，就不做任何变动，直接返回其结果
3. 如果`then`方法中返回的是一个普通值（Number、String等）就将这个值包装为一个新Promise对象并返回
4. 如果`then`方法中没有return语句，则视为返回一个包装Undefined的Promise对象
5. 如果`then`方法中出现异常，则调用失败态方法（reject）跳转到下一个`then`的`onRejected`函数
6. 如果`then`方法没有传入任何回调，则继续向下传递值

因为要实现的这些功能较为复杂，抽离一个方法来实现。

```js
/**
 * 解析then返回值与新Promise对象
 * @param {Object} promise2 新的Promise对象 
 * @param {*} x 上一个then的返回值
 * @param {Function} resolve promise2的resolve
 * @param {Function} reject promise2的reject
 */
function resolvePromise(promise2, x, resolve, reject) {
    //...
}
```

首先需要判断参数`promise2`与`x`是否相等，若相等则会发生循环引用，无法正常执行。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
}
```

然后按参数`x`的类型分别处理。当`x`是一个Promise，就直接执行；当`x`是一个对象或函数，再进一步处理；剩下情况则为对普通值的处理。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
    
    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
    }else{
        //x为普通值
        resolve(x)
    }
}
```

如果`x`是一个对象，我们就以是否有`then`方法判断其是否为Promise对象。若报错，则直接将`promise2`转为失败态。若有`then`，且为一个函数，就可认定`x`是Promise对象，之后使用`x`作为`this`调用`then`方法。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
    
    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
        try{
            let then = x.then；
            if(typeof then === 'function'){
                //then是function，那么执行Promise
                then.call(x, y =>{
                    resolve(y)
                },(r) => {
                    reject(r)
                })
            }
        }catch(e){
            reject(e)
        }
    }else{
        //x为普通值
        resolve(x)
    }
}
```

若Promise对象转为成功态或失败态时传入了一个Promise对象，此时应使用递归的方法继续执行，直至Promise全部执行完。

```js
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 == x){
        reject(new TypeError('Promise发生了循环引用'))
    }
    
    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
        try{
            let then = x.then；
            if(typeof then === 'function'){
                //then是function，那么执行Promise
                then.call(x, y =>{
                    //递归调用，传入y若是Promise对象，继续循环
                    resolvePromise(promise2, y, resolve, reject)
                },(r) => {
                    reject(r)
                })
            }
        }catch(e){
            reject(e)
        }
    }else{
        //x为普通值
        resolve(x)
    }
}
```

最后，由于我们自己的代码是同步执行，那么使用setTimeout函数来变为异步执行。

```js
setTimeout(() => {
    try {
        let x = onFulfilled(value);
        resolvePromise(promise2, x, resolve, reject);
    } catch (e) {
        reject(e);
    }
},0);
```

##  完整代码

```js
function MyPromise(executor) {
    let _this = this;
    this.state = 'pending';
    this.value = undefined;
    this.reason = undefined;
    this.onFulfilledFunc = [];//保存状态变为成功时调用的回调函数
    this.onRejectedFunc = [];//保存状态变为失败时调用的回调函数

    function resolve(value){
        //当状态为pending时才可以改变状态
        if(_this.state === 'pending'){
            _this.value = value;
            _this.state = 'resolved';
            _this.onFulfilledFunc.forEach(fn => fn(value))
        }
    }

    function reject(reason){
        //当状态为pending时才可以改变状态
        if(_this.state === 'pending'){
            _this.reason = reason;
            _this.state = 'rejected';
            _this.onRejectedFunc.forEach(fn => fn(reason))
        }
    }
    //这样的话，实例化一个MyPromise对象时，直接运行executor函数
    //executor执行可能出错
    try{
      executor(resolve, reject);
    } catch (err) {
      reject(err);
    }
}

MyPromise.prototype.then = function(onFulfilled,onRejected){
	// onFulfilled如果不是函数，就忽略onFulfilled，直接返回value
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : value => value;
    // onRejected如果不是函数，就忽略onRejected，直接扔出错误
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };
    let promise2 = new MyPromise((resolve,reject) => {
        //Promise中有异步代码
        if(this.state === 'pending'){
            this.onFulfilledFunc.push(() => {
                // 异步
                setTimeout(() => {
                    try {
                        let x = onFulfilled(this.value);
                        // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                        resolvePromise(promise2, x, resolve, reject);
                    } catch (e) {
                        reject(e);
                    }
                }, 0);
            });//保存
            this.onRejectedFunc.push(() => {
                setTimeout(() => {
                    try{

                        let x = onRejected(this.reason);
                        // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                        resolvePromise(promise2, x, resolve, reject)
                    }catch(e){
                        reject(e)
                    }
                },0)
            })//保存
        }

        if(this.state === 'resolved'){
            // 异步
            setTimeout(() => {
                try {
                    let x = onFulfilled(this.value);

                    // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                    resolvePromise(promise2, x, resolve, reject);
                } catch (e) {
                    reject(e);
                }
            }, 0)
        }

        if(this.state === 'rejected'){
            // 异步
            setTimeout(() => {
                try{
                    let x = onRejected(this.reason);
                    // resolvePromise函数，处理自己return的promise和默认的promise2的关系
                    resolvePromise(promise2, x, resolve, reject)
                }catch(e){
                    reject(e)
                }
            },0)
        }
});
    return promise2
};


/**
 * 解析then返回值与新Promise对象
 * @param {Object} promise2 新的Promise对象 
 * @param {*} x 上一个then的返回值
 * @param {Function} resolve promise2的resolve
 * @param {Function} reject promise2的reject
 */
function resolvePromise(promise2, x, resolve, reject){
    if(promise2 === x){
        reject(new TypeError('Promise发生了循环引用'))
    }

    if(x!=null && (typeof x==='object'||typeof x==='function')){
        //x为对象或函数
        try{
            let then = x.then;
            if(typeof then === 'function'){
                //then是function，那么执行Promise
                then.call(x, y =>{
                    //递归调用，传入y若是Promise对象，继续循环
                    resolvePromise(promise2, y, resolve, reject)
                },(r) => {
                    reject(r)
                })
            }
        }catch(e){
            reject(e)
        }
    }else{
        //x为普通值
        resolve(x)
    }
}
```

##  各方法的实现

###  Promise.all

```js
Promise.all(iterable);
```

参数：

- iterable：一个可迭代对象，如Array或String

返回值：

- 如果传入的参数是一个空的迭代对象，则返回一个已完成状态的Promise
- 如果传入的参数不包含任何Promise，则返回一个异步完成Promise
- 其他情况下返回一个处理中的Promise。这个返回的Promise之后会在所有的Promise都完成或有一个Promise失败时异步的变为完成或失败

####  实现

根据描述，设计思路为：

1. promise.all()参数应该是一个数组
2. promise.all()返回值是new Promise
3. 需要用一个数组存放每一个promise的结果值
4. 遍历参数数组，判断是否是promise，是的话执行得到结果后压入结果数组；否则直接放入结果数组。
5. 当每个都成功执行后，resolve（result）
6. 当有一个失败，reject

```js
//使用resolve方法
MyPromise.all = function(promises){
    //首先判断参数是否为迭代器对象
    if(typeof(promises[Symbol.iterator]) !== 'function' ){
        throw new Error('Arguments must be Iterator')
    }
    //目前只适用于数组与字符串,未考虑Map，Set的情况
    return new MyPromise((resolve,reject) => {
        let resolvedCounter = 0,//用以计算数组元素完成的个数
            promiseNum = promises.length,
            resolvedValues = new Array(promiseNum);//保存结果
        if(promiseNum === 0) resolve([]);
        for(let i=0; i<promiseNum; i++){
            //使用resolve方法可以不需要判断数组元素是否为Promise对象
            Promise.resolve(promises[i]).then(res => {
                resolvedCounter++;
                resolvedValues[i] = res;
                //当数组元素全部达到完成态，新的Promise实例才算完成
                if(resolvedCounter === promiseNum){
                    resolve(resolvedValues)
                }
            },err => reject(err))//有一个元素出错则到达失败状态
        }
    })
}

//不使用resolve方法
MyPromise.all = function(promises){
    //首先判断参数是否为迭代器对象
    if(typeof(promises[Symbol.iterator]) !== 'function' ){
        throw new Error('Arguments must be Iterator')
    }
    //目前只适用于数组与字符串,未考虑Map，Set的情况
    return new MyPromise((resolve,reject) => {
        let resolvedCounter = 0,//用以计算数组元素完成的个数
            promiseNum = promises.length,
            resolvedValues = new Array(promiseNum);//保存结果
        if(promiseNum === 0) resolve([]);
        for(let i=0; i<promiseNum; i++){
            if(isPromise(promises[i])){
                promises[i].then(value => {
                    resolvedCounter++;
                    resolvedValues[i] = value;
                    //因为可能有异步代码，所以判断应该放在then方法里
                    if(resolvedCounter === promiseNum){
                        resolve(resolvedValues)
                    }
                },err => reject(err))
            }else{
                resolvedCounter++;
                resolvedValues[i] = promises[i]
            }
        }
    })
}
```

###  Promise.race

`Promise.race(iterable)`

参数：

- iterable：一个可迭代对象，如Array或String

返回值：

- 一个待定的Promise只要给定迭代中的一个Promise解决或拒绝，就采用第一个Promise的值作为它的值，从而异步地解析或拒绝

###  实现

根据要求，迭代对象哪一个先到达完成态就返回其结果

```js
MyPromise.race = function(promises){
    //首先判断参数是否为迭代器对象
    if(typeof(promises[Symbol.iterator]) !== 'function' ){
        throw new Error('Arguments must be Iterator')
    }
    //目前只适用于数组与字符串,未考虑Map，Set的情况
    return new MyPromise((resolve,reject) => {
        //传入空数组，返回的Promise永远是等待状态
        for(let i=0; i<promises.length; i++){
            //使用resolve方法可以不需要判断数组元素是否为Promise对象
            Promise.resolve(promises[i]).then(res => {
                resolve(res)//有一个元素完成则到达成功状态
            },err => reject(err))//有一个元素出错则到达失败状态
        }
    })
}
```

