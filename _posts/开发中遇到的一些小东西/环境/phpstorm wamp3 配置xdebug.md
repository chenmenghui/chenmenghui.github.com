---
title: phpstorm wamp3 配置xdebug
date: 2018/10/6 
categories: 开发环境
tags: 
- wamp的使用
- phpstorm
---

# phpstorm 配置xdebug

首先,需要知道的是php运行有两种模式,分别是命令行模式(cli)和浏览器访问模式(cgi).在wamp中,这两种方式使用的配置文件不同

## cli模式下的xdebug配置

如下图所示,首先看看这里面有没有在cli配置文件下配置好xdebug,这里展示的是已经配置好的.

![phpstorm自动识别xdebug配置](https://wx1.sinaimg.cn/mw690/00784LeJgy1fvyvfwhmflj31h70sstbb.jpg)

如果没有配置好,则需要自己配置.首先需要在wamp开启xdebug,如图

![wamp开启xdebug](https://wx1.sinaimg.cn/mw690/00784LeJgy1fvyvfxk3s0j31h70smb29.jpg)

开启之后,就可以在对应的地方找到xdebug的路径了.打开C:\wamp64\bin\apache\apache2.4.33\bin\php.ini(注意这里的安装位置),搜索xdebug就可以找到了(这就省的自己下载安装xdebug了)

注意,这里的开启的地方是apache使用的php.ini配置文件,和phpstorm使用的配置文件不是同一个.phpstorm使用的是php-cli模式中使用的php.ini配置文件.

这里有两者修改方式,一种是修改配置文件,另一种是修改phpstrom的配置

第一种方式:

可以在这里直接看到,点击编辑即可

![php-cli模式使用的php.ini配置文件位置](https://wx1.sinaimg.cn/mw690/00784LeJgy1fvyvfwmyavj31h70ss41a.jpg)

点击编辑之后,就可以直接修改啦

![配置xdebug](https://wx3.sinaimg.cn/mw690/00784LeJgy1fvyvfwqio0j31h70sstd1.jpg)

第二种方式:

直接把上述获得的xdebug的路径复制到上述第一张图中的debugger extension即可

## cgi下的配置

在wamp打开xdebug配置之后,chrome浏览器下载xdebug helper插件并启用,phpstorm开启监听debug功能(第四张图右上角一个类似电话的图标),然后在根据对应提示即可使用.

1. wamp打开xdebug配置
2. phpstorm开启监听debug功能(上面的第四张图右上角一个类似电话的图片)
3. 浏览器访问时添加debug session参数.或浏览器下载对应插件开启debug功能(如chrome浏览器使用xdebug helper)
4. 根据提示,配置映射路径