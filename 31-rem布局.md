# rem布局

> rem,em,px,vw,vh的区别是什么？

- 1px是固定一像素大小.

- em作为font-size的单位，代表父元素的字体大小的倍率，作为其他css单位，代表自身字体大小的倍率。

例如，父元素12px;

子元素font-size设置为2em,则子元素字体大小为24px;

子元素width设置为2em,则子元素width大小为48px.

- rem作为非根元素大小的单位，代表根元素的字体大小的倍率，作为根元素（html）大小的单位，代表初始根元素大小的倍率。

比如根元素font-size设置为12px,非根元素的font-size为2rem,则是24px.

**因此可以得出结论，rem比起em，布局更稳定。**

- vw/vh

vw:视窗宽度的1/100

vh:视窗高度的1/100

## rem布局

一般在项目中的`public/index.html`中引入：

```javascript
(function(doc,win){
    let docEl = doc.docmentElement,
    resizeEvt = 'onorientationchange' in window? 'onorientationchange':'resize',
    recalc = function(){
        let clientWidth = docEl.clientWidth;
        if(!clientWidth)return;
        if(clientWidth>=750){
            docEl.style.fontSize = '100px';
        }else{
            docEl.style.fontSize = 100*(clientWidth/750)+'px'
        }
    } 
    if(!doc.addEventListener) return;
    win.addEventListener(resizeEvt,recalc,false);
    doc.addEvebtListener('DOMContentLoaded',recalc,false);
})(document,window)
```