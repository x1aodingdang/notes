# 总结的一些面试问题

## 浏览器

#### url 输入到页面显示全过程

`www.baidu.com`  
👇  
通过 `DNS` 找到 `www.baidu.com` ip 地址 14.215.177.39  
👇  
找到 14.215.177.39 这台电脑 返回一个 html 文件  
👇  
浏览器执行 html 加载 css js  
呈现页面

> 参考 https://segmentfault.com/a/1190000006879700

## Vue

#### vuex 包括哪些内容

- state
- actions
  > 异步的操作 一般获取 http 请求后 调用 `mutations` 中的方法 set
- mutations
  > 同步的 set
- getters
  > 如果需要从 state 中派生出一些状态 这里边可以对 state 中的一些数据进行过

> 参考 https://vuex.vuejs.org/zh/guide/

#### vue 项目优化

- 路由懒加载
- 第三方资源的按需引入(如: ui 框架)
- 使用 cdn 缓存
- gzip 压缩
- 图片体积再次压缩

真的太多啦...

> 参考 https://juejin.im/post/5d548b83f265da03ab42471d

#### computed 和 watch 区别

- computed

  - 返回值具有缓存效果
  - computed 会监听 函数体 所用到的值

- watch
  - watch 只监听一个值的改变

> 参考 https://juejin.im/post/5c9990d6f265da60ea146d21

#### vue-router 钩子介绍

有全局的钩子 也有针对个别钩子设置的 beforeEach

- `router.beforeEach`

  > 全局的 每一次路由跳转都会执行 因为有第三个参数 next 的存在 常用来做登录或者鉴权等操作

- `router.afterEach`

  > 在跳转之后

- routers 配置中 `beforeEnter`
  > 这是每一个配置中独享的 可以用来做数据的提前加载 获取 详情 404 登录鉴权等...

##### vue-router 完整的导航解析流程

1. 导航被触发: （也就是路由跳转了）
2. 在 `from` 的组件中 触发 `beforeRouteLeave`
3. 如果上一步没有 next(false) 则会调用 全局的 `beforeEach` 钩子
4. 在重用的组件里调用 [beforeRouteUpdate](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB) (v2.2+)
5. 调用 router 单个配置中的 `beforeEnter`
6. 解析异步路由 (请求懒加载的组件)
7. 在被激活的组件(`to`)里调用 `beforeRouteEnter`
8. 调用全局的 `beforeResolve` (v2.5+)
9. 导航被确认 (跳转成功啦)
10. 调用全局的 `afterEach`
11. 触发 DOM 更新 (页面要更新啦)
12. 最后调用 `beforeRouteEnter` 中传给 next 的回调函数 （这里和第 7 点不是同的, [参考](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E7%BB%84%E4%BB%B6%E5%86%85%E7%9A%84%E5%AE%88%E5%8D%AB)）

> 逻辑结束一定要 调用 next

> 参考 https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E5%89%8D%E7%BD%AE%E5%AE%88%E5%8D%AB

#### vue-router 懒加载实现

```js
new VueRouter({
  routes: [{ path: "/foo", component: () => import("/foo") }],
});
```
