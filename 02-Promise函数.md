# 什么是Promise函数？

> 简单讲，Promise就是一个对象

- 它具有pending(等待态),fulfilled(成功态),rejected（失败态）三种属性。用来通知cpu，await标识的函数是否已经运行完成，是否可以开始执行接下来的回调函数。当成功时，需要有一个成功接收的返回值value，`new Promise((resolve,reject)=>{resolve(value)})`;相应的失败了，需要有一个失败的reason，`new Promise((resolve,reject)=>{reject(reason)})`.

- Promise具有then()方法，具有两个方法分别是onFulfilled和onRejected，需要分别将value/reason作为第一个参数，第二个参数则是相关的回调函数。

- 但是很有可能Promise具有then()回调，或者其中穿插了setTimeout本身要得到一个state的状态变化还需要一定的时间，于是可以放置一个`callback[]`数组去保存全部的回调。

- `new Promise().then().then()`是一个经典的链式回调。为了实现链式，会在第一个`new Promise().then()`中返回一个新的promise2。

- promise链式调用如果传入then()的值不是一个函数，而是一个常数，则直接返回上一个promise的值。

于是乎，一个简单的Promise类
```javascript
    class Promise{
        constructor(executor){
            this.state = 'pending';
            this.value = undefined;
            this.reason = undefined;
            
            //放置成功时需要执行的回调函数
            this.onResolvedCallbacks = [];
            //放置失败时候需要执行的回调函数
            this.onRejectedCallbacks = [];
            
            let resolve = value=>{
                if(this.state==='pending'){
                    this.state = 'fulfilled';
                    this.value = value;
                    //遍历成功函数
                    this.onResolvedCallbacks.forEach(fn=>fn());
                }
            }
            let reject = reason=>{
                if(this.state === 'pending'){
                    this.state = 'rejected';
                    this.reason = reason;
                    //遍历失败函数
                    this.onRejectedCallbacks.forEach(fn=>fn());
                }
            }
            try{
                executor(resolve,reject);
            }catch(err){
                reject(error);
            }
        };

        then(onFulfilled,onRejected){

            onFulfilled = typeof onFulfilled === 'function'? onFulfilled:value=>value;
            onRejected = typeof onRejected === 'function'? onRejected:reason=>{throw new Error(reason)};

            return new Promise((resolve,reject)=>{
                let fulfilled = ()=>{
                    //比较res和newPromise
                    try{
                        let res = onFulfilled(this.value);
                        return res instanceof Promise? res.then(resolve,reject):resolve(res);
                    }catch(err){
                        reject(err);
                    }
                }
                let rejected= ()=>{
                    try{
                        let res = onRejected(this.reason);
                        return res instanceof Promise? res.then(resolve,reject):reject(res);
                    }catch(err){
                        reject(err);
                    }
                }
                switch(this.state){
                    case 'fulfilled':
                        fulfilled();
                        break;
                    case 'rejected':
                        rejected();
                        break;
                    case 'pending':
                        this.onResolvedCallbacks.push(fulfilled());
                        this.onRejectedCallbacks.push(rejected());
                        break;
                }
            })
        }

        catch(onRejected){
            return this.then(null,onRejected);
        }

        static resolve(value){
            if(value instanceof Promise){
                return value;
            }else{
                new Promise((resolve,reject)=>{
                    resolve(value);
                })
            }
        }

        static reject(reason){
            if(reason instanceof Promise){
                return reason;
            }else{
                new Promise((resolve,reject)=>{
                    resolve(reason);
                })
            }
        }

        static all(promiseAll){
            const len = promiseAll.length;
            const values = new Array(len);

            let count = 0;
            return new Promise((resolve,reject)={
                for(int i = 0;i<len；i++){
                    Promise(resolve(promiseAll[i])).then(
                        fulfilled=>{
                            values[i] = fulfilled;
                            count++;
                            if(count === len){
                                resolve(values);
                            }
                        },
                        rejected=>{
                            reject(rejected);
                        }
                    )
                }
            })
        }
    }

```


Promise相对于传统的回调函数，具有一个显著的特点是链式调用，能够使得代码的可读性得到提高。

- 传统的回调函数

```javascript
    setTimeout(function(){
        var d = new Date();
        document.getElementById('two').innerHTML = d.toLocalTimeString();
        setTimeout(function(){
            setTimeout(function(){
                //
            },1000)
        },1000)
    },1000)
```

- 引入Promise之后的回调函数

```javascript
    let num = 1;
    function A(){
        var pro = new Promise(function(resolve,reject){
            var greet = "hello world";
            resolve(greet);
        });
        return pro;
    }
    A().then(num=>{
        console.log(num++);
        return num;
    })
    .then(num=>{
        console.log(num--);
        return num;
    })
    .then(num=>{
        console.log(num+100);
        return num+100;
    })
```
因此这段代码放置在vscode等编译器的环境下，**then(){}**函数则可以很好地折叠起来。

在实际应用中往往会在store的仓库中使用async/await/promise，看一下下面项目中的实例：

```javascript
    async addOrUpdateShopCart({commit},{skuId,skuNum}){
        let result = await reqAddOrUpdate(skuId,skuNum);
        if(result.code == 200){
            return "成功";
        }else{
            return Promise.reject(new Error("failed"));
        }
    }
```

> JavaScript是一种单线程的编程语言，单线程的编程语言的特性使得js适用于浏览器频繁打开关闭网页，刷新网址等密集型cpu运行工作；而像是多线程语言，例如python，则适用于深度学习等需要长时间占据cpu/gpu的运行工作。