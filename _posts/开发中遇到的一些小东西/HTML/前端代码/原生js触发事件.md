---
title: 原生js触发事件
date: 2018-12-18
categories: js
tags: 
- js运用
- 实用代码片段
---

遇到个视频在浏览器中不能播放的原因.后查询得知现在的chrome和safari不能直接播放带声音的声音[苹果与谷歌移动浏览器添加视频自动播放功能](https://m.huanqiu.com/r/MV8wXzk0Nzc2MDRfNjEyXzE0NzQ1OTMxODA=)

另附 [safari直接播放带音频的视频的配置方法](http://www.zanmeishi.com/help/20.html)

然后我就想着能不能利用jQuery中的trigger来实现视频音频自动播放
```html
<video autoplay="autoplay" loop="loop" id="bg-video" >
    <source src="image/upload/video/weizhen.mp4" type="video/mp4"></source>
</video>
<button id="test">点击播放按钮</button>
<script type="text/javascript">
    window.onload = function () {
        trigger();
    };

    document.getElementById('test').onclick = function () {
        alert('click');
        var res =  document.getElementById('bg-video').play();
    };

    function trigger(){
        if(document.all) {
            document.getElementById("test").click();
        }
        else {
            var e = document.createEvent("MouseEvents");
            e.initEvent("click", true, true);　　　　　　　　　　　//这里的click可以换成你想触发的行为
            document.getElementById("test").dispatchEvent(e);　　　//这里的clickME可以换成你想触发行为的DOM结点
        }
    }
</script>
```
可事实证明,这个投机方法是无用的.浏览器已经做了处理.依然必须要手动触发才行

