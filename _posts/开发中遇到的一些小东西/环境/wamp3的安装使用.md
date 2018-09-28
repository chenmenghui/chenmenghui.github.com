---
title: wamp3的安装使用
date: 2018-9-13 10:51:22
categories: 开发环境
tags: wamp的使用
---
# wamp3的安装使用

刚刚安装使用了wamp3,中间遇到了一些问题

### 缺少msvcr120.dll文件

这个百度解决的.就是在360人工服务查找这个问题,然后提示安装了什么工具集就好了.

### 无法配置默认文件路径

原来wamp3默认开启了v-host的服务,需在httpd-vhosts.conf文件中修改路径

### 修改路径后无法访问的问题

需修改httpd.conf文件
```apacheconfig
<Directory />
    AllowOverride none
    Require all denied
</Directory>
```
改成
```apacheconfig
<Directory />
    #AllowOverride none
    #Require all denied
    Options FollowSymLinks
    AllowOverride None
    Order deny,allow
    Deny from all
</Directory>
```

### 总是提示"c:/wamp64 or PHP in PATH"

现在的wamp3不提倡使用环境变量.使用环境变量对切换php版本有影响