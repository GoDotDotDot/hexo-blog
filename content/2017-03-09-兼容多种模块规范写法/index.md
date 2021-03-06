---
title: 兼容多种模块规范写法
path: blog/兼容多种模块规范写法
date: 2017-03-09
tags: [javascript]
categories: 2017·前端
description: 为了能够让同一个模块能够运行前后端（这里特指浏览器和Nodejs），兼容主流设计规范。模块开发者往往需要将自己的代码包装在一个函数闭包中。那么一般开发者是怎样做的呢？下面给出的示例能够兼容AMD、CMD以及CommonJS规范，能够同时运行在浏览器端和以Nodejs为后端语言的后端。
excerpt: 为了能够让同一个模块能够运行前后端（这里特指浏览器和Nodejs），兼容主流设计规范。
---

为了能够让同一个模块能够运行前后端（这里特指浏览器和Nodejs），兼容主流设计规范。模块开发者往往需要将自己的代码包装在一个函数闭包中。那么一般开发者是怎样做的呢？下面给出的示例能够兼容AMD、CMD以及CommonJS规范，能够同时运行在浏览器端和以Nodejs为后端语言的后端。

```javascript
(function(id,definition){
  // 判断是否是AMD或CMD
  var isAmdOrCmd = typeof define === 'function'
  // 判断是否是CommonJS
  var isCommonjs = typeof module !== 'undefined' && module.exports
  if(isAmdOrCmd){
    define(definition)
  }else if(isCommonjs){
    module.exports = definition()
  }else{
    // 这里的this在浏览器中为window，在node中为global
    this[id] = definition()
  }
})('demo',()=>{
  var demo = ()=>'this is a module demo'
  return demo
})
```

如果对三大主流规范不清楚的，可以查看我上一篇[《前端模块设计规范》](http://blog.godotdotdot.com/2017/03/08/%E5%89%8D%E7%AB%AF%E6%A8%A1%E5%9D%97%E8%AE%BE%E8%AE%A1%E8%A7%84%E8%8C%83/)博文。