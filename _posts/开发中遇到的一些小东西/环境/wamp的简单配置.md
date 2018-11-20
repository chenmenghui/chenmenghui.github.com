---
title: WAMP的一些基本配置
date: 2018-09-06 19:50:59
categories: 开发环境
tags: wamp的使用
---

# 环境变量的配置
把 C:\wamp64\bin\php\php5.6.31;C:\wamp64\bin\mysql\mysql5.7.19\bin 写进环境变量中即可.注意自己安装wamp的位置及各自后面的版本号
# 服务器设置
在wamp/bin/apache/Apache###/conf/httpd.conf文件中设置
# 根文件夹
修改documentroot和directory两项
保存后重启服务
# 404返回值
修改errordocument其后的值并删除前面的#
保存后重启服务
更改端口
修改或增加listen
保存后重启服务
注意格式，否则重启不了
# 设置外部访问
1.更改为一下内容（有两部分）

```
<Directory "E:/wamp/bin/apache/apache2.4.9/cgi-bin">
    AllowOverride None
    Options None
    Require all granted
</Directory>


#   onlineoffline tag - don't remove
    Require all granted
	allow from all
	
</Directory>
```

# 设置localhost默认显示页面
1.更改以下内容
```$xslt
<IfModule dir_module>
    DirectoryIndex index.php index.php3 index.html index.htm
</IfModule>
```
2.如果localhost文件夹中没有上面所包含的页面，地址栏输入localhost将显示localhost文件夹内的文件

# 取消php函数未定义之类的报错
在 php.ini 中设置
error_reporting = E_ALL & ~E_NOTICE
其中
&：和
~：非
## 注意
其他错误类型可以问度娘后根据需要利用逻辑判断进行取舍
# 修改MySQL默认编码格式
在my.ini中添加
[mysql]添加配置default-character-set=utf8
[mysqld]添加配置character_set_server=utf8
# 修改post上传文件大小
在 php.ini 中设置，搜索post后自行寻找……
修改服务器时间
php.ini文件里的 date.timezone = prc(中国时区)

# 建立第二站点
打开C:\wamp\bin\apache\apache2.4.9\conf\httpd.conf
去掉"Include conf/extra/httpd-vhosts.conf"的注释
去掉"LoadModule rewrite_module modules/mod_rewrite.so"的注释
httpd.conf文件修改，
```$xslt
AllowOverride all
Require all granted
allow from all
```
打开C:\wamp\bin\apache\apache2.4.9\conf\extra\httpd-vhosts.conf,添加
```$xslt
<VirtualHost *:80>
    ServerName blog
    DocumentRoot "E:/blog"
    <Directory "E:/blog">
    Options Indexes FollowSymLinks
    AllowOverride all
    Order Allow,Deny
    Allow from all
    </Directory>
</VirtualHost>
```
hosts 文件添加
文件位置：C:\Windows\System32\drivers\etc
127.0.0.1      blog

## 注意：

1. Options Indexes FollowSymLinks 如果没有，则禁止 Apache 显示该目录结构，那样就不会从浏览器看到该目录下的文件和子目录列表了（不过，服务器有默认优先打开index.html等文件的设置，如果有了此设置且当前目录下有index.html等文件，依旧无法打开此目录）。

2. 第一次输入第二站点时，需要在地址前面带上http://(不知道为什么)

3. 如果开启了第二或者更多的站点,第一个就是localhost能够访问的.这个是由如果还需要启用外部访问,记得要在第一个站点配置加上Allow from all(把上面的改下路径直接覆盖第一个就好了)

4. 如果想使用其他端口（如88）作为登陆，则直接在此文件中加上* Listen:88 *，并把对应的配置项改成* <VirtualHost *:88> *

5. 在阿里云上配置其他端口时，除了要配置防火墙，还要注意策略组
