# instanceof详解

作用： 用于检测**构造函数的prototype**是否在**实例对象的原型链**上。`object instanceof constructor`，其中`object`必须是实例对象，`constructor`必须是构造函数或者构造函数实例对象。

看一下来自于MDN关于instanceof的示例：

```javascript
function Car(make,model,year){
    this.make;
    this.model;
    this.year = year;
}
    
const auto = new Car('Honda','Accord',1998);
console.log(auto instanceof Car);
console.log(auto instanceof Object);
```

更详细的示例：

```javascript
function C(){}
function D(){}

let o = new C()

console.log(o instanceof C)//true
console.log(o instanceof D)//false
console.log(o instanceof Object)//true
console.log(C.prototype instanceof Object)//true
console.log(o.__proto__ instanceof C)//false
console.log(o.__proto__ instanceof C.prototype)//报错，因为C.prototype是一个实例对象，不是函数
console.log(o.__proto__ instanceof Object)//true
console.log(o.__proto__ instanceof Object.prototype)//报错，原因如上

C.prototype = {}
let o2 = new C()

console.log(o2 instanceof C)//true
console.log(o instanceof C)//false,因为C的原型链已经发生了变动

D.prototype = new C()
let o3 = new D()
console.log(o3 instanceof D)//true
console.log(o3 instanceof C)//true
```

从上述的实例和相关的定义，可以知道，当要实现`instanceof`的功能不仅需要，验证object是否是构造函数constructor的实例，还需要验证，object的原型链是否存在constructor。

```javascript
const myinstanceof = (target,func)=>{

    if(target !== 'object' && target === null){
        throw new Error('target is not an `object` prototype');
    }
     
    if(target.__proto__ === func.prototype){
        return true;
    }else{
        let proto = func.prototype;
        if(proto){
            myinstanceof(target.__proto__,proto);
        }else{
            return false;
        }
    }
}
```


