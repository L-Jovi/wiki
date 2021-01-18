---
title: Playground
description: 编辑本站常用的格式
published: true
date: 2021-01-18T07:51:42.025Z
tags: playground
editor: markdown
dateCreated: 2020-11-30T15:13:25.120Z
---

# Markdown

## 代码块

```
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('miao')
    resolve(1)
  }, 3000)
})
```

## 引用

> 首先，需要光。
  —— 海德喵喵

## 列表

- HiJack
  - coffee
  - music
- Hydra
  - game
  - anime
  
# 表情符号

`:apple:` will produce :apple:
`:dog`: will produce :dog:
`:leaves`: will produce :leaves:

# Tab 组

## Tabset {.tabset}

### Tab1
MiaoMiaoMiao

### Tab2
BulaBulaBula

# 链接

- [Google *google.com*](https://www.google.com)
- [Github *github.com*](https://github.com)
{.links-list}

# 提示区域

> Please say I love you :)
{.is-info}

> This module is only available from version **2.4 and up**.
{.is-warning}

> Oh no :(
{.is-danger}
