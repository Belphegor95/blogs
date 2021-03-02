---
title: Vue中引入静态html文件
date: 2020-11-01 09:43:27
tags: 
    - vue
---

因为公司项目中有政策、协议之类的公示文件需要做成静态的引入到项目中,而vue有是单页应用,所以需要用到`iframe`来引入静态文件

<!-- more -->

1.main.js下层,home同层创建`iframe`入口文件,因为我有多个静态文件所以src不能写死
```html
<template>
  <div class="Policy">
    <iframe
      :src="html"
      width="100%"
      height="100%"
      frameborder="0"
      scrolling="auto"
    ></iframe>
  </div>
</template>
```

2.在public文件下放入你的静态html文件,因为项目打包的问题所以放这里,而且src文件要用到绝对路径引入

3.因为有在静态页面跳转功能,他自身是引入的模式,所以直接在html里跳转不合适,需要在html静态文件点击来通知`iframe`入口,切换src来实现跳转

4.在html文件里声明js,`postMessage`方法通知`iframe`入口
```javascript
document.getElementById("onSdk").onclick = function() {
    window.parent ? window.parent.postMessage({
            name: "Sdk"
    }, "*") : null
};
```
5.在`iframe`入口`mounted`函数里接收,在`watch`里监听路由参数跳转,因为单路由切换,我选择在路由参数作为浏览记录,用来实现用户后退前进时记录跳转位置
```javascript
mounted

window.onmessage = (e) => {
  if (e.data.name) {
    // 路由参数改变
    this.$router.push(`/policy?name=${e.data.name}`);
  }
};

watch

"$route.query.name"(val) {
  this.html = `/${val}.html`;
  // 因为本地引入 所以路由跳转 会导致回退后路由不更新  所有需要刷新下页面
  location.reload();
},
```