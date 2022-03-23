commonjs 是nodejs的一个规范，并且是被nodejs认可的一个规范。

commonjs 规范默认浏览器不支持！！！

commonjs 规范大致可以分为四个步骤



1. 书写模块中的属性和方法

```javascript
const obj = {
    name:'zs',
    sayHello () {
    	console.log('hello')
	}
}
```



1. 暴露属性或者方法给外部

```javascript
module.exports = obj
或者
module.exports = {
    obj
}
或者
exports.obj = obj 这里 exports 相当于module.exports的引用
```



1. 需要用的时候引入具体模块
2. 调用暴露的属性或者方法

commonJS模块 和 es6模块化  区别

| a    | b    |
| ---- | ---- |
|      |      |

