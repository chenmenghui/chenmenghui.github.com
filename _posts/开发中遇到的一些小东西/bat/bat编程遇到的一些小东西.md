---
title: bat编程遇到的一些小东西
date: 2020-1-18
categories: 
- 实用代码片段
- bat
---

> 这些实际运用的很少，需要的话肯定还是要百度的~~~写点遇到的吧，不知道的到时候再查呗

### 赋值

赋值号「=」两边的参数是不能有空格的！！！其他语言用习惯了，这一点很容易中枪

另外，从文件中读取值是这样的

```cmd
if exist opt_data.txt (
    set /p lastDays=<opt_data.txt
) else (
    set lastDay=0
)
```

### %~x0

bat 文件中像「%0」「%1」的含义就是命令行输入的命令的第几个参数。

比如在命令行输入

```cmd
1.bat a b c
```

其中
- %0 指的是「1.bat」
- %1 指的是「a」
- %2 指的是「b」
- %3 指的是「c」

而「~x」指的是取后缀

所以「%~x0」就是取「1.bat」的后缀，即「bat」

### for

[cmd for命令](https://blog.csdn.net/Ctrain/article/details/47917573)

```cmd
for /l %%i in (1 1 10) do (
    set var=%%i
    echo !var! 启用延缓环境变量
    echo %var% 未启用延缓环境变量
)
```

### 字符串截取




