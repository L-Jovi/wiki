---
title: 去抖动（Debounce）
description: 用 JavaScript 实现 Debounce 功能
published: true
date: 2021-01-05T10:31:39.406Z
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

- 如果这个 `wait` 并不是 50ms，而是 500ms，用户每次频繁的交互，都会让下一次反馈等待 0.5s 才能被感知到，有没有办法可以照顾到用户的体验，在频繁的交互流程中，让下次触发的时间智能的根据上次触发的时间有效计算，而不是无限的延后固定的 0.5s 呢？
- 第一次调用，也需要等待 50ms 才能触发，用户如果只键入了一个字符，想要马上看到结果想必并不是过分的要求 :)
- 参考第一节的调用序列帧图，我们发现每一段序列都有**开始**和**结束**这两个明确的边界点（试想下你打字搜索内容的时候，大多数场景都是输入几个字，停下来然后再输入几个字），那么这个 `debounce` 功能是否可以明确指定触发的时间点在序列开始或结束的时候呢（目前我们实现的延迟器都是在结束的时候才触发调用）？
- 函数对于输入没有任何校验。

只是随便想了想，我们就发现了很多问题，目前的 `debounce` 是不折不扣的玩具代码，实际运行起来，怕是自己都看不下去。

不过，玩具代码正是抽象思维输出的关键逻辑，我们只需要更进一步，把事情做完整即可。

## 修正调用行为

我们参考开源项目 [lodash/lodash](https://github.com/lodash/lodash)[^2] 中实现的 debounce 功能[^3]

# 额外功能

[^1]: [Debouncing and Throttling Explained Through Examples | CSS-Tricks  ](https://css-tricks.com/debouncing-throttling-explained-examples/)
[^2]: [lodash/lodash: A modern JavaScript utility library delivering modularity, performance, & extras.](https://github.com/lodash/lodash)
[^3]: [lodash/debounce.js at master · lodash/lodash](https://github.com/lodash/lodash/blob/master/debounce.js)