---
title: 说说
type: artitalk
keywords: 咸鱼闲想
comments: 'false'
photos: https://cdn.jsdelivr.net/gh/uncleacc/Img/custom/4.webp
---
<!-- 说说 -->
<link rel="stylesheet" type="text/css" href="https://cdn.jsdelivr.net/gh/uncleacc/my_style/shuoshuo.css">
<script src="https://libs.baidu.com/jquery/2.0.0/jquery.min.js"></script>
<!-- 结束 -->
{% raw %}
<script>
var img = "https://cdn.jsdelivr.net/gh/uncleacc/cdn/img/custom/avatar.jpg"; //说说旁边显示的头像
var appID = "s3FE3ET00znFbcF4XOgNb9fn-MdYXbMMI";
var appKEY = "2XVblWCfnt2qfktiDHkYRgQA";
var per = "5"; //每页显示说说的数量
var username = "Uncleacc"; //Leancloud中设置的用户名
var placeholder1="只有Uncleacc才能评论哦"; //在编辑说说的输入框中的占位符
var placeholder2="请输入您的头像URL！";  //在编辑密码的输入框中的占位符
var lazy = 1; //是否开启懒加载动画
var bgimg = "https://gitee.com/cungudafa/source/raw/master/img/gif/katon/Totoro2.gif"; //背景动画
var atemoji_array={
        a:"https://cdn.jsdelivr.net/gh/drew233/cdn/caihongtiao.webp",
        b:"https://cdn.jsdelivr.net/gh/drew233/cdn/20200409130254.webp",
        c:"https://cdn.jsdelivr.net/gh/drew233/cdn/20200409130258.webp",
        d:"https://cdn.jsdelivr.net/gh/drew233/cdn/20200409130301.webp",
        e:"https://cdn.jsdelivr.net/gh/drew233/cdn/kawayi.webp",
        f:"https://cdn.jsdelivr.net/gh/drew233/cdn/20200409130304.webp"
}
</script>
{% endraw %}
<!-- <div id="artitalk_main"></div> -->
<div id="lazy"></div>
<div id="artitalk"></div>
<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/artitalk@v2.1.8"></script>