# vue2和vue3的区别
### 打包工具
vue2往往用webpack进行打包，vue3往往使用vue团队开发的vite进行打包。
Vite相对于webpack的优点在于，vite的启动速度更快.
因为webpack运行过程是先有模板，然后解析成虚拟DOM，然后整体渲染真实DOM，最后创建一个端口号。
vite则是相反的过程，先创建端口号，然后根据目前用户访问的模块加载相应的组件，实现了按需加载的特色。

### script中的选项
vue2中组件所用到的属性方法等，被放置在data,methods,computed等等中，但是在vue3中全部被配置在setup中，不过vue3本身能兼容vue2的写法。
但是使用vue3是setup是无法访问computed,methods等等使用vue2写法的数据，所以最好不要混用。

### 响应式原理的实现
vue2的响应式原理是使用object.defineProperty实现，但是在vue3中则采用了ref和reactive实现响应式原理。
其中ref将单个数据转化为一个refImpl的对象，refImpl内封装了getvalue和setvalue方法,reactive将一个对象转化为Proxy对象。

### TypeScript的支持
vue2如果要使用TypeScript语法往往需要安装额外的插件，vue3则更好地支撑了TypeScript的使用。