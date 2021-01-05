---
title: 去抖动（Debounce）
description: 用 JavaScript 实现 Debounce 功能
published: true
date: 2021-01-05T08:44:06.553Z
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

我们默认一个延迟时间为 50ms，上面的实现返回一个未被执行的匿名函数，因此 `timer` 变量被闭包缓存起来不会被销毁。每次该匿名函数执行的时候，我们都可以判断当前是否存在一次未等到真正触发的调用，如果有，那就重设延迟器到 50ms，调用行为越频繁的触发，就会被计时器不断的延迟触发。

具体使用时，下面的 `input` 事件绑定的回调函数是 `debounce(userAction)`，也就是 `debounce` 返回而未被执行的匿名函数，当 `input` 事件被浏览器感知并触发的时候，匿名函数被调用，延迟计时器重置成功。

```
function userAction() {
	console.log('被扼制的疯狂输入')
}
    
const input = document.getElementById('#input')
input.addEventListener('input', debounce(userAction))
```

# 完善细节

上一节实现的逻辑，其实只不过是不断重设延迟时间，让调用行为不断等待，直到用户停下来的时候触发。

我们试想下面几个问题。

- 如果这个 `wait` 并不是 50ms，而是 500ms，用户每次频繁的交互，都会让下一次反馈等待 0.5s 才能被感知到，这个重设的行为是否有些太过生硬，有没有办法可以照顾到用户的体验，在频繁的交互流程中，让下次触发的时间智能的根据上次触发的时间有效计算，而不是无限的延后呢？
- 第一次调用，也需要等待 50ms 才能触发，用户如果只键入了一个字符，想要马上看到结果并不是过分的要求吧 :)
- 

# 额外功能

[^1]: [Debouncing and Throttling Explained Through Examples | CSS-Tricks  ](https://css-tricks.com/debouncing-throttling-explained-examples/)