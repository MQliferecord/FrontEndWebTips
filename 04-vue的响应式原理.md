# vue2的响应式原理

# vue3的响应式原理

从**01**可以知道vue3对于单数据形式是采用ref将其转化为RefImpl对象，对于对象则是使用reactive将其转化为一个Proxy对象相应的，触发响应式我们需要

```javascript
    let name = ref("张三");
    let age = ref(18);
    let props = reactive({
        prop1:"前端工程师",
        prop2:"30k"
    })

    function changeName(){
        name.value = "李四";
        age.value = 18;
        props.prop1 = "UI设计师";
        props.prop2 = "60k";
    }

    return{
        name,
        age,
        props,
        changeName
    }
```

但这样未免规则太过于繁复，可以将所有属性全部归为一个对象，对其整体进行reactive


```javascript
    let person = reactive({
        name:"张三"，
        age:18,
        props:{
            prop1:"前端工程师"，
            prop2:"30k"
        }
    })

    function changeName(){
        person.name = "李四";
        person.age = 18;
        person.props.prop1 = "UI设计师";
        person.props.prop2 = "60k";
    }

    return{
        person,
        changeName
    }
```

- 现在让我们看一下reactive是怎么构建proxy对象。

首先，proxy并不是Vue3独特的一个属性，它是es6定义的，window所带的一个函数。

下面让我们简单模拟一下vue3使用Proxy实现响应式原理的过程：

```JavaScript
    const p = new Proxy(person,{
        get(target,propName){
            return Reflect.get(target,propName)
        },
        //set操作不但完成了newVal和oldVal的响应式，还实现了add操作的响应式
        set(target,propName,value){
            Reflect.set(target,propName,value)
        },
        deleteProperty(target,propName){
            return Reflect.deleteProperty(target,propName)
        }
    })
```

从上面可以看出vue3在响应式上面一个显著的改变就是引入了Proxy这项meta programming技术。

原本在vue2中**vue的object.defineProperty本身是无法监视数据的add和delete**，因为是属于es5的语法，所以对于vue的处理方法是额外封装了**vm.$set**和**vm.$delete**API去弥补object.defineProperty函数的缺陷，但是Proxy却可以整体完成全部的响应式过程。