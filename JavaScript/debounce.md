防抖：
一段时间内，事件在我们规定的间隔n秒内触发多次，回调只会执行一次。

特点：
* 持续触发不执行
* 不触发的一段时间之后才执行

```html
<input type="text" id="input">
```
匿名函数版本：

补充～

https://www.educative.io/answers/how-to-use-the-debounce-function-in-javascript

箭头函数版本：
```Javascript
// 防抖包装
function debounce(fn,timer){
    let timeoutID = null;
    return function (value){
        // 如果timeoutID已经存在，说明上次已经触发，重新计时
        if(timeoutID){
            clearTimeout(timeoutID);
        }
        timeoutID = setTimeout(()=>{
            fn.call(this,value)
        },timer)
    }
}
```

第一次立即执行版本：

补充～


```JavaScript
// 调用接口查询
function search(){
    console.log('ajax search');
}

const dom = document.querySelector('#input');

const debounceFn = debounce(search,1000);

dom.addEventListener('input',(e)=>{
    debounceFn(e.target.value)
})
```