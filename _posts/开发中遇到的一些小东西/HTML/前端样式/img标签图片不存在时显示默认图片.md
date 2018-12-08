---
title: img标签图片不存在时显示默认图片.md
date: 2018-11-21
categories: 
- css
tags: css
---

```html
<img src="img_url" onerror="this.src='default_url';this.onerror=null;">
```
注意`this.onerror=null`,可以避免在default_url不存在的情况下,陷入无限循环中