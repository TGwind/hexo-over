---
title: towxml渲染LaTeX公式
---
在使用towxml渲染LaTeX公式时，需要引入MathJax插件。具体步骤如下：

1. 在小程序的app.js文件中引入MathJax插件

```javascript
App({
  onLaunch: function () {
    // 引入MathJax插件
    requirePlugin("wxParsePlugin").wxParse("MathJax");
  }
})
```

2. 在需要渲染LaTeX公式的页面的wxml文件中引入MathJax插件

```html
<!-- 引入MathJax插件 -->
<import src="../../miniprogram_npm/towxml/extra/cxml/MathJax.wxml" />
<template is="MathJax" data="{{cxml:article.content}}" />
```

其中，`article.content`是需要渲染的内容。

3. 在需要渲染LaTeX公式的页面的js文件中引入towxml插件

```javascript
const towxml = require('../../towxml/main'); // 引入towxml插件
Page({
  data: {
    article: {}
  },
  onLoad: function () {
    // 将markdown转为towxml数据
    let md = `这是一段含有LaTeX公式的markdown文本：$\\sum_{i=1}^n i = \\frac{n(n+1)}{2}$`;
    let data = towxml(md, 'markdown');
    this.setData({
      article: data
    })
  }
})
```

在以上步骤完成后，即可在小程序中正确渲染LaTeX公式。
