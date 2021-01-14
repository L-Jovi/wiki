---
title: Webpack 应用和原理
description: 前端工程化构建和打包工具
published: true
date: 2021-01-14T11:30:35.392Z
tags: webpack
editor: markdown
dateCreated: 2021-01-11T15:39:33.917Z
---

# 引入

不同于以往对页面展示交互和数据表现的开发，越来越完善的概念和技术被注入到目前的大前端生态中。

## 前端工程化

当下前端领域，逐渐被工程化[^1]一词衡量项目的完整性，主要指下面几个方面的综合作用，可以构建出相对高效的多人协作流程。

### 模块化

不同功能的文件拆分，组织为模块，可以进一步划分为

- 前端代码逻辑的模块化
- 资源和样式文件的模块化

> The FEE can be divided into separate packages covering different portions of the project.

### 组件化

用设计模式的思路拆分页面为组件，每个组件可以针对不同场景进一步做多态和复用

### 规范化

项目规范化，可以进一步划分为：
  
- 项目结构的合理划分
- 代码编写层面的语法和缩进规范
- 组件、函数和工具库的统一文档输出
- 合理的代码源版本控制逻辑
- 组件的配色和交互视觉逻辑

### 测试

结合不同的测试场景和意图，可以进一步划分为：

- 与后端网络交互获取的 RESTful API 数据结构和完整性测试
- 界面组件表现的正确性测试
- 交互到完整业务流程的功能性测试
- 端到端的冒烟测试

### 自动化

自动化流程，可以进一步划分为：

- 自动化打包流程（本地或线上打包）
- 单次提交的自动化本地语法检查和测试用例覆盖检测
- 通过脚本、平台和容器化技术自动化部署到不同环境
- 以及不同环境的自动化流水线持续集成，测试通过率、失败错误信息报告

### 环境隔离

将开发、测试、灰度、预发布和生产环境做隔离（具体区分几个环境视业务场景而定），并使流水线能够在不同的环境中做集成测试。

## 什么是 Webpack

那么 Webpack 和上一节提及的工程化又有什么关系？

试想一下，我们编写的 JavaScript，可能在不同的模块化[^2]和语言规范（ECMAScript）[^3]支持下，与不同开发组织形式的 CSS（Less、Scss）相互配合，克服诸多浏览器平台和兼容性问题，才能顺利输出展示为完整的前端页面。

而 Webpack 作为项目模块的打包工具，依赖丰富的生态插件支持，可以配合脚本处理前端项目依赖、组件和规范化等诸多问题，我们可以以较小的代价完成上述流程的运作。

![webpack.png](/technology/web/webpack/webpack.png =95%x)

# 核心功能

既然 Webpack 可以解决前端工程化的部分问题，接下来我们便结合文档，一步步理解该工具的使用方式，然后结合使用场景思考其原理和实现方法。

## 从一个简单的用例开始

从一个简单的示例[^4]开始理解 Webpack 如何组织并编译 JavaScript 实现的模块，最终输出到 HTML 页面中。

首先创建一块新的目录区域并安装必要的 Webpack 依赖和命令行工具，我们后面所有的用例都会基于[该项目](https://github.com/L-Jovi/latte-web/tree/master/build/webpack)迭代，这里先从 [`getting-started`](https://github.com/L-Jovi/latte-web/tree/master/build/webpack/getting-started) 开始。

```bash
mkdir webpack-demo && cd webpack-demo
npm init -y
npm install webpack webpack-cli --save-dev
mkdir getting-started && cd getting-started
```

添加 HTML 模板和源码文件同步为下面的目录结构。

```bash
.
├── dist
│   └── index.html
├── src
│   └── index.js
└── webpack.config.js

```

添加一些简单的 JavaScript 内容到 `index.js` 中，同时为了模拟与 JQuery 相似的外部体验，这里引入了 Lodash 依赖作为工具库。

```bash
npm install --save lodash
```

```js
import _ from 'lodash';

function component() {
  const element = document.createElement('div');
  element.innerHTML = _.join(['Hello', 'webpack'], ' ');
  return element;
}

document.body.appendChild(component());
```

虽然 Webpack 4 开始就支持无配置执行，为便于后面的功能迭代，我们继续添加配置 `webpack.config.js` 的内容。

下面的配置内容会使 Webpack 从 `./src/index.js` 开始作为项目入口开始寻找所有与此文件相关联的依赖，然后将 `src/index.js` 中的 ES6 规范逻辑转换为主流浏览器可以执行的 JavaScript 并输出到 `dist/bundle.js`。

```
const path = require('path')

module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  }
}
```

接着准备 HTML 模板，引入上述的 JavaScript 文件。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <title>Getting Started</title>
  </head>
  <body>
    <script src="./bundle.js"></script>
  </body>
</html>
```

最后使用 npx 调用 Webpack 命令运行。

```bash
$ npx webpack
```

打开 `dist/index.html`，可以看到上述的 JavaScript 逻辑和外部依赖 Lodash 均被 Webpack 组织到 HTML 模板中输出，打印出 `Hello Webpack`。

## 资源管理

上一节我们实现的项目已经可以翻译 ES6 规范的逻辑到浏览器可以执行的主流 JavaScript，并将依赖的外部模块 Lodash 一并打包到最终的 `bundle.js` 中，但是真实的项目环境，我们还需要处理样式表和图片等诸多类型的资源文件。

## 输出管理

## 代码切分

## 缓存

## 热模块替换

## 代码裁剪

## 填充

## 插件

# Webpack 原理

## AST

## Hooks

[^1]: [Front-end engineering - Wikipedia](https://en.wikipedia.org/wiki/Front-end_engineering)
[^2]: [Understanding (all) JavaScript module formats and tools](https://weblogs.asp.net/dixin/understanding-all-javascript-module-formats-and-tools)
[^3]: [ECMAScript - Wikipedia](https://en.wikipedia.org/wiki/ECMAScript)
[^4]: [Getting Started | webpack](https://webpack.js.org/guides/getting-started/)