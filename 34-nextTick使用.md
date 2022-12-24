# nextTick

### nextTick的作用

在下次DOM更新循环结束后执行延迟回调。在修改数据之后立即使用该方法获取更新后的DOM。可以使得程序员直接操作真实DOM元素。虽然vue的理念不建议直接操作DOM，但存在一些特殊情况比如获得scrollTop,clientHeight,Offset等等操作。

> 如何使用nextTick?

两种使用方法，一种是传入回调，一种是使用Promise

```javascript
vm.msg = 'Hello'
//回调
Vue.nextTick(()=>{})

//promise()
Vue.nextTick().then(()=>{})
```