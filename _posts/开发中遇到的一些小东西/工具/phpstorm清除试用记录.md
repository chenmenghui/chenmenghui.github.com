---
title: phpstorm清除试用记录
date: 2020-3-19
categories: 
- 工具
tags: phpstorm
---

删除注册器和和配置文件中的一些记录数据就可以啦

可使用批处理

```cmd
reg delete "HKEY_CURRENT_USER\Software\JavaSoft\Prefs\jetbrains" /f
del /s/f/q "C:%HOMEPATH%\.PhpStorm2019.3\config\options\other.xml"
rd /s/q  "C:%HOMEPATH%\.PhpStorm2019.3\config\eval"
```

暂时对批处理还不太了解，所以「.PhpStorm2019.3」是写死的。

这点应该不难，不日解决。

按照这个思路，其实jetbrain全系的软件都可以这般「试用」了。

争取早日解决。现在批处理如何处理变量还是不知道，还是需要点东西的。