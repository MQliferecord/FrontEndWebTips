# 手撕call、apply、bind函数

### 手撕call函数

在写代码之前需要先明白call代码的运作原理：

实际上就是对于外部环境传入的指针`thisArg`，构造了一个该指针的属性func，然后把原函数的属性赋值给了这个`thisArg.func`，最后让返回新的复制结果，并删除工具函数`thisArg.func`。

```javascript
//完整的代码
Function.prototype.mycall = function(thisArg){
    thisArg = thisArg || window;
    thisArg.func = this;
    const args = [];
    for(let i = 1;i<arguments.length;i++){
        args.push('arguments[' + i + ']');
    }
    const result = eval('thisArg.func(' + args + ')');
    delete thisArg.func;
    return result;
}

//ES6语法版本
Function.prototype.mycall2 = function(thisArg,...args){
    //外部是否传参
    thisArg = thisArg || window;
    //获得Function内部的指针
    thisArg.func = this;
    args = args || [];、
    //通过内部指针获取属性值并且赋值到另一个地址
    const result = thisArg.func(...args);
    delete thisArg.func;
    return result;
}
```

### 手撕apply函数

apply()方法和call()方法并没有太大的区别。call()方法接受的是一个参数的列表，而apply()方法接受的是一个参数的数组。

```javascript
//完整的代码
Function.prototype.myapply = function(thisArg){
    thisArg = thisArg || window;
    thisArg.func = this;
    const args = [];
    for(let i = 1;i<arr.length;i++){
        args.push('arr[' + i + ']');
    }
    const result = eval('thisArg.func(' + args + ')');
    delete thisArg.func;
    return result;
}

//ES6语法版本
Function.prototype.myapply2 = function(thisArg,...args){
    thisArg = thisArg || window;
    thisArg.func = this;
    args = args || [];
    const result = thisArg.func(...args);
    delete thisArg.func;
    return result;
}
```

### 手撕bind函数

call,apply,bind三者传参，但是apply传递的参数是数组，而call是参数列表，并且apply和call都是一次性传入参数，而bind可以分为多次传入。

并且因为bind的实现使用了柯里化编程，bind可以返回绑定this之后的函数，便于稍后调试；apply,call则是立即执行。

```javascript
//完整的代码
Function.prototype.mybind = function(thisArg){
    /**
     * bind采用柯里化的方法，所以返回值应该是一个函数。
     * 这里判断返回值是否是函数。
    */
    if(typeof this!=='function'){
        throw new TypeError('Function.prototype.bind - what is trying to be bound is not call');
    }    
    /**
     * @args 返回由arguments组成的数组
     * @fToBind 获得Function内部的指针
     * @fNOP 一个用于实现寄生式继承的中间函数
     * @fBound 返回的闭包函数
    */
    let args = Array.prototype.slice.call(arguments,1),
    fToBind = this,
    fNOP = function(){},
    fBound = function(){
        return fToBind.apply(
            this instanceof fBound ? this:thisArg,
            args.concat(Array.prototype.slice.call(arguments))
        );
    }
    if(this.prototype){
        fNOP.prototype = this.prototype;
    }
    fBound.prototype = new fNOP();
    return fBound;
}

//ES6语法版本
Function.prototype.mybind2 = function(thisArg,...args1){
    let _this = this;
    return function(...args2){
        return _this.call(thisArg,...args1,...args2);
    }
}
```

> 柯里化编程：将多参数的函数转化为单参数的函数。也就是在函数里面返回函数，再返回函数。

先看一个案例来初步了解一下柯里化编程：

```javascript
//普通函数
function plus(x,y){
    return x+y;
}
plus(1+2);

//柯里化后的函数
function plus2(y){
    return function(x){
        return x+y;
    }
}

plus2(1)(2);
```

柯里化变成的作用：

1. 参数复用-当在多次调用同一个函数的时候，并且传递的参数绝大多数都是相同的，该函数可以进行柯里化。

2. 提前返回-多次调用多次内存判断，可以直接把最后判断的结果返回到外部接收

3. 延迟计算/运行-避免重复的去执行程序，等真正需要结果的时候再执行。

```javascript
//函数一：作用是输入三个参数返回一个凭借字符串
function url(protocol,host,port){
    return `${protocol}${host}${port}`;
}
const urlAns = url('http://','127.0.0.1:','8000');
console.log(urlAns);
/**
 * 这种结果的弊端就是当大量的网址的时候，
 * 前面的内容都一样，只是修改后面的端口号
 * 但是却不得不重复输入'http://','127.0.0.1:'
*/

//柯里化函数一
function urlCurring(protocol,host){
    return function(port){
        return `${protocol}${host}${port}`;
    }
}
const urlProandHost = urlCurring('http://','127.0.0.1:');

const p8000 = urlProandHost('8000');
const p8080 = urlProandHost('8080');
```


