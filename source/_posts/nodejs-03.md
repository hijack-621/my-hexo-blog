## 爬虫

nodejs也可以进行数据的抓取，而不单单时 python！！！

```
这里借助cherrio 这个第三方库来创建虚拟dom树，将获取到的html字符串转成一颗虚拟dom树，转换后，可以类似使用jquery操作dom的方式来操作生成的这个vnode tree
```

```
const http = require('http')
const url = require('url')
const https = require('https')
const cheerio = require('cheerio')

function filterData(data) {
 let $ = cheerio.load(data);
  $('.sub-container a').each((index, el) => { // 找到 虚拟node树中的 a  打印其文本内容！！！ 
    console.log($(el).html());
  })
}

const server = http.createServer((req, res) => {
  https.get('https://www.bilibili.com', (result) => {
    let data = ''
    result.on('data', (chunk) => {
      data += chunk
    })
    result.on('end', () => {
      filterData(data)
    })
  })
})
server.listen(8082, () => {
  console.log('server listening on port 8082')
})
 /* 关于 http 状态码 
      4xx: 基本可以认定为前端错误
      5xx:基本可以认定为后端错误
*/
```



## nodejs 内置模块-events

```
/** 
 * events 为nodejs 内置模块 用于事件的绑定和出发  有点类似 vue中的 订阅者发布模式
 * 该模块向外暴露的是一个类！！！
*/
const EventsEmitter = require('events')

class MyEventsEmitter extends EventsEmitter {

}
const e = new MyEventsEmitter();
e.on('print', (value) => { // 发布/绑定 事件 使用on 关键字发布的事件何以可以重复被出发【emit几次就执行几次】
  console.log(value)
})
// e.once('play', () => {}) 使用once关键字定义就只会被触发一次
/* 并且 可以重复绑定同一个 事件 ex: 
e.on('play', ()=>{})
e.on('play', ()=>{})
e.on('play', ()=>{})
重复绑定的 触发时，都可以一并被触发！！！
e.emit('play', {test:'test'}) 这里会打印三次  {test:'test'}
 */
e.emit('print', {test:'test'}) // c出发事件， 第二个参数为回传给 绑定事件的参数 
```

