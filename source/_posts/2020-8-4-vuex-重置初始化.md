---
layout: post
title: "vuex 重置初始化"
date: 2020-8-4 14:45:12
comments: true
tags: 
	- Vuex
---

因为用了`vuex`持久化插件 `vuex-persistedstate`  会把数据存进 `Local Storage` 里,刷新并不会自动初始化默认值,所以就需要自己手动初始化

<!-- more -->

这里在 `vuex` 生成的配置文件里 声明一个方法  return 出来 你需要初始化的 参数

```javascript
// 设置要重置的参数
const getDefaultState = () => {
  return {
    activeid: 0,
    user: {},
  }
}
```

然后需要用到 `mutations` 修改状态,在这里声明的方法,参数`state` 就是`vuex `的`state`,用`assign`方法会把`state`和`getDefaultState`方法里return出来的对象合并,如果存在会用后方对象的属性赋值,不存在就合并

```javascript
mutations: {
    resetState(state) {
      Object.assign(state, getDefaultState())
    }
  },
```

初始事只需要

```javascript
this.$store.commit("resetState");
```

