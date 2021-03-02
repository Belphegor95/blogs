---
layout: post
title: "hexo-yilia搭载Valine评论"
date: 2020-8-5 15:00:25
comments: true
tags: 
	- hexo
---

Valine一款快速、简洁且高效的无后端评论系统,理论上支持但不限于静态博客.

<!-- more -->

1.注册leancloud

[leancloud](https://www.leancloud.cn/)官网免费注册账号,然后创建应用,生成的应用对应的有`AppID`、`AppKey`

2.在博客主题文件添加评论配置设置

在`_config.yml`文件评论设置后面添加valine设置

```XML
valine: 
 appid:  AppID  #Leancloud应用的appId
 appkey: AppKey  #Leancloud应用的appKey
 verify: false #验证码
 notify: false #评论回复提醒
 avatar: robohash #评论列表头像样式：''/mm/identicon/monsterid/wavatar/retro/hide
 #头像类型可见： https://valine.js.org/avatar.html
 placeholder: 写下你的评论! #评论框占位符
```

3.在`article.ejs`文件中添加valine设置

在`themes\yilia\layout\_partial\article.ejs`文件中的`<% if (!index && post.comments){ %>`代码后面添加

```XML
<% if (theme.valine && theme.valine.appid && theme.valine.appkey){ %>
    <section id="comments" class="comments">
      <style>
        .comments{margin:30px;padding:10px;background:#fff}
        @media screen and (max-width:800px){.comments{margin:auto;padding:10px;background:#fff}}
      </style>
      <%- partial('post/valine', {
        key: post.slug,
        title: post.title,
        url: config.url+url_for(post.path)
        }) %>
  </section>
<% } %>
```

4.新建`valine.ejs`文件

新建在`themes\yilia\layout\_partial\post\valine.ejs`位置

```XML
<div id="vcomment" class="comment"></div> 
<script src="//cdn1.lncld.net/static/js/3.0.4/av-min.js"></script>
<script src="//unpkg.com/valine/dist/Valine.min.js"></script>
<script>
   var notify = '<%= theme.valine.notify %>' == true ? true : false;
   var verify = '<%= theme.valine.verify %>' == true ? true : false;
    window.onload = function() {
        new Valine({
            el: '.comment',
            notify: notify,
            verify: verify,
            app_id: "<%= theme.valine.appid %>",
            app_key: "<%= theme.valine.appkey %>",
            placeholder: "<%= theme.valine.placeholder %>",
            avatar:"<%= theme.valine.avatar %>"
        });
    }
</script>
```

设置好后,加载的样式如下


![评论](2020-8-5-hexo-yilia搭载Valine评论/Valine.jpg)



Valine还有许多其他的配置可以在leancloud的应用里设置,可参考[leancloud](https://www.leancloud.cn/)官网