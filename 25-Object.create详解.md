# Object.create详解

-用于创建一个新的对象，使用现有的对象来作为新创建对象的原型（prototype）。`object = Object.create(object)`，注意输入只能是`object`或者是`NULL`，不能是基础数据类型。Object.create()能够实现对象的浅拷贝。

```javascript
const person = {
    isHuman:false,
    printIntroduction:function(){
        console.log(`My name is ${ this.name}. Am I  human? ${this.isHuman}`);
    }
}
const me = Object.creat(person);
me.name = 'MQ';
me.isHuman = true;
me.printIntroduction();

const prevObj = {
    name:'MQ',
    hobbies:['coding','reading','genshin'],
    getHobbies:function(){
        let res = ''
        for(let i = 0;i<this.hobbies.length;i++){
            res += this.hobbies[i]+' '
        }
        let ans = res.slice(0,res.length-1);
        return ans
    }
} 

const copyObj = Object.create(prevObj);
console.log(copyObj.getHobbies());//coding reading genshin

copyObj.hobbies.push('Gaming');
console.log(prevObj.hobbies);//['coding','reading','genshin','Gaming']
```

- 使用`Object.create()`实现类式继承

```javascript
function Shape(){
    this.x = 0;
    this.y = 0;
}

Shape.prototype.move = function(x,y){
    this.x += x;
    this.y += y;
    console.info('Shape moved');
}

function Rectangle(){
    Shape.call(this);
}
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle();

const rect = new Rectangle();
console.log(rect instanceof Rectangle);//true
console.log(rect instanceof Shape);//true
```

- Object.create实现

从上面的例子可以看出来Object.create可以实现对象的浅拷贝，并且只能是`object`或者`null`.

```javascript
const myCreate = proto =>{
    if(typeof proto !== 'object' && typeof proto !== null){
        throw new Error('input not object or null')
    }
    return Object.assign(proto);
}
```


