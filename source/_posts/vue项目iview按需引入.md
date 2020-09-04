---
title: vue项目iview按需引入
date: 2020-08-18 15:53:59
comments: true
tags: 
	- iview
	- vue
---

第三方库一般很少需要全部组件,所以考虑到性能和项目大小最好还是按需引入比较好。借助插件 `babel-plugin-import`可以实现按需加载组件，减少文件体积。

<!-- more -->

1. 安装插件
   需要安装`iview`和`babel-plugin-import`插件,可以用`cnpm`安装比较快但是需要设置

```
$ npm install view-design --save
$ npm install babel-plugin-import --save-dev
```

2. 在`babel.config.js`中配置：
   
```
module.exports = {
  presets: ["@vue/cli-plugin-babel/preset"],
  "plugins": [["import", {
    "libraryName": "view-design",
    "libraryDirectory": "src/components"
  }]]
}
```

3. 可以直接在`main.js`里引入
   
```
import 'view-design/dist/styles/iview.css';  //按需引入需要引入样式
import { Button, Table } from 'view-design';
Vue.component('Button', Button);
Vue.component('Table', Table);
```

这里其实已经结束了,如果有强迫症可以参考下文


也可以声明外部文件引入,避免`main` 文件配置过多(强迫症,统一声明便于维护)
在`main.js`同级声明文件夹`plugins`在文件夹里创建文件`iview.js` 然后在文件里引入组件

```
import Vue from 'vue'
import { Button, Table } from 'view-design';
import 'view-design/dist/styles/iview.css';
Vue.component('Button', Button);
Vue.component('Table', Table);
```
然后在`main.js`文件里引入这个文件就可以了
```
import './plugins/iview'
```