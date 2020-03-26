---
title: git导出日志
date: 2020-3-26
categories: git
tags: git
---

```cmd
git log --date=iso --pretty=format:'"%h","%an","%ad","%s"' >log.csv
```
