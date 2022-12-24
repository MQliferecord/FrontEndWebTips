# vuex的基本用法

> 什么是vuex？

所谓的Vuex是一个为了Vue.js设计的数据仓库，将各个组件公用的数据放到一个仓库里面进行统一管理。

> vuex的优点？

1. 使得组件之间的数据共享变得更加简单

2. 将数据抽离出来使得程序维护更加简单

3. 仓库内的数据发生变化，其他组件引用的数据也会自动更新

### vuex的基本结构

vuex主要是包含了四个部分：state,mutations,actions,getters

- state:存放数据的地方。

- mutations:实际更改数据的地方，每个mutations都有一个字符串的事件类型（type）和回调函数（callbacks）,通常在回调函数中实现状态的更改，callbacks的第一个参数往往都是state。

- actions:接受更改参数的请求，并把请求派发给mutations，能够接收异步dispatch请求。

- getters:store的计算属性。

> 为什么使用actions和mutations，将变更数据变成两步？

答：mutations有同步限制，但是actions可以执行异步操作

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
    state:{
        test:'x'
    }
    mutations:{
        changetest(state,xxx){
            state.test = xxx
        }
    }
    actions:{
        changetest({commit},xxx){
            commit('changetest',xxx)
        }
        changetest2({dispatch,commit},xxx){
            await dispatch('changetest')
            commit('changetest2',xxx)
        }
    }
    getters:{
        test2:state=>state.test
    }
})
export default store
```

### vuex的使用

- 如何使用state

子组件能够通过`this.$store`访问到

```javascript
this.$store.state.test
```

- 如何使用mutations

子组件可以通过`this.$store.commit`访问得到

```javascript
this.$store.commit('changetest',xxx)
```

- 如何使用actions

子组件可以通过`this.$store.dispatch`访问 

```javascript
this.$store.dispatch('changetest',xxx)
```

- 如何使用getters

子组件能够通过`this.$store.getters`访问

```javascript
this.$store.getters.test2
```