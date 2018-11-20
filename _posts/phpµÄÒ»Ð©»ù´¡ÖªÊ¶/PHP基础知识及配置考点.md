---
title: PHP基础知识及配置考点
date: 2018-09-06
categories: 
- PHP
- PHP基础知识
tags: PHP基础知识
---
版本控制软件
- 集中式
    需要一个中央服务器,所有的代码都通过中央服务器进行管理
- 分布式
    每个客户端都有代码
    
- cvs和svn
- git
    
延伸:php的运行原理

nginx+php-fpm

CGI

FastCGI

PHP-FPM 是FastCGI的管理器

延伸: PHP常见配置项

- register-globals
    注入变量
- allow_url_fopen
    是否允许打开远程文件
- all_url_include
    运程包含文件
- data.timezone
    时区
- display_errors
    是否展示错误
- error_reporting
    展示错误级别
- safe_mode
    是否开启安全模式
- upload_max_filesize
    上传的最大文件大小
- max_file_uploads
    上传最大文件数量
- post_max_size
    post的最大文件大小
    
> 请简述CGI,FastCGI和PHP-FPM的区别


