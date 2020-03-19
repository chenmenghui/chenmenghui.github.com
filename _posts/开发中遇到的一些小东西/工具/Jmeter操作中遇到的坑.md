---
title: JMeter 操作中遇到的坑
date: 2020-3-13
categories: JMeter
tags: 
- 测试
---

### Http 请求，从剪切板添加参数

「从剪切板复制」这个功能是不能识别从浏览器直接复制过来的json字符串的。

不过多了些转化，比一个个复制快得多了

```
rptfilters[countryregion.id]
rptfilters[region.id]
rptfilters[branch.id]
rptfilters[office.id]
rptfilters[month]	 2
rptfilters[runyear]	 2020
rptfilters[monthStart]	 0
rptfilters[runyearStart]	 0
rptfilters[monthEnd]	 0
rptfilters[runyearEnd]	 0
rptfilters[dateStart]
rptfilters[dateEnd]
rptfilters[currentsupercheck]	 1
rptfilters[plannedsupercheck]	 1
supervisor_select	 all
run_select	 all
rptfilters[building.id]_label
rptfilters[building.id]
hasactualbutnoplan
otheroption[PlanVisitHours]	 on
rptfilters[sortby]
page	 1
```

这样就行了。总结起来就是：
1. 删除所有的双引号
2. 把「:」转化成「(\t)」
3. ba「,」转化成「(\n)」

### Header请求头数据

同样也有「从剪切板复制」的功能。

复制请求头（firefox下需切换到原始头模式）就可以了。

