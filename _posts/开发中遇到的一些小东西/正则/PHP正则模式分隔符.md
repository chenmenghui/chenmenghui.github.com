---
title: PHP模式分隔符
date: 2019-12-26
categories: 正则
tags: 
- 正则
- 实用代码片段
---

[分隔符](https://www.php.net/manual/zh/regexp.reference.delimiters.php)

说起来这个不是什么特别重要的东西，但不知道我为什么会在乎这个

```php
// Another rule
$aPatternMode2['tasksetid'] = "<input.*?name=\"unitinfo\[(?<unitinfoid>\d+)\]\[tasksetid\]\[(?<key>\d+)\]\".*?value=\"(?<value>.*?)\".*?>";
$aPatternMode2['islost'] = "<input.*?name=\"unitinfo\[(?<unitinfoid>\d+)\]\[islost\]\[(?<key>\d+)\]\".*?value=\"(?<value>.*?)\".*?>";
$aPatternMode2['sequenceOrder'] = "<input.*?name=\"unitinfo\[(?<unitinfoid>\d+)\]\[sequenceOrder\]\[(?<key>\d+)\]\".*?value=\"(?<value>.*?)\".*?>";
$aPatternMode2['schedule_set_id'] = "<input.*?name=\"unitinfo\[(?<unitinfoid>\d+)\]\[schedule_set_id\]\[(?<key>\d+)\]\".*?value=\"(?<value>.*?)\".*?>";
foreach ($aPatternMode2 as $key => $pattern) {
    if (preg_match_all($pattern, $sContent, $match, PREG_SET_ORDER)) {
        foreach ($match as $matchItem) {
            $aUnitinfo[$matchItem['unitinfoid']][$key][$matchItem['key']] = $matchItem['value'];
        }
    }
}
```
像这样，分隔符就别的可有可无了。我也是无意间发现的。