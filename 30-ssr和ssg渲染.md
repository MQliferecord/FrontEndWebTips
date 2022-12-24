### 服务端渲染（SSR）

服务端渲染是指，在服务端实现HTML页面的绘制，然后直接将绘制好的页面发送给浏览器端。然后再在浏览器端绑定事件和状态，增加交互能力。

SSR主要应用于两种场景：

1. SEO，因为SEO无法等待异步数据，如果页面是异步请求，可以使用SSR直接发送页面。

2. 网络较慢和设备运行缓慢时需要更快的到达时间，可以使用SSR渲染

SSR的缺点：

1. 运行环境单一，SSR一般是Nodejs上进行部署，对于JAVA等后端环境不支持。

2. 对缓存要求更高，像是Nodejs的Express框架下views里放置了jade/ejs等写死的一些SSR页面，需要去占据后端文件的内存。

3. 兼容性处理，在第三方引用时可能需要去进行一些兼容性的设置。

### Vue服务端渲染的基本实现

```javascript
const Vue = require('vue');
const express = require('express');
const renderer = require('vue-server-renderer').createRenderer();

express.get('*',(req,res)=>{
    const app = new Vue({
        data:{
            url:req.url
        }
        template:'<div>The visited url is：{{url}}</div>';
    })
})
renderer.renderToString(app,(err,html)=>{
    if(err){
        throw new Error(err)
    }
    res.end(`
        <!DOCTYPE html>
        <html lang="en">
            <head><title>Hello</title></head>
            <body>${html}</body>
        </html>
    `)
})
server.listen(8000,()=>{
    console.log("8000端口已启动")
});
```