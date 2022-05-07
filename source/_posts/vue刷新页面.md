
---
title: vue_刷新当前页面的几种方法
---
第一个想到的方法是this.$router.push() , 但是会报错 , 不让自己页面跳转自己页面 , 就算解决了这个问题跳转了之后 , dom也不会渲染 , 页面的数据显示不出来 , 所以查询了一下方法来解决问题

#  location.reload()  不推荐 : 页面出现一瞬间的空白
1. 这个方法相当于( ctrl + R )刷新当前页面 , 但是刷新过后``vuex里面的值也会重新刷新`` , 当我们要实现多个组件根据 vuex 里面的值做相应的判断时 , 这个功能不能实现 , 于是我们结合``window.sessionStorage.getItem() 和 vuex ``以实现vuex中的值在本次会话的时候不会被刷新

```javascript
//freeze状态只出现一次 , 解封之后知道浏览器关闭都不在出现
this.$store.commit('changeFreeze')
//  1. dom没有渲染 , 我们要重新渲染dom 
location.reload()//这种刷新方式太耗能了,亟待优化
//  2. 强制刷新之后freeze还会出现-->把值保存在session中
window.sessionStorage.setItem('freeze',0)


以下是vuex中的
freeze: window.sessionStorage.getItem('freeze') || 1,

dom页面根据vuex中的freeze来判断显不显示
<div class="all0" v-if="this.$store.state.freeze===1">
```

# provide / inject 
这对选项是一起使用的 , 允许父组件向子孙组件注入一个依赖 , 如果子孙组件想要获取祖先组件的资源 , 就可以使用 inject 中的方法
provide : 一个对象 , 提供给子孙资源
inject : 一个组件 , 子孙组件调用父组件的值

使用provide / inject 来实现 当前页面刷新 , 组件自身刷新 , 并且重新渲染dom

父组件中编写provide向子组件提供资源 , 利用 v-if ,来显示app在dom树上存在与否 

```javascript
<template>
  <div id="app">
    <router-view v-if="isRouterAlive"/>
  </div>
</template>
<script>
export default {
  provide() {
    return {
      reload:this.reload
    }
  },
  data() {
    return {
      isRouterAlive:true
    }
  },
  methods: {
    reload(){
      this.isRouterAlive = false
      this.$nextTick(function(){
        this.isRouterAlive = true
      })
    }
  },
}
</script>
```

子组件中inject使用父组件提供的方法

```javascript
export default {
  //注入刷新依赖provide/inject
  inject:['reload'],
  methods:{
  //使用inject中的方法
  this.reload()
  }
}
```
# nextTick
在下次dom刷新之后调用它的回调函数


上面的nextTick的作用是在app刷新之后 , 重新给data中的 ``isRouterAlive 赋值true``
```javascript
this.$nextTick(function(){
        this.isRouterAlive = true
      })
```