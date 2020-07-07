---
title: hexo写作标签
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 技术
comments: true
date: 2020-05-09 19:42:26
tags: markdown
keywords:
description: 给你的写作增加标签
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/43.webp
---
<blockquote>
<p>
"我们在写作时不只是需要好的内容，简洁美丽的页面也是我们所要追求的，简洁的页面能够使读者更容易看懂，结构更清晰，这时我们就需要一些标签来辅助我们写作，以下内容转载于"
<a href="https://cndrew.cn/2020/04/18/mdjj/" target="_blank" rel="noopener">https://cndrew.cn/2020/04/18/mdjj/</a>
</p>
</blockquote>
<p class="p center cyan">下面只是我的一些记录也算是一种演示，需要JS和CSS文件，具体参考原文</p>

## JS
`复制下面代码到JS文件中，在../node_modules/hexo-generator-tag/index.js中调用`
```
function postFolding(args, content) {
  args = args.join(' ').split(',');
  let style = ''
  let title = ''
  if (args.length > 1) {
    style = args[0].trim()
    title = args[1].trim()
  } else if (args.length > 0) {
    title = args[0].trim()
  }
  if (style != undefined) {
    return `<details ${style}><summary> ${hexo.render.renderSync({text: title, engine: 'markdown'}).split('\n').join('')} </summary>
              <div class='content'>
              ${hexo.render.renderSync({text: content, engine: 'markdown'}).split('\n').join('')}
              </div>
            </details>`;
  } else {
    return `<details><summary> ${hexo.render.renderSync({text: title, engine: 'markdown'}).split('\n').join('')} </summary>
              <div class='content'>
              ${hexo.render.renderSync({text: content, engine: 'markdown'}).split('\n').join('')}
              </div>
            </details>`;
  }
}
hexo.extend.tag.register('folding', postFolding, {ends: true});
function postP(args) {
  args = args.join(' ').split(',')
  let p0 = args[0].trim()
  let p1 = args[1].trim()
  return `<p class='p ${p0}'>${p1}</p>`;
}
function postSpan(args) {
  args = args.join(' ').split(',')
  let p0 = args[0].trim()
  let p1 = args[1].trim()
  return `<span class='p ${p0}'>${p1}</span>`;
}
 
hexo.extend.tag.register('p', postP);
hexo.extend.tag.register('span', postSpan);
 
function postNote(args) {
  args = args.join(' ').split(',')
  if (args.length > 1) {
    let cls = args[0].trim()
    let text = args[1].trim()
    return `<div class="note ${cls}">${hexo.render.renderSync({text: text, engine: 'markdown'}).split('\n').join('')}</div>`;
  } else if (args.length > 0) {
    let text = args[0].trim()
    return `<div class="note">${hexo.render.renderSync({text: text, engine: 'markdown'}).split('\n').join('')}</div>`;
  }
}
 
function postNoteBlock(args, content) {
  return `<div class="note ${args.join(' ')}">
            ${hexo.render.renderSync({text: content, engine: 'markdown'}).split('\n').join('')}
          </div>`;
}
hexo.extend.tag.register('note', postNote);
hexo.extend.tag.register('noteblock', postNoteBlock, {ends: true});
```
## CSS
复制CSS代码在head.ejs文件中引用[点击我](https://cdn.jsdelivr.net/gh/drew233/css/newtag.css)  

~~总是记不住这些代码干脆记录下来直接CTRL C+CTRL V~~

## 源码
```
{% folding, 图片测试 %}
{% fb_img https://cdn.jsdelivr.net/gh/drew233/cdn/dreww.webp %}
{% endfolding %}
{% folding cyan open, 查看默认打开的折叠框 %}

这是一个默认打开的折叠框。

{% endfolding %}
{% folding green, 查看代码测试 %}

{% endfolding %}
{% folding yellow, 查看列表测试 %}

haha
hehe
{% endfolding %}
{% folding red, 查看嵌套测试 %}

{% folding blue, 查看嵌套测试2 %}

{% folding 查看嵌套测试3 %}

hahaha

{% endfolding %}
{% endfolding %}
{% endfolding %}
```


{% folding green, 查看代码测试 %}

{% endfolding %}
{% folding yellow, 查看列表测试 %}

haha
hehe
{% endfolding %}
{% folding red, 查看嵌套测试 %}

{% folding blue, 查看嵌套测试2 %}

{% folding 查看嵌套测试3 %}

hahaha

{% endfolding %}
{% endfolding %}
{% endfolding %}


{% folding, 图片测试 %}   
{% fb_img https://cdn.jsdelivr.net/gh/uncleacc/cdn/img/custom/avatar.jpg %}   
{% endfolding %}   
{% folding cyan open, 查看默认打开的折叠框 %}   

这是一个默认打开的折叠框。

{% endfolding %}   
{% folding green, 查看代码测试 %}   

{% endfolding %}   
{% folding yellow, 查看列表测试 %}   
* haha
* haha
{% endfolding %}

{% folding red, 查看嵌套测试 %}

{% folding blue, 查看嵌套测试2 %}

{% folding 查看嵌套测试3 %}

hahaha

{% endfolding %}  
{% endfolding %}  
{% endfolding %}  
## 源码
```
支持使用颜色参数：
clear，light，gray，red，yellow，green，cyan，blue

{% note red, 为简单的一句话提供的简便写法。 %}

{% note quote, 为简单的一句话提供的简便写法。 %}

{% note info, 为简单的一句话提供的简便写法。 %}

{% note warning, 为简单的一句话提供的简便写法。 %}

{% note done, 支持同样丰富的参数。 %}

{% note success, 支持同样丰富的参数。 %}

{% note danger, 支持同样丰富的参数。 %}

{% note error, 支持同样丰富的参数。 %}

{% note radiation, 支持同样丰富的参数。 %}

{% note bug, 支持同样丰富的参数。 %}

{% note idea, 支持同样丰富的参数。 %}

{% note link, 支持同样丰富的参数。 %}

{% note todo, 支持同样丰富的参数。 %}

{% note msg, 支持同样丰富的参数。 %}

{% note guide, 支持同样丰富的参数。 %}

{% note up, 支持同样丰富的参数。 %}

{% note undo, 支持同样丰富的参数。 %}

{% note paperclip, 支持同样丰富的参数。 %}
```

{% note red, 为简单的一句话提供的简便写法。 %}

{% note quote, 为简单的一句话提供的简便写法。 %}

{% note info, 为简单的一句话提供的简便写法。 %}

{% note warning, 为简单的一句话提供的简便写法。 %}

{% note done, 支持同样丰富的参数。 %}

{% note success, 支持同样丰富的参数。 %}

{% note danger, 支持同样丰富的参数。 %}

{% note error, 支持同样丰富的参数。 %}

{% note radiation, 支持同样丰富的参数。 %}

{% note bug, 支持同样丰富的参数。 %}

{% note idea, 支持同样丰富的参数。 %}

{% note link, 支持同样丰富的参数。 %}

{% note todo, 支持同样丰富的参数。 %}

{% note msg, 支持同样丰富的参数。 %}

{% note guide, 支持同样丰富的参数。 %}

{% note up, 支持同样丰富的参数。 %}

{% note undo, 支持同样丰富的参数。 %}

{% note paperclip, 支持同样丰富的参数。 %}

## 源码
```
{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。

{% span small, Test %}
{% span large, Test %}
{% span huge, Test %}
{% span ultra, Test %}
{% span ultra logo red, Test %}

{% p center logo large, Uncle_drew %}
{% p center small, Hand down,man down %}
```

{% span red, 红色 %}、{% span yellow, 黄色 %}、{% span green, 绿色 %}、{% span cyan, 青色 %}、{% span blue, 蓝色 %}、{% span gray, 灰色 %}。

{% span small, Test %}
{% span large, Test %}
{% span huge, Test %}
{% span ultra, Test %}
{% span ultra logo red, Test %}

{% p center logo large, Uncle_drew %}
{% p center small, Hand down,man down %}