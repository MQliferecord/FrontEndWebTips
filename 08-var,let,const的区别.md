# var,let,const 的区别

### var
- 全局变量
- 具有函数作用域和全局作用域
- 具有变量提升。

变量提升的例子：

```javascript
    var num;
    console.log(num);//10
    num = 10;
```

因为var变量提升的特性，往往会导致以下的一些特殊情况:

```javascript
    for(var num = 0;num<3;num++){
        console.log("for:"+num);//0 1 2
        setTimeout(()=>{console.log("setTimeout:"+num)},1000);//3 3 3
    }
    console.log("外部："+num);//3
```

在let和const还没有引入js的时候往往对于限制var的全局作用域的特性，去大量使用函数作用域的闭包。

```javascript
    (function(){
        for(var num = 0;num<3;num++){
            console.log("for:"+num);//0 1 2
        }
    })();
    console.log(num);//报错
```

### let
- 局部变量
- 可变更指向的内存地址
- 具有块作用域，函数作用域，全局作用域
- 不存在变量提升，存在暂时性死区
- 创建变量后如果不立即进行赋值，词法环境会自动赋值undefined

```javascript
    for(let num = 0;num<3;num++){
        console.log(num);//0 1 2
        setTimeout(()=>{console.log(num)},1000);//0 1 2
    }
    console.log(num);//不需要闭包也会报错
```

```javascript
    let num;
    console.log(num);//undefined
    num = 10;
```

### const
- 局部变量
- 不可变更指向的内存地址
- 具有块级作用域，函数作用域，全局作用域
- 不存在变量提升，存在暂时性死区
- 创建变量后如果不立即进行赋值，词法环境会自动赋值undefined

```javascript
    const num = 10;
    num = 20;//报错
    const obj = {name:"lili"};
    obj = {name:"lisa"};//报错
    obj.name = "lisa";//运行成功
    obj.age = 10;//运行成功
```