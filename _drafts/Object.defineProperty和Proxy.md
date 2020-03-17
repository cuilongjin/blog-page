---
title: Object.defineProperty和Proxy 对比
date: 2019/12/2
---

Object.defineProperty
的作用就是劫持一个对象的属性,通常我们对属性的 getter 和 setter 方法进行劫持,在对象的属性发生变化时进行特定的操作

```js
const obj = {};
Object.defineProperty(obj, 'text', {
  get: function() {
    console.log('get val');&emsp;
  },
  set: function(newVal) {
    console.log('set val:' + newVal);
    document.getElementById('input').value = newVal;
    document.getElementById('span').innerHTML = newVal;
  }
});

const input = document.getElementById('input');
input.addEventListener('keyup', function(e){
  obj.text = e.target.value;
})
```

Object.defineProperty 只能劫持对象的属性,无法监听数组变化

Proxy 可以直接监听对象而非属性

```js
const input = document.getElementById('input')
const p = document.getElementById('p')
const obj = {}

const newObj = new Proxy(obj, {
  get: function(target, key, receiver) {
    console.log(`getting ${key}!`)
    return Reflect.get(target, key, receiver)
  },
  set: function(target, key, value, receiver) {
    console.log(target, key, value, receiver)
    if (key === 'text') {
      input.value = value
      p.innerHTML = value
    }
    return Reflect.set(target, key, value, receiver)
  }
})

input.addEventListener('keyup', function(e) {
  newObj.text = e.target.value
})
```

Proxy 可以直接监听数组的变化
Proxy 有多达 13 种拦截方法
Proxy 返回的是一个新对象,我们可以只操作新的对象达到目的,而 Object.defineProperty 只能遍历对象属性直接修改
Proxy 的劣势就是兼容性问题,而且无法用 polyfill
