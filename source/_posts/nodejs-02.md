`1. NPX: npm package extention` npm 从5.2开始增加了npx命令

npx 有许多优点：

1. npx 执行某些第三方包的命令时，如果本地和全局npm都没有安装这个第三方包时，npx 会自动取安装这个包到临时文件夹中，用后自己删除，不会污染你的环境！！！

2. npx 也可以设置 是使用本地的还是远程下载 ` --no-install 参数和 --ignore-existing 参数`

3. 如果想让 npx 强制使用本地模块，不下载远程模块，可以使用--no-install参数。如果本地不存在该模块，就会报错。

   ```
   $ npx --no-install http-server
   ```

   反过来，如果忽略本地的同名模块，强制安装使用远程模块，可以使用--ignore-existing参数。比如，本地已经安装了http-server，但还是想使用远程模块，就用这个参数。

   ```
   $ npx --ignore-existing http-server
   ```

   

   ## 在chrome浏览器中debug node 代码！！！

   `node 执行 某个js文件时，可以加上 --inspect --inspect-brk 这个参数，表示开启调试 ex: node --inspect -inspect-brk test.js`

   ![image-20210928092131399](D:\phpstudy_pro\WWW\my-hexo-blog\source\_posts\image-20210928092131399.png)

![image-20210928092608769](D:\phpstudy_pro\WWW\my-hexo-blog\source\_posts\image-20210928092608769.png)

最后在对应位置打断点就可以进行调试了！！！



## node 进程管理工具

- supervisor
- nodemon (本地调试！) 全局安装后 代替node 执行脚本，脚本修改自己会重新启动！
- forever
- pm2 (*)





## express 脚手架 快速搭建一个 环境！！！

npm i express express-generator -g



`之后输入 express -e 命令 默认就会生成一个空的express 项目，项目默认端口为3000，`



vscode 插件 REST Client 发送 post 请求 请求 express 搭建的 api

M:

我们在VS Code新建一个以.http或者.rest结尾的文件，填入你的HTTP请求，点击Send Request，或者右键选择Send Request，或者直接用快捷键 Ctrl+Alt+R ，你的REST API就执行了，然后API Response就会显示在右边区域。

![image-20210928144657744](D:\phpstudy_pro\WWW\my-hexo-blog\source\_posts\image-20210928144657744.png)

![image-20210928144845869](D:\phpstudy_pro\WWW\my-hexo-blog\source\_posts\image-20210928144845869.png)



## nodeJS 跨域



### jsonp

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>jsonp</title>
</head>
<body>
  <script>
    function getJsonData (data) {
      console.log(data)
    }
  </script>
  <script src="http://localhost:8080/api/getdata?callback=getJsonData"></script>
</body>
</html>

《js》
/**
 * jsonp原理： 利用了浏览器不限制具有 src 属性的标签的跨域。 创建一个script标签，src属性 指向一个不同源的网址或者w接口，
 * 并提供一个回调函数来接收数据（函数名可约定，或通过地址参数传递）。     
 * 第三方产生的响应为json数据的包装（故称之为jsonp，即json padding），形如：     callback({"name":"hax","gender":"Male"})  
 * 这样浏览器会调用callback函数，并传递解析后json对象作为参数。本站脚本可在callback函数里处理所传入的数据。
 */
const http = require('http')
const url = require('url')
const querystring = require('querystring')
const server = http.createServer((req, res) => {
  const urlstr = url.parse(req.url, true)
  // 第二个参数 true 表示 生成 query 参数  get请求的参数会以对象形式存放于 query中
  // console.log(urlstr)
  switch (urlstr.pathname) {
    case '/api/getdata':
      res.write(`${urlstr.query.callback}("hello")`)
      break;
    default:
      res.write('404 not found')
      break;
  }
  res.end() // ！important
})

server.listen(8080, () => {
  console.log('server run on 8080 ')
})
```

### CORS

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>cors 跨域</title>
</head>
<body>
  <script>
    fetch('http://localhost:8080/api/data').then(response => response.json()).then(result => {
      console.log(result)
    })
  </script>
</body>
</html>

《js》
const http = require('http')
const url = require('url')

const server = http.createServer((req, res) => {
  const u = req.url
  const urlstr = url.parse(u)
  switch (urlstr.pathname) {
    case '/api/data':
      res.writeHead(200, {
        'Content-Type': 'application/json',
        'Access-Control-Allow-Origin': '*'
        //  这里是关键！！！
      })
      res.write(`{"type":true, "data": "hello"}`)
      /* json 格式  "a": "a"  */
      break;
    default:
      res.write('404 not found')
      break;
  }
  res.end()
})
server.listen(8080, () => {
  console.log('server listening on port 8080')
})

```

### 设置代理跨域（middle ware 中间件）

```
const http = require('http')
const url = require('url')
const { createProxyMiddleware } = require('http-proxy-middleware')
// 使用 http-proxy-middleware 来辅助跨域！！
const server = http.createServer((req, res) => {
  const urlStr = req.url
  // console.log(urlStr)
  const options = {
    target: 'https://mapi.vip.com',
    changeOrigin: true,
    pathRewrite:{ // 路径转发  请求时 /api 用于nodejs拦截， 拦截后，将/api 这个url部分 转发为 空
      // http://localhost:8080/api/vips-mobile/rest/layout/pc/channel_b/data 【vscode 里面使用 REST Client 来发请求！！！】
      // 这样请求上面接口 实际就是在请求  http://localhost:8080/vips-mobile/rest/layout/pc/channel_b/data
      '^/api': ''
    }
  }
  if (/\/api/.test(urlStr)) {
    const proxy = createProxyMiddleware('/api', options)
    proxy(req, res)
  }else {
    console.log('test url not found')
  } 
})
server.listen(8080, () => {
  console.log('server listening on port 8080')
})
 /* 关于 http 状态码 
      4xx: 基本可以认定为前端错误
      5xx:基本可以认定为后端错误
*/

```

