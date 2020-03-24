---
title: phpstorm清除试用记录
date: 2020-3-19
categories: 
- 工具
---

删除注册器和和配置文件中的一些记录数据就可以啦

可使用批处理

```cmd
rem For free evaluation jetbrain again
rem 2020-3-24 by chenmenghui
@echo off
setlocal enabledelayedexpansion

set year=%date:~0,4%
set month=%date:~5,2%
set day=%date:~8,2%
set /a currentDays=%year%*365+%month%*30+%day%
if exist last_date.txt (
    set /p lastDays=<last_date.txt
) else (
    set lastDays=0
)
set /a diff=%currentDays%-%lastDays%
:: 假如距上一次操作大于等于20天
if %diff% GEQ 20 (
    :: 删除相应的配置信息
    set folder=C:%HOMEPATH%\
    for /f "delims=" %%a in ('dir /ad /b !folder!') do (
        set str=%%a
        set needDel=0
        if /i "!str:~0,9!"==".PhpStorm" set needDel=1
        if /i "!str:~0,8!"==".PyCharm" set needDel=1
        if !needDel! EQU 1 (
            rd  "C:%HOMEPATH%\!str!\config\eval" /s /q
            del "C:%HOMEPATH%\!str!\config\options\other.xml" /f /s /q /a
        )
    )
    reg delete "HKEY_CURRENT_USER\Software\JavaSoft\Prefs\jetbrains" /f
    :: 记录本次操作时间
    echo %currentDays% > last_date.txt
)
```

其他的就交给开机自启啦。

说实在的，没有代码提示，踩了一堆坑。