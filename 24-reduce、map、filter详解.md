# reduce、map、filter详解

## Array.reduce详解

作用：对数组中的每一个元素按照顺序执行自定义的`reducer()`函数，并且每次运行`reducer()`，都将先前的结果作为参数返回，最后将结果汇总为单一一个返回值返回一个单一返回值。

另外如果有输入`initval`，那么将initval作为`reducer()`第一个操作的初始值，不然将数组的第一个元素作为`reducer()`的初始值。

这里的reducer()函数只要接收4个参数：

1. Accumulator(acc)累加器

2. Current Value(cur)当前值

3. Current Index(idx)当前索引

4. Source Array(src)(源数组)

先看一个实例：

```javascript
const arr = [1,2,3,4];
const initval = 1;
//initval+arr[0]+arr[1]+arr[2]+arr[3]
const sum = arr.reduce(
    (acc,curVal)=>acc+curVal,
    initval
)
console.log(sum)//11
```

Array.reduce()的作用：

- 求数组和

```javascript
let sum = [1,2,3,4].reduce(function(prevVal,curVal){
    return prevVal+curVal;
},0);
```

- 累加对象的值

```javascript
let initVal = 0;
let sum = [{x:1},{x:2},{x:3}].reduce(function(prevVal,curVal){
    //注意这里的prevVal并没.x，因为它是一个之前reducer()计算的返回值
        return prevVal+curVal.x;
    },initVal);
```

- 将二维数组转化为一维数组

```javascript
let flattened = [[0,1],[2,3],[4,5]].reduce(
    function(prevVal,curVal){
        return preVal.concat(curVal)
    },[]
)
```

- 计算数组中的每个元素出现的次数

```javascript
let names = ['Alice','Bob','Tiff','Bruce',' Alice'];

let countedNames = names.reduce(function(allNames,name){
    if(name in allNames){
        allNames[name]++;
    }else{
        allNames[name] = 1;
    }
    return allNames;
},{})
```

- 按属性对object分类

```javascript
let people = [
    {name:'Alice',age:21},
    {name:'Max',age:20},
    {name:'Jane',age:20}
];

function groupBy(objectArray,property){
    return objectArray.reduce(function(acc,obj){
        let key = obj[property]
        if(!acc[key]){
            acc[key] = []
        }
        acc[key].push(obj)
        return acc;
    })
}

let groundPeople = groupBy(people,'age');
/** groupedPeople is:
 {
   20: [
     { name: 'Max', age: 20 },
     { name: 'Jane', age: 20 }
        ],
   21: [{ name: 'Alice', age: 21 }]
 }
```

- 数组去重

```javascript
let myArray = ['a','b','a','b','c','e','e','c','d','d','d','d'];
let myArrNoDup = myArray.reduce(function(prevVal,curVal){
    //这是关键的去重的代码  prevVal.indexOf(curVal) === -1
    if(prevVal.indexOf(curVal) === -1){
        prevVal.push(curVal);
    }
    return prevVal;
},[])

console.log(myArrNoDup);
```

- 手撕reduce()代码：

从上面的例子可以就看出来有一个数组去调用`reduce`，输入函数以及初始值，需要对是否有输入具体初始值做判断，然后确定函数操作的起始值。

```javascript
Array.prototype.myReduce = function(fn,initVal){
    //一般手撕最好是增加一个判断条件
    if(initVal === undefined && !this.length){
        throw new Error('myReduce of empty array with no inital value');
    }

    let res = initVal ? initVal:this[0];
    for(let i = initVal? 0:1;i<this.length;i++){
        /**
         * @res res指的是先前计算的值
         * @this 表示调用myReduce的数组元素
         * @i  数组的index
        */
        res = fn(res,this[i],i,this);
    } 
    
    return res;
}
```

## Array.map详解

`map()`方法创建一个新数组，新数组的每个元素都由原数组的元素调用函数后的结果组成。

`map()`只会对当前数组元素进行处理，并且会返回一个新数组，并不会影响原数组，如果你并不希望得到一个全新的数组，请使用`forEach`或者是`for...in...`

```javascript
const arr = [1,2,3,4]
const _map = arr.map(x=>x*2)
console.log(mao)
```

- 求数组每个元素的平方根

```javascript
const numbers = [1,4,9];
const ans = numbers = numbers.map(num=>Math.sqrt(num));
```

- 使用map重新格式化数组中的对象

```javascript
const kvArray = [
    {key:1,value:10},
    {key:2,value:20},
    {key:3,value:30}
]

const reformattedArray = kvArray.map(({key,value})=>({[key]:value}))
```

- 手撕Array.map()代码：

从上面可以看出来，`map()`主要是对于输入的数组进行元素的遍历，然后返回遍历后的结果。

```javascript
Array.prototype.myMap = function(fn){
    let res = []
    this.forEach((item)=>{
        res.push(fn(item))
    })
    return res
}
```

## Array.filter详解

`filter()`会返回一个新数组，新数组仅仅包括了满足输入函数条件的元素。

```javascript 
const words = ['spray','limit','elite','exuberant']
const result = word.filter(word=>word.length>6)
console.log(result)//'exuberant'
```

- 筛选排除所有较小的值

```javascript
function isEnough(value){
    return value >= 10
}
const filtered = [12,5,8,130,44].filter(isEnough);
```

- 找到数组中所有的素数

```javascript
const array = [-3,-2,-1,0,1,2,3,4,5,6,8,9,12,11,17]

function isPrime(num){
    for(let i = 2;num>i;i++){
        if(num%i === 0){
            return false;
        }
    }
    return num>1;
}
console.log(array.filter(isPrime));
```

- 在数组中搜索

```javascript
const fruits = ['apple','banana','grapes','mango','orange'];

function filterItems(arr,query){
    return arr.filter(el=>el.toLowerCase().includes(query.toLowerCase()));
}
console.log(filterItems(fruits,'ap'));
```

- 手撕Array.filter代码：

```javascript
Array.prototype.myFilter = function(fn){
    let arr = []
    this.forEach(item=>{
        if(fn(item)){
            arr.push(item)
        }
    })
    return arr
}
```










