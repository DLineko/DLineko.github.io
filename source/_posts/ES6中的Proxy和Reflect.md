---
title: ES6的Proxy和Reflect
tags:
  - 前端
  - Vue源码衍生
abbrlink: b3a314df
date: 2020-05-30 00:14:18
---
# 1.Proxy 
在访问对象前添加一层拦截，过滤很多操作。 
### 1.1属性
>handler 其属性是操作对应的自定义代理函数
target 目标对象（可以是任何类型的对象，包括原生数组，函数，甚至另一个代理）
<!-- more -->
### 1.2语法
>const p = new Proxy(target, handler)
### 1.3使用场景
> 可以方便对传值的数组数据进行自动验证，进行错误抛出和页面联动。

***
### 1.4使用示例
①
```javascript
let targetObject = {
   name: "小名"
 };
 proxyName = new Proxy(targetObject, {
   get:(target,name,)=>{
       return "小字"
    }
 });
 console.log(proxyName.name);  // 小字
 // 本来是输出小名，由于使用了proxy进行代理，拦截读取该属性，因此打印出小字

```
getter只能对已知的属性键进行监听，而无法对所有属性读取行为进行拦截，而 Proxy 可以通过设定 get 监听方法，拦截和干涉目标对象的所有属性读取行为。
get(target,name,property)方法，用于拦截某个读取属性的操作,第一个参数为目标对象，第二个参数为属性名称，第三个属性为操作所针对的对象（可选参数）

②
```javascript
let targetObject = {
   name: "小名",
   age: 15
 };
 proxyName = new Proxy(targetObject, {
   get(target, key) {
     let result = target[key];
     if (key === "age") result += "岁"
     return result;
   },
   set(target, key, value) {
     if (key === "age" && typeof value !== "number") {
       throw Error("age字段必须为Number类型");
     }
     return Reflect.set(target, key, value);
   }
 });
 console.log(`${proxyName.name}今年${proxyName.age}了`); // 小名今年15岁了 --调用get
 proxyName.age = 18
 proxyName.age = '18' // Uncaught Error: age字段必须为Number类型
```
handler.set 用于监听目标对象的所有属性赋值行为
set(target,name,value,property),用于拦截某个属性的赋值操作，第一个参数为目标对象，第二个参数为属性名，第三个参数为属性值，第四个参数为操作行为所针对的对象（可选参数）。

### 1.5其他属性

属性值	 | 监听器参数 |  监听内容
-|-|-
has | (target, prop) | 监听 in 语句的使用
get | (target, prop, reciver) | 	监听目标对象的属性读取
set	 | (target, prop, value, reciver) | 	监听目标对象的属性赋值
deleteProperty	 | (target, prop)	 | 监听 delete 语句对目标对象的删除属性行为
ownKeys	 | (target)	 | 监听 Object.getOwnPropertyName() 的读取
apply	 | (target, thisArg, arguments)	 | 监听目标函数(作为目标对象)的调用行为
construct	 | (target, arguments, newTarget)	 | 监听目标构造函数(作为目标对象)利用 new 而生成实例的行为
getPrototypeOf | 	(target)	 | 监听 Objext.getPrototypeOf() 的读取
setPrototypeOf	 | (target, prototype)	 | 监听 Objext.setPrototypeOf() 的调用
isExtensible | 	(target) | 	监听 Objext.isExtensible() 的读取
preventExtensions	 | (target)	 | 监听 Objext.preventExtensions() 的读取
getOwnPropertyDescriptor | 	(target, prop) | 	监听 Objext.getOwnPropertyDescriptor() 的调用
defineProperty	 | (target, property, descriptor)	 | 监听 Object.defineProperty() 的调用

# 2.Reflect
1.将Object对象属于语言内部的方法映射到放到reflect对象上
2.将老对象方法报错的情况，改为false
3.与Proxy相辅相成，Reflect拥有Proxy上的方法
### 2.1 使用方法
```javascript
// 老写法
try {
  Object.defineProperty(target, 'foo', { value: 'bar' })
  // yay!
} catch (e) {
  // oops.
}
// 新写法
var yay = Reflect.defineProperty(target, 'foo', { value: 'bar' })
if (yay) {
  // yay!
} else {
  // oops.
}

```
这样，我们避免了try/ catch块，并使我们的代码在过程中更具可维护性。
```javascript
// 老写法
var target = { foo: 'bar', baz: 'wat' }
delete target.foo
console.log(target)
// <- { baz: 'wat' }
// 新写法
var target = { foo: 'bar', baz: 'wat' }
Reflect.deleteProperty(target, 'foo')
console.log(target)
// <- { baz: 'wat' }

```
就像deleteProperty一样，有一些其他的方法可以很容易地执行其他操作。

有了<code>Reflect对象</code>以后，很多操作会更易读。
```javascript
// 老写法
Function.prototype.apply.call(Math.floor, undefined, [1.75]) // 1

// 新写法
Reflect.apply(Math.floor, undefined, [1.75]) // 1

```
### 2.2 Reflect对象的常用方法
属性值 | 监听器参数 | 使用方法
-|-|-
get | target, name, receiver | 查找并返回target对象的name属性，如果没有该属性，则返回undefined。如果name属性部署了读取函数，则读取函数的this绑定receiver。
set | target, name, value, receiver | 设置target对象的name属性等于value。如果name属性设置了赋值函数，则赋值函数的this绑定receiver。
has | obj, name | 等同于name in obj
deleteProperty | obj, name | 等同于delete obj[name]
construct | target, args | 等同于new target(...args)，这提供了一种不使用new，来调用构造函数的方法
getPrototypeOf | obj | 读取对象的__proto__属性，对应Object.getPrototypeOf(obj)
setPrototypeOf | obj, newProto | 设置对象的__proto__属性，对应Object.setPrototypeOf(obj, newProto)
apply | fun,thisArg,args | 等同于Function.prototype.apply.call(fun,thisArg,args)。一般来说，如果要绑定一个函数的this对象，可以这样写fn.apply(obj, args)，但是如果函数定义了自己的apply方法，就只能写成Function.prototype.apply.call(fn, obj, args)，采用Reflect对象可以简化这种操作

### 2.3 注意地方
<p style="color:#f5871f">
Reflect.set()、Reflect.defineProperty()、Reflect.freeze()、Reflect.seal()和Reflect.preventExtensions()返回一个布尔值，表示操作是否成功。它们对应的Object方法，失败时都会抛出错误
</p>

```javascript
// 失败时抛出错误
Object.defineProperty(obj, name, desc);
// 失败时返回false
Reflect.defineProperty(obj, name, desc);
```

