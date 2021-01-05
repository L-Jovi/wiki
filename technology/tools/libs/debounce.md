---
title: 去抖动（Debounce）
description: 用 JavaScript 实现 Debounce 功能
published: true
date: 2021-01-05T08:10:45.595Z
tags: javascript, tools, debounce
editor: markdown
dateCreated: 2021-01-05T08:10:04.873Z
---

# 什么是去抖动

去抖动（Debounce）是为了性能和交互体验考量，将高频的多个连续调用序列分组提取为单次调用的功能[^1]。

> The Debounce technique allow us to “group” multiple sequential calls in a single one.

可以在 CodePen 提供的[交互用例](https://codepen.io/dcorb/embed/KVxGqN?height=391&theme-id=1&slug-hash=KVxGqN&default-tab=result&user=dcorb&name=cp_embed_2)中体验，用关键帧记录动作调用的轨迹会呈现下图中的效果。

![debounce.png](/technology/tools/libs/debounce/debounce.png)

该功能在不同端均有实用场景，比如大家经常使用到的搜索引擎中界面输入框的实时下拉列表联想词展示。

![google-search.png](/technology/tools/libs/debounce/google-search.png =65%x)

# 实现核心功能

从功能分析，我们可以用一个计时器延迟触发调用，当有更多后续调用触发的时候，重置这个计时器。这样从结果上看，很多冗余的调用被忽略掉了，我们不断重置的计时器只会允许时间间隔较大的一些调用触发。

那么这个**重置计时器延迟调用**行为可以用 JavaScript 实现成下面的样子。

```
function debounce(func, wait = 50) {
  let timer = 0
  
  return function(...args) {
    if (timer) {
    	clearTimeout(timer)
    }
    timer = setTimeout(() => {
    	func.apply(this, args)
    }, wait)
  }
}
```

实际

# 完善细节

# 额外功能

[^1]: [Debouncing and Throttling Explained Through Examples | CSS-Tricks  ](https://css-tricks.com/debouncing-throttling-explained-examples/)