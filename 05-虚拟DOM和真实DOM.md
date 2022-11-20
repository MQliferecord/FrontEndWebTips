# 什么是虚拟DOM和真实DOM？

虚拟DOM和真实DOM的关系很像是建筑图纸和真实的建筑物。

虚拟DOM的优点在于，如果每次网页的节点发生变化就刷新网站的整体真实DOM树，将会带来浏览器巨大的负荷开销，降低浏览器的运行速度。由此发明了虚拟DOM，在每次进行网站刷新前，会对比虚拟DOM中每个节点的newVal和oldVal，然后把其中变化的部分通知浏览器，浏览器只需要重新渲染发生变化的节点。

- 一个简单的虚拟DOM树的例子

- 渲染函数版本

```javascript
    function render(){
        return h('div',[
            h('div',[
                h('span','hello')
            ])
        ])
    }
```

- JSONDOM树版本

```javascript
    const vdom = {
        tag:'div',
        children:[
            {
                tag:'div',
                children:[
                    {
                        tag:'span',
                        children:'hello'
                    }
                ]
            }
        ]
    }
```

可以看到虚拟DOM往往通过节点/JSON的方式表达网页。


- 真实DOM

```html
    <div>
        <div>
            <span>hello</span>
        </div>
    </div>
```

可以看到真实的DOM树往往是html代码。