---
title: 如何给Hexo博客添加说说页面
author: uncleacc
avatar: >-
  https://dss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3616765171,3721318254&fm=26&gp=0.jpg
authorLink:
authorAbout: 一个好奇的人
authorDesc: 一个好奇的人
categories: 
comments: true
date: 2020-04-22 21:27:19
tags:
keywords:
description: 为你的博客添加说说界面
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/textbg/11.webp
---
# 前言
> 本文已经过期，说说已经更名为artitalk具体百度

最近看了许多大佬的博客，终于明白了我到底有多弱:weary:,不过虽然我菜，但是Chinese还是能看懂的:grin:,直接按照教程往下走，感谢把我教会的[原文1](https://cungudafa.gitee.io/post/ec85.html)和[原文2](https://cndrew.cn/2019/09/11/shuoshuo/)

>看看效果吧:


{% fb_img (https://img-blog.csdnimg.cn/20200703225932343.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0FHTklORw==,size_16,color_FFFFFF,t_70 %}
这个和QQ空间里面的说说类似，用来记录自己的生活以及心情都挺好的，请忽略内容里面的表情符号:sleeping:我太菜了，这些原本是要被转成表情的，但说说页面好像不支持，/手动流汗/，如果哪位大佬看到了这篇文章，祈求您留言指教我

好了，废话少说，正文开始：
# 步骤
1.在**themes\sakura\languages\zh-cn.yml**中增添定义：

    shuoshuo: 说说
2.修改导航栏，位置：themes\sakura\_config.yml增添：

    说说: {path: /shuoshuo/, fa: fa-commenting-o fa-commenting }
这里需要注意的是如果你的说说是添加在导航栏的子页面的，比如说在归档里面，那么需要在最后添加逗号( , )

3.在博客主目录下新建目录：

    hexo new page shuoshuo
路径**Users/用户名/博客文件夹/source**文件夹里就会出现刚刚新建的文件夹shuoshuo，打开文件shuoshuo，删除index文件夹，还有一个index.md文件,待会再来修改它。

4.Myblog\source\shuoshuo\ ，新建文件夹 **shuoshuo.css**

**shuoshuo.css**样式文件：
```
#shuoshuo_content {
    background-color: #fff;
    padding: 10px;
    min-height: 500px;
}
/* shuo */
 
body.theme-dark .cbp_tmtimeline::before {
    background: RGBA(255, 255, 255, 0.06);
}
 
ul.cbp_tmtimeline {
    padding: 0;
}
 
div class.cdp_tmlabel > li .cbp_tmlabel {
    margin-bottom: 0;
}
 
.cbp_tmtimeline {
    margin: 30px 0 0 0;
    padding: 0;
    list-style: none;
    position: relative;
}
/* The line */
 
.cbp_tmtimeline:before {
    content: '';
    position: absolute;
    top: 0;
    bottom: 0;
    width: 4px;
    background: RGBA(0, 0, 0, 0.02);
    left: 80px;
    margin-left: 10px;
}
/* The date/time */
 
.cbp_tmtimeline > li .cbp_tmtime {
    display: block;
    /* width: 29%; */
    /* padding-right: 110px; */
    max-width: 70px;
    position: absolute;
}
 
.cbp_tmtimeline > li .cbp_tmtime span {
    display: block;
    text-align: right;
}
 
.cbp_tmtimeline > li .cbp_tmtime span:first-child {
    font-size: 0.9em;
    color: #bdd0db;
}
 
.cbp_tmtimeline > li .cbp_tmtime span:last-child {
    font-size: 1.2em;
    color: #9BCD9B;
}
 
.cbp_tmtimeline > li:nth-child(odd) .cbp_tmtime span:last-child {
    color: RGBA(255, 125, 73, 0.75);
}
 
div.cbp_tmlabel > p {
    margin-bottom: 0;
}
/* Right content */
 
.cbp_tmtimeline > li .cbp_tmlabel {
    margin: 0 0 45px 65px;
    background: #9BCD9B;
    color: #fff;
    padding: .8em 1.2em .4em 1.2em;
    /* font-size: 1.2em; */
    font-weight: 300;
    line-height: 1.4;
    position: relative;
    border-radius: 5px;
    transition: all 0.3s ease 0s;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.15);
    cursor: pointer;
    display: block;
}
 
.cbp_tmlabel:hover {
    /* transform:scale(1.05); */
    transform: translateY(-3px);
    z-index: 1;
    box-shadow: 0 15px 32px rgba(0, 0, 0, 0.15) !important
}
 
.cbp_tmtimeline > li:nth-child(odd) .cbp_tmlabel {
    background: RGBA(255, 125, 73, 0.75);
}
/* The triangle */
 
.cbp_tmtimeline > li .cbp_tmlabel:after {
    right: 100%;
    border: solid transparent;
    content: " ";
    height: 0;
    width: 0;
    position: absolute;
    pointer-events: none;
    border-right-color: #9BCD9B;
    border-width: 10px;
    top: 4px;
}
 
.cbp_tmtimeline > li:nth-child(odd) .cbp_tmlabel:after {
    border-right-color: RGBA(255, 125, 73, 0.75);
}
 
p.shuoshuo_time {
    margin-top: 10px;
    border-top: 1px dashed #fff;
    padding-top: 5px;
}
/* Media */
 
@media screen and (max-width: 65.375em) {
    .cbp_tmtimeline > li .cbp_tmtime span:last-child {
        font-size: 1.2em;
    }
}
 
.shuoshuo_author_img img {
    border: 1px solid #ddd;
    padding: 2px;
    float: left;
    border-radius: 64px;
    transition: all 1.0s;
}
 
.avatar {
    border-radius: 100% !important;
    -moz-border-radius: 100% !important;
    box-shadow: inset 0 -1px 0 3333sf;
    -webkit-box-shadow: inset 0 -1px 0 3333sf;
    -webkit-transition: 0.4s;
    -webkit-transition: -webkit-transform 0.4s ease-out;
    transition: transform 0.4s ease-out;
    -moz-transition: -moz-transform 0.4s ease-out;
}
 
.zhuan {
    transform: rotateZ(720deg);
    -webkit-transform: rotateZ(720deg);
    -moz-transform: rotateZ(720deg);
}
/* end */
```

主页面：
```
---
title: shuoshuo
type: shuoshuo
noDate: 'true'
comments: 'false'
---
<link rel="stylesheet" href="./shuoshuo.css">
 
<div id="primary" class="content-area" style="">
    <main id="main" class="site-main" role="main">
        <div id="shuoshuo_content">
            <ul class="cbp_tmtimeline">
                <li> <span class="shuoshuo_author_img"><img src="https://cdn.jsdelivr.net/gh/cungudafa/cdn/img/custom/cungudafa.jpg" class="avatar avatar-48 zhuan" width="48" height="48"></span>
                    <a class="cbp_tmlabel" href="">
                        <p></p>
                        <p>想要开学，想吃火锅，想吃烧烤，想吃蟹肉煲，想吃鸡脚米线，想喝奶茶~</p>
                        <iframe frameborder="no" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1338809890&auto=1&height=66"></iframe>
                        <p></p>
                        <p class="shuoshuo_time"><i class="fa fa-clock-o"></i>
                            2020年2月25日
                        </p>
                    </a>
                </li>
                 <li> <span class="shuoshuo_author_img"><img src="https://cdn.jsdelivr.net/gh/cungudafa/cdn/img/custom/cungudafa.jpg" class="avatar avatar-48 zhuan" width="48" height="48"></span>
                    <a class="cbp_tmlabel" href="">
                        <p></p>
                        <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1582636990314&di=2421dcd34e1cc519b7f7f9559afbe7b1&imgtype=0&src=http%3A%2F%2Fpics1.baidu.com%2Ffeed%2Fb17eca8065380cd7531865282a19873258828151.jpeg%3Ftoken%3Dce6f76a2b9dc38c02c91acfc2a4bb8d8%26s%3D3C79EF14C510746516F547E003007036" height="200" width="100" />
                        <p>武汉加油！中国加油！</p>
                        <p></p>
                        <p class="shuoshuo_time"><i class="fa fa-clock-o"></i>
                            2020年2月25日
                        </p>
                    </a>
                </li>
                <li> <span class="shuoshuo_author_img"><img src="https://cdn.jsdelivr.net/gh/cungudafa/cdn/img/custom/cungudafa.jpg" class="avatar avatar-48 zhuan" width="48" height="48"></span>
                    <a class="cbp_tmlabel" href="">
                        <p></p>
                        <p>第一个说说</p>
                        <p></p>
                        <p class="shuoshuo_time"><i class="fa fa-clock-o"></i>
                            2020年2月25日
                        </p>
                    </a>
                </li>
            </ul>
        </div>
</div>
<script type="text/javascript">
    (function () {
        var oldClass = "";
        var Obj = "";
        $(".cbp_tmtimeline li").hover(function () {
            Obj = $(this).children(".shuoshuo_author_img");
            Obj = Obj.children("img");
            oldClass = Obj.attr("class");
            var newClass = oldClass + " zhuan";
            Obj.attr("class", newClass);
        }, function () {
            Obj.attr("class", oldClass);
        })
    })
</script>
```
只会报别人代码的我再次像大佬们深深鞠一躬(orz)