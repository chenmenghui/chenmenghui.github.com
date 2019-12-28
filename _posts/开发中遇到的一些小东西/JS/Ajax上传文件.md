---
title: 获取get中的参数
date: 2019-12-23
categories: js
tags: 
- js运用
- 实用代码片段
---

除了使用ajaxForm,也可以这样做

```html
<!doctype html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <script src="jquery.js"></script>
</head>
<body>
<form action="" id="myForm" method="post" enctype="multipart/form-data">
    <input type="text" name="myText"><br>
    <input type="text" name="myText1"><br>
    <input type="file" name="image" id="image"><br>
    <input type="submit" id="submit">
</form>
<script>
    $(function () {
        $("#submit").click(function (e) {
            e.preventDefault();
            var formobj = document.getElementById("myForm");
            var formData = new FormData(formobj);
            formData.append('file', $('#image')[0].files[0]); // 注意,文件特殊
            var data = formData;
            $.ajax({
                url: "../ajaxForm/index.php",
                data: data,
                type: "Post",
                dataType: "json",
                cache: false,//上传文件无需缓存
                processData: false,//用于对data参数进行序列化处理 这里必须false
                contentType: false, //必须
                success: function (result) {
                    alert("上传完成!");
                },
            })
        })
    })
</script>
</body>
</html>
```
