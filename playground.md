---
title: Playground
description: 
published: true
date: 2020-12-24T17:09:59.751Z
tags: playground
editor: markdown
dateCreated: 2020-11-30T15:13:25.120Z
---

# Playground

下面给出编辑本站页面的常用格式。

## Markdown

### 代码块

```
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    console.log('miao')
    resolve(1)
  }, 3000)
})
```

### 列表

- HiJack
  - coffee
  - music
- Hydra
  - game
  - anime
  
## 表情符号

`:apple:` will produce :apple:
`:dog`: will produce :dog:
`:leaves`: will produce :leaves:

## Tab 组

### Tabset {.tabset}

#### Tab1
MiaoMiaoMiao

#### Tab2
BulaBulaBula

## 提示区域

> Please say I love you :)
{.is-info}

> This module is only available from version **2.4 and up**.
{.is-warning}

> Oh no :(
{.is-danger}
