---
title: 手写JavaScript深浅拷贝
tags:
  - 前端
  - JavaScript
abbrlink: 31c8a771
date: 2022-09-13 23:44:43
---
  - 浅拷贝只复制指向某个对象的指针，而不复制对象本身，新旧对象还是**共享同一块内存**
  - 深拷贝会另外创造一个一模一样的对象，新对象跟原对象不共享内存，**修改新对象不会改到原对象**
<br>
<!-- more -->
  
下图看原理
{% asset_img difference.png  %}


## 1.浅拷贝

在JavaScript中，存在浅拷贝的对象：
`Object.assign`
`Array.prototype.slice(), Array.prototype.concat()`
`使用拓展运算符实现的复制`

```javascript
var shallowClone = function (obj) {
    // 只拷贝对象
    if (typeof obj !== 'object') return;
    // 根据obj的类型判断是新建一个数组还是对象
    var newObj = obj instanceof Array ? [] : {};
    // 遍历obj，并且判断是obj的属性才拷贝
    for(var prop in obj) {
        if(obj.hasOwnProperty(prop)) {
          newObj[prop] = obj[prop]
        }
    }
    return newObj
}

```

### 2.深拷贝
`lodash的_.cloneDeep()`
`jQuery.extend()`
`JSON.parse(JSON.stringify(obj1) ` * 该方法不能拷贝函数，undefined，symbol
```javascript
 // 递归调用就ok
var deepClone = function(obj) {
    // 只拷贝对象
    if (typeof obj !== 'object') return;
    // 根据obj的类型判断是新建一个数组还是对象
    var newObj = obj instanceof Array ? [] : {};
    // 遍历obj，并且判断是obj的属性才拷贝
    for(var prop in obj) {
        if(obj.hasOwnProperty(prop)) {
            newObj[prop] = typeof obj[prop] === 'object' ? deepCopy(obj[prop]) : obj[prop];
        }
    }
    return newObj
    
}

```


**参考文章**

[图解对象之：深拷贝与浅拷贝](https://mp.weixin.qq.com/s/IR0NILKFI7hR2_Z1mnFUQA)
[实现深拷贝的多种方式](https://mp.weixin.qq.com/s/F3dJqMR7em_GFPz4yaXeUQ) 
[你最少用几行代码实现深拷贝？](https://mp.weixin.qq.com/s/WufLPff129BYw4s9qToJ4g)  
[JavaScript专题之深浅拷贝](https://github.com/mqyqingfeng/Blog/issues/32)
