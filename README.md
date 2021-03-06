# Event
自定义事件订阅/发布器 (依赖 jQuery.Callbacks)

简单用法：

```javascript
var evt = new Event()

// 监听
evt.on('xx', function(a, b, c){
    console.log(a)
})

evt.one('xx', function(){
    console.log('once')
})

evt.on('xx', listenerA)

function listenerA (a, b, c){
    console.log(b)
}

// 触发
evt.emit('xx', 666, 2333)

// 取消监听
evt.off('xx', listenerA)    // 单个
evt.off('xx')    // 整个
evt.off()    // 所有（慎用）
```

高级用法：

已触发过的事件会被记录，数据也会缓存起来（仅最近一次），之后调用 on 方法进行监听时，如果第三个参数传入true，回调将会被立即调用

```javascript
var evt = new Event()

evt.emit('xx', 666)

evt.on('xx', function(a){
    console.log(a)    // => 666
}, true)
```

可以借此判断事件是否触发过
```javascript
var loaded = false

evt.on('load', function(a){
    loaded = true
}, true)

if (loaded) {
    // do something
}
```
监听所有事件。`ALL`事件是一个特殊的事件，当有任何事件触发时，它也会触发：
```javascript
evt.on('ALL', function(name, data){
    console.log(name, data)
})

evt.emit('xx', 'Hello')    // => xx Hello
```

作为混入类使用也是没有问题的

``` javascript
var TT = {}

Object.assign(TT, new Event())

TT.on('xx', function(){
    console.log(1)
})

TT.emit('xx')
```

