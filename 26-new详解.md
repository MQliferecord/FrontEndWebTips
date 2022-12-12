# new详解

在创建新对象的过程中，new操作符主要做了三件事：

1. 先创建一个空对象:`let son = {}`

2. 然后`son.__proto__  = Parent.prototype`

3. 最后改变属性的this指针指向：`Parent.call(son)`

因此我们可以尝试复现一下new的代码：

```javascript
    const myNew = function(Parent,arguments){
        const son = {};
        son.__proto__ = Parent.prototype;
        let ans = Parent.call(son,arguments);
        return ans instanceof Object? ans:son;
    } 
```