---
title: vue 异步组件
categories:
  - [vue]
date: 2019/08/06
---

1. 使用 CDN 资源,减小服务器带宽压力
2. 路由懒加载
3. 将一些静态 js css 放到其他地方（如 OSS），减小服务器压力
4. 按需加载三方资源，如 iview,建议按需引入 iview 中的组件
5. 使用 nginx 开启 gzip 减小网络传输的流量大小
6. 若首屏为登录页，可以做成多入口，登录页单独分离为一个入口
7. 使用 uglifyjs-webpack-plugin 插件代替 webpack 自带 UglifyJsPlugin 插件

### 异步组件

[官方文档-异步组件](https://cn.vuejs.org/v2/guide/components-dynamic-async.html#异步组件)

同步方式引入

```js
import Login from './views/Login'
export default new Router({
  routes: [
    {
      path: '/',
      name: 'login',
      component: Login
    }
  ]
})
```

异步方式引入

```js
const Login = () => import('./views/Login')
export default new Router({
  routes: [
    {
      path: '/',
      name: 'login',
      component: Login
    }
  ]
})
```

必须使用 Vue Router 2.4.0+ 版本

异步方式引入的组件在需要的时候才从服务器加载，且会把结果缓存起来供未来重渲染，组件本身不需要做任何修改可以直接引用

利用此特性，我们便能做很多针对前端的优化
比如：针对大型项目，将页面核心功能打包成一个核心模块，通过框架优先加载。其他的一些周边功能打包后，通过服务器异步加载，从而解决业务需求越来越多导致的系统难维护、访问慢问题

也可以引入一个对象来处理异步组件的加载状态

```js
const Login = () => ({
  // 需要加载的组件
  component: import('./views/Login'),
  // 异步组件加载时使用的组件
  loading: LoadingComponent,
  // 加载失败时使用的组件(如果提供了超时时间且组件加载也超时了， 则使用该组件)
  error: ErrorComponent,
  // 展示加载时组件的延时时间。默认值是 200 (毫秒)
  delay: 200,
  // 超时时间,默认值是：`Infinity`
  timeout: 3000
})
```
