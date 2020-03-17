---
title: 常用 js 代码段汇总
categories:
  - [js]
date: 2019/07/22
updated: 2019/10/24
---

## 类型识别

### 判断一个变量是字符串

```js
Object.prototype.toString.call('str') // '[object String]'
typeof 'str' // 'string'
```

### getRawType：获取数据类型，返回结果为 Number、String、Object、Array 等

```js
function getRawType(value) {
  return Object.prototype.toString.call(value).slice(8, -1)
}
```

### isObject：判断数据是不是引用类型的数据

例如： arrays, functions, objects, regexes, new Number(0),以及 new String('')

```js
function isObject(value) {
  let type = typeof value
  return value != null && (type == 'object' || type == 'function')
}
```

### isPlainObject：判断数据是不是 Object 类型的数据

```js
function isPlainObject(obj) {
  return Object.prototype.toString.call(obj) === '[object Object]'
}
```

### isArray：判断数据是不是数组类型的数据

```js
function isArray(arr) {
  return Object.prototype.toString.call(arr) === '[object Array]'
}
```

### 将 isArray 挂载到 Array 上

```js
Array.isArray = Array.isArray || isArray
```

### isRegExp：判断数据是不是正则对象

```js
function isRegExp(value) {
  return Object.prototype.toString.call(value) === '[object RegExp]'
}
```

### isDate：判断数据是不是时间对象

```js
function isDate(value) {
  return Object.prototype.toString.call(value) === '[object Date]'
}
```

## 格式转换

### 数字格式化：小于10的数值前面加上0

```js
/**
 * @param {number} num 要格式化的数值
 * @return {string} 把小于10的数值前面加上0
 */
function prefix_zero(num) {
  return num >= 10 ? num : '0' + num
}
```

### 价格格式化 (1234567 => 1,234,567.00)

```js
/**
 * @param {number} price 价格
 * @returns {string} 1234567 => 1,234,567.00
 */
function formatPrice(price) {
  if (typeof price !== 'number') return price
  return String(Number(price).toFixed(2)).replace(/\B(?=(\d{3})+(?!\d))/g, ',')
}
```

### 手机号格式化：隐藏中间四位数字

```js
/**
 * @param {string} mobile 手机号
 * @returns {string}
 */
function formatMobile(mobile) {
  mobile = String(mobile)
  if (!/\d{11}/.test(mobile)) {
    return mobile
  }
  return mobile.replace(/(\d{3})\d{4}(\d{4})/, '$1****$2')
}
```

### 进制转换

parseInt(str,radix) // 任意进制转换为 10 进制整数值
与 Number.toString(radix) 返回表示该数字的指定进制形式的字符串

## 常用正则

```js
// 任意正负数字
let reg = /(^[\-0-9][0-9]*(.[0-9]+)?)$/

// 匹配邮箱
let reg = /^([a-zA-Z]|[0-9])(\w|\-)+@[a-zA-Z0-9]+\.([a-zA-Z]{2,4})$/

// 匹配手机号
let reg = /^1[0-9]{10}$/;

// 匹配8-16位数字和字母密码的正则表达式
let reg = /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{8,16}$/;

// 匹配国内电话号码 0510-4305211
let reg = /\d{3}-\d{8}|\d{4}-\d{7}/;

// 匹配身份证号码
let reg=/(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/;

// 匹配腾讯QQ号
let reg = /[1-9][0-9]{4,}/;

// 匹配ip地址
let reg = /\d+\.\d+\.\d+\.\d+/;

// 匹配中文
let reg = /^[\u4e00-\u9fa5]*$/
```

## 检测平台（设备）类型

```js
isWechat = /micromessenger/i.test(navigator.userAgent)
isWeibo = /weibo/i.test(navigator.userAgent)
isQQ = /qq\//i.test(navigator.userAgent)
isIOS = /(iphone|ipod|ipad|ios)/i.test(navigator.userAgent)
isAndroid = /android/i.test(navigator.userAgent)
```

## 敏感符号转义

```js
function entities(s) {
  let e = {
    '"': '&quot;',
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;'
  }
  return s.replace(/["<>&]/g, m => {
    return e[m]
  })
}
```

## 数组去重

```js
function distinct(arr) {
  return arr.filter((v, i, array) => array.indexOf(v) === i)
}
```
